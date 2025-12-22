# MCP Security Best Practices (2025 Update)

**Last Updated**: December 2025

With the release of MCP specification 2025-11-25 and widespread enterprise adoption, security and authorization have become critical concerns for FinOps teams deploying MCP servers. This guide covers the latest security features and best practices.

---

## 🔐 Key Security Updates in MCP 2025-11-25

### 1. Mandatory PKCE for OAuth Flows
**What changed**: Proof Key for Code Exchange (PKCE) is now **required** for all OAuth authorization flows.

**Why it matters**:
- Protects against authorization code interception attacks
- Essential when MCP servers are exposed over public networks or cloud-hosted
- Clients must verify PKCE support before proceeding with authorization

**Implementation**: Ensure your MCP client and authorization server both support PKCE (RFC 7636).

---

### 2. OAuth Client ID Metadata Documents (CIMD)
**What it is**: A new client registration method where the `client_id` is a URL pointing to a JSON metadata document.

**Benefits for FinOps**:
- Simplifies enterprise MCP deployments by eliminating Dynamic Client Registration (DCR)
- Clients specify their metadata via a public URL
- Authorization servers fetch and validate client information from this URL

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

**Use cases**:
- CI/CD cost validation pipelines
- Scheduled cost reports and anomaly detection
- Automated tagging compliance checks
- Nightly billing export processing

**Security considerations**:
- Store client credentials in secrets managers (AWS Secrets Manager, Azure Key Vault, GCP Secret Manager)
- Rotate credentials regularly (every 90 days recommended)
- Use least-privilege scopes for each automated workflow
- Audit all client credential usage via logging

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

**Benefits**:
- Eliminates repeated logins across AWS, Azure, GCP, Vantage, and other MCP servers
- Improves user experience for FinOps teams using multiple data sources
- Centralized authentication and authorization management

**Security best practices**:
- Use enterprise SSO (Okta, Azure AD/Entra ID, Google Workspace) as the identity provider
- Implement Just-In-Time (JIT) provisioning for MCP server access
- Enforce MFA for all cross-app access sessions
- Set short session lifetimes (e.g., 8 hours) and require re-authentication

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

**Best practices**:
- Default to least privilege (read-only for most operations)
- Require explicit user consent for step-up
- Log all step-up requests and approvals
- Set short TTLs on elevated tokens (15-60 minutes)

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
**For cloud-hosted (remote) MCP servers**:
- Use HTTPS/TLS 1.3 for all connections
- Implement IP allowlisting for enterprise clients
- Deploy behind VPCs with private endpoints where possible
- Use Web Application Firewalls (WAF) to protect against common attacks

**For local MCP servers**:
- Run on localhost only (127.0.0.1) unless remote access is required
- Use SSH tunneling for remote access instead of exposing ports
- Encrypt data at rest if caching billing data locally

---

### Principle 3: Secrets Management
**Never hardcode credentials**. Use:
- **AWS**: AWS Secrets Manager, Parameter Store
- **Azure**: Azure Key Vault
- **GCP**: Secret Manager
- **Multi-cloud**: HashiCorp Vault, 1Password, Doppler

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
**What to log**:
- All MCP server access (who, what, when)
- Authorization events (logins, step-ups, failures)
- Cost queries and data access patterns
- Failed authentication attempts
- Privilege escalations

**Tools**:
- **AWS**: CloudTrail, CloudWatch Logs
- **Azure**: Azure Monitor, Log Analytics
- **GCP**: Cloud Logging, Cloud Audit Logs
- **SIEM**: Splunk, Datadog, Elastic Security

**Alerting examples**:
- Alert on 5+ failed auth attempts in 10 minutes
- Alert on cost queries exceeding 1 year of data
- Alert on any write operations (tagging, budget modifications)
- Alert on MCP server access from unexpected IP ranges

---

### Principle 5: Data Privacy and Compliance
**Considerations**:
- **Where does cost data go?** ChatGPT/Claude send data to cloud AI services (OpenAI/Anthropic). Gemini sends to Google. Copilot sends to Microsoft.
- **Data residency**: Use Gemini (GCP), Copilot (Azure), or self-hosted MCP clients if data must stay in specific regions.
- **Compliance**: Ensure MCP deployments meet SOC 2, GDPR, HIPAA, or other regulatory requirements.
- **Data retention**: Configure MCP servers to delete cached billing data after defined periods.

**Best practice**: For highly sensitive cost data, use local or self-hosted MCP clients (VS Code, Claude Code) instead of cloud-based AI services.

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
