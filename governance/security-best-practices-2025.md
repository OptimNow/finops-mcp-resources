# MCP Security Best Practices (2025 Update)

**Last Updated**: December 2025

With the release of MCP specification 2025-11-25 and widespread enterprise adoption, security and authorization have become critical concerns for FinOps teams deploying MCP servers. This guide covers the latest security features and best practices.

---

## 🔐 Key Security Updates in MCP 2025-11-25

### 1. Mandatory PKCE for OAuth Flows

**What changed**: Proof Key for Code Exchange (PKCE) is now **required** for all OAuth authorization flows in the 2025-11-25 specification.

**Why it matters**: PKCE protects against authorization code interception attacks—a critical vulnerability when MCP servers are exposed over public networks or cloud-hosted. The attack vector is simple: intercept the authorization code between the authorization server and client, then exchange it for an access token before the legitimate client can. PKCE prevents this by requiring the client to prove it initiated the authorization request. This is essential for enterprise deployments where MCP servers run in cloud environments rather than localhost.

**Implementation**: Ensure your MCP client and authorization server both support PKCE (RFC 7636). Most modern OAuth implementations include PKCE by default, but verify support before proceeding with authorization.

---

### 2. OAuth Client ID Metadata Documents (CIMD)

**What it is**: A new client registration method where the `client_id` is a URL pointing to a JSON metadata document.

**Benefits for FinOps**: CIMD dramatically simplifies enterprise MCP deployments by eliminating the complexity of Dynamic Client Registration (DCR). Instead of a multi-step registration dance, clients simply specify their metadata via a stable public URL. Authorization servers fetch and validate client information directly from this URL, reducing deployment friction for IT teams. This means you can onboard new MCP clients without manual registration processes or back-and-forth with security teams.

**Example**:
```json
{
  "client_id": "https://finops-corp.example.com/mcp-client.json",
  "redirect_uris": ["https://finops-corp.example.com/oauth/callback"],
  "client_name": "FinOps MCP Client",
  "logo_uri": "https://finops-corp.example.com/logo.png"
}
```

**Best practice**: Host your CIMD at a stable, versioned URL and use HTTPS with valid certificates.

---

### 3. OAuth Client Credentials (Machine-to-Machine)

**What it enables**: Automated FinOps workflows without user interaction.

**Use cases**: Client credentials unlock the automation workflows that make FinOps scalable. CI/CD pipelines can validate cost impacts before merging code. Scheduled jobs generate cost reports and detect anomalies overnight. Compliance scanners check tagging adherence across thousands of resources. Billing export processors transform raw data into actionable insights—all without requiring a human to manually authenticate.

**Security considerations**: The power of automation comes with security responsibilities. Store client credentials in dedicated secrets managers like AWS Secrets Manager, Azure Key Vault, or GCP Secret Manager—never in code or configuration files. Rotate credentials every 90 days to limit exposure windows. Apply least-privilege scopes for each workflow so a compromised credential can't escalate beyond its intended purpose. Finally, audit all client credential usage via comprehensive logging to detect misuse before it becomes a breach.

**Example IAM policy** (AWS):
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {"Service": "mcp-automation.example.com"},
    "Action": "sts:AssumeRole",
    "Condition": {
      "StringEquals": {"sts:ExternalId": "unique-external-id"}
    }
  }]
}
```

---

### 4. Cross-App Access (Single Sign-On)

**What it is**: Sign in once through an MCP client, gain access to all authorized MCP servers.

**Benefits**: Cross-app access eliminates the authentication fatigue that plagued early MCP deployments. FinOps teams juggling AWS, Azure, GCP, Vantage, and other data sources no longer authenticate separately for each. One login through your MCP client grants access to every authorized server, dramatically improving user experience. Centralized authentication and authorization management also reduces IT overhead—provision once, secure everywhere.

**Security best practices**: The convenience of cross-app access demands robust security controls. Use enterprise SSO providers like Okta, Azure AD/Entra ID, or Google Workspace as your identity source, leveraging their battle-tested authentication infrastructure. Implement Just-In-Time (JIT) provisioning so users only gain MCP server access when they need it, not permanently. Enforce MFA for all cross-app sessions—single sign-on shouldn't mean single-factor authentication. Finally, set short session lifetimes (8 hours recommended) and require re-authentication to limit the blast radius of compromised credentials.

**Architecture**:
```
User → MCP Client (Claude Code/ChatGPT/Copilot)
         ↓ (OAuth + CIMD)
       Enterprise SSO (Okta/Azure AD)
         ↓ (Issues tokens)
       MCP Servers (AWS/Azure/GCP/Vantage)
```

---

### 5. Step-Up Authorization
**What it is**: Request elevated permissions only when needed, during runtime.

**FinOps example**:
- **Read-only by default**: Query cost data, view billing exports
- **Step-up when needed**: Write cost allocation tags, modify budgets, create savings plans

**Implementation**:
```
1. User queries cost data → uses read-only token
2. User attempts to tag resources → insufficient permissions
3. MCP server triggers step-up authorization flow
4. User authenticates with elevated privileges (e.g., MFA + manager approval)
5. Server issues time-limited write token
6. Tagging operation completes
7. Token expires, returns to read-only
```

**Best practices**: Design your authorization model around step-up from the start. Default to least privilege with read-only access for most operations, only elevating when necessary. Require explicit user consent for step-up—never silently escalate privileges. Log every step-up request and approval for compliance and forensics. Most importantly, set short TTLs on elevated tokens (15-60 minutes) so that write access automatically reverts to read-only after the operation completes.

---

## 🛡️ General MCP Security Principles for FinOps

### Principle 1: Least Privilege Access
**Apply to**:
- IAM policies for cloud provider access (see [security-privileges-aws.md](./security-privileges-aws.md))
- MCP server permissions and tool scopes
- User roles and authorization levels

**Example**: An MCP server for cost reporting should have `ce:GetCostAndUsage` but NOT `ce:UpdateCostAllocationTagsStatus`.

---

### Principle 2: Network Security

**For cloud-hosted (remote) MCP servers**: Network security is your first line of defense. Use HTTPS/TLS 1.3 for all connections—never expose MCP servers over plain HTTP, even for internal development. Implement IP allowlisting to restrict access to known corporate networks, reducing your attack surface. Deploy behind VPCs with private endpoints wherever possible, keeping MCP servers off the public internet entirely. Finally, place Web Application Firewalls (WAF) in front of your servers to protect against common attacks like SQL injection and cross-site scripting.

**For local MCP servers**: Local servers have different threat models but still require protection. Run on localhost only (127.0.0.1) unless remote access is explicitly required. When remote access is necessary, use SSH tunneling instead of exposing ports directly to the network. If your local server caches billing data, encrypt it at rest to protect against disk theft or unauthorized filesystem access.

---

### Principle 3: Secrets Management

**Never hardcode credentials**—not in code, not in configuration files, not in environment variables committed to git. Use dedicated secrets managers designed for secure credential storage and rotation. AWS teams should use AWS Secrets Manager or Parameter Store. Azure teams have Azure Key Vault. GCP provides Secret Manager. For multi-cloud environments, consider HashiCorp Vault, 1Password, or Doppler for unified secrets management across providers.

**MCP client configuration example**:
```json
{
  "mcpServers": {
    "aws-cost": {
      "command": "mcp-aws-cost",
      "env": {
        "AWS_ACCESS_KEY_ID": "${secrets:aws-mcp-key-id}",
        "AWS_SECRET_ACCESS_KEY": "${secrets:aws-mcp-secret}"
      }
    }
  }
}
```

---

### Principle 4: Audit and Monitoring

**What to log**: Comprehensive logging is non-negotiable for enterprise MCP deployments. Capture all MCP server access with full context: who accessed what data, when, and from where. Log authorization events including logins, step-up requests, and failures. Track cost queries and data access patterns to detect anomalous behavior. Record failed authentication attempts and privilege escalations. This telemetry forms the foundation of your security posture and compliance evidence.

**Tools**: Leverage your cloud provider's native logging infrastructure. AWS teams should use CloudTrail for audit logs and CloudWatch Logs for application logs. Azure offers Azure Monitor and Log Analytics for unified visibility. GCP provides Cloud Logging and Cloud Audit Logs. For centralized monitoring across multi-cloud environments, integrate with a SIEM like Splunk, Datadog, or Elastic Security.

**Alerting examples**: Convert logs into actionable alerts. Trigger on 5+ failed authentication attempts within 10 minutes to catch credential stuffing attacks. Alert on cost queries exceeding 1 year of data—legitimate queries rarely need this much history. Flag any write operations like tagging or budget modifications for immediate review. Most importantly, alert on MCP server access from unexpected IP ranges, which often indicates compromised credentials or unauthorized access.

---

### Principle 5: Data Privacy and Compliance

**Considerations**: The data flow question dominates enterprise MCP discussions. When you query cost data through ChatGPT or Claude, that data flows to OpenAI or Anthropic's cloud infrastructure. Gemini sends data to Google. Copilot routes through Microsoft. This matters for regulated industries and data sovereignty requirements.

Data residency adds another layer of complexity. If your cost data must remain in specific geographic regions, choose cloud-aligned MCP clients: Gemini for GCP deployments, Copilot for Azure, or self-hosted options for complete control. Ensure your MCP architecture meets SOC 2, GDPR, HIPAA, or whatever regulatory frameworks govern your organization. Configure MCP servers to automatically delete cached billing data after defined retention periods, reducing your compliance footprint.

**Best practice**: For highly sensitive cost data—especially in regulated industries like healthcare or finance—use local or self-hosted MCP clients like VS Code with Cline or Claude Code instead of cloud-based AI services. This keeps your billing data on infrastructure you control, eliminating third-party data processor relationships.

---

## 🚀 Enterprise Deployment Checklist

Before deploying MCP servers in production for FinOps:

- [ ] **Use dedicated IAM users/roles** for MCP servers (not personal credentials)
- [ ] **Apply least-privilege policies** (read-only by default)
- [ ] **Enable PKCE** for all OAuth flows
- [ ] **Implement cross-app access** via enterprise SSO
- [ ] **Configure step-up authorization** for write operations
- [ ] **Store credentials in secrets managers** (never hardcode)
- [ ] **Enable comprehensive logging** (CloudTrail, Azure Monitor, Cloud Logging)
- [ ] **Set up alerts** for suspicious activity
- [ ] **Review data privacy implications** of cloud AI services
- [ ] **Document MCP architecture** for security reviews
- [ ] **Establish credential rotation policies** (90-day recommended)
- [ ] **Test authorization failures** (verify step-up flows work correctly)
- [ ] **Validate network security** (HTTPS, IP allowlisting, VPCs)

---

## 📖 Additional Resources

- [MCP Specification 2025-11-25 - Authorization](https://modelcontextprotocol.io/specification/2025-11-25)
- [AWS MCP IAM Policies](./security-privileges-aws.md)
- [MCP Authorization Spec Details](https://den.dev/blog/mcp-november-authorization-spec/)
- [Client Registration and Enterprise Management](https://aaronparecki.com/2025/11/25/1/mcp-authorization-spec-update)
- [MCP Enterprise Readiness](https://subramanya.ai/2025/12/01/mcp-enterprise-readiness-how-the-2025-11-25-spec-closes-the-production-gap/)

---

## 🤝 Contributing

Found a security issue or have suggestions? Please open an issue or PR. For security vulnerabilities, contact the repository maintainers privately.
