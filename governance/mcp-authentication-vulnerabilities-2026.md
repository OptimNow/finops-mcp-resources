# MCP Authentication Vulnerabilities & Remediation Guide

**Last Updated**: January 2026

The Model Context Protocol shipped without mandatory authentication. This document covers the known vulnerabilities, real-world incidents, and the security controls your organization must implement to protect MCP deployments.

---

## The Core Problem

MCP launched in November 2024 with authentication as an optional feature. Authorization frameworks arrived six months after widespread adoption. Research from Pynt shows that deploying just 10 MCP plugins creates a 92% probability of exploitation—with meaningful risk even from a single plugin.

The protocol's focus was on simplicity and ease of integration—not authentication and encryption. This design choice created the attack surface that organizations are now racing to close.

### Lessons from Clawdbot

Clawdbot—a popular AI assistant that automates tasks like email management and code operations—runs on MCP. When security researchers scanned the internet in January 2026, they found 1,862 MCP servers exposed with no authentication. They tested 119 servers. Every single one responded without requiring credentials.

Thousands of developers had deployed Clawdbot-connected MCP servers on internet-facing infrastructure without any security controls. The convenience that made MCP adoption so rapid also made its exposure catastrophic.

---

## Critical Vulnerabilities (CVEs)

Three critical vulnerabilities emerged in the six months following MCP's widespread adoption. Each exploits the same root cause: MCP's authentication was optional, and developers treated optional as unnecessary.

### CVE-2025-49596 (CVSS 9.4)
**Component**: Anthropic MCP Inspector

**What happened**: The MCP Inspector—a debugging tool from Anthropic—exposed unauthenticated access between its web UI and proxy server. An attacker could craft a malicious webpage that, when visited by a user running MCP Inspector, gained full access to the proxy server and all connected MCP resources.

**Impact**: Full system compromise via drive-by attack. Any developer debugging MCP connections became a target.

**Remediation**: Update MCP Inspector to the latest version. Never run debugging tools on production systems.

---

### CVE-2025-6514 (CVSS 9.6)
**Component**: mcp-remote OAuth proxy (437,000+ downloads)

**What happened**: Command injection in mcp-remote allowed attackers to take over systems by connecting to a malicious MCP server. The vulnerability existed in how mcp-remote processed the `authorization_endpoint` field—it passed the value directly to the system shell without sanitization.

**Attack vector**: An attacker hosts a malicious MCP server. When a victim's client connects, the server responds with a booby-trapped authorization endpoint. mcp-remote executes it as a shell command, achieving remote code execution.

**Impact**: Complete client-side compromise from connecting to any untrusted MCP server.

**Remediation**: Update mcp-remote immediately. Audit all MCP server connections. Only connect to trusted, verified MCP servers.

---

### CVE-2025-52882 (CVSS 8.8)
**Component**: Claude Code VS Code extensions

**What happened**: Popular Claude Code extensions exposed unauthenticated WebSocket servers on localhost. Any local process—or any webpage using WebSocket connections—could access files and execute code through the extension.

**Impact**: Arbitrary file access and code execution on developer machines.

**Remediation**: Update all Claude Code extensions. Audit extension configurations for exposed ports.

---

## Attack Patterns

Security researchers have documented several attack patterns that exploit MCP's architectural weaknesses.

### Tool Poisoning
A malicious MCP server provides tools with deceptive descriptions. The AI agent calls what it believes is a legitimate function, but the tool performs malicious actions—exfiltrating data, modifying files, or establishing persistence.

**Example**: Invariant Labs demonstrated that a malicious MCP server could silently exfiltrate a user's entire WhatsApp history by combining tool poisoning with a legitimate whatsapp-mcp server in the same agent configuration.

### Prompt Injection via Documents
An attacker embeds instructions in a document the AI agent processes. The agent follows the hidden instructions, performing actions the user never authorized.

**Example**: PromptArmor demonstrated a malicious document that manipulated an MCP-connected agent into uploading sensitive financial data to an attacker-controlled server.

### Cross-Server Tool Shadowing
A malicious MCP server intercepts calls intended for a trusted server by registering tools with similar names. The agent routes requests to the attacker's server instead of the legitimate one.

---

## Exposed Server Statistics

Equixly's analysis of popular MCP implementations found:

| Vulnerability Type | Prevalence |
|-------------------|------------|
| Command injection flaws | 43% |
| Unrestricted URL fetching | 30% |
| Path traversal (file leakage) | 22% |

These aren't edge cases. They're direct consequences of MCP's design decisions and the ecosystem's rapid, security-unconscious adoption.

---

## Implementing MCP Authorization

The MCP specification recommends OAuth 2.1 for authorization. While authorization is technically optional, it is strongly recommended when:

- Your server accesses user-specific data (emails, documents, databases)
- You need to audit who performed which actions
- Your server grants access to APIs that require user consent
- You're building for enterprise environments with strict access controls
- You want to implement rate limiting or usage tracking per user

### The Authorization Flow

When a client connects to a protected MCP server:

1. **Initial Handshake**: The server responds with `401 Unauthorized` and provides a pointer to its Protected Resource Metadata (PRM) document
2. **Metadata Discovery**: The client fetches the PRM to learn about the authorization server and supported scopes
3. **Authorization Server Discovery**: The client discovers the authorization server's capabilities via OIDC Discovery or OAuth 2.0 metadata endpoints
4. **Client Registration**: The client registers with the authorization server (either pre-registered or via Dynamic Client Registration)
5. **User Authorization**: The client opens a browser for user login and consent, receiving an authorization code
6. **Token Exchange**: The client exchanges the code for access and refresh tokens
7. **Authenticated Requests**: The client includes the access token in the `Authorization` header for all subsequent requests

### Local vs Remote MCP Servers

For MCP servers using **STDIO transport** (running locally), you can use environment-based credentials or credentials from third-party libraries embedded in the server. OAuth flows are primarily designed for **HTTP-based transports** where the MCP server is remotely hosted.

---

## Mandatory Security Controls

### 1. Authentication is Non-Negotiable

Every MCP server touching production systems needs authentication enforced at deployment.

**Implementation checklist**:
- [ ] Enable OAuth 2.1 with mandatory PKCE for all user-facing flows
- [ ] Use client credentials (OAuth `client_credentials` grant) for machine-to-machine automation
- [ ] Never deploy MCP servers with authentication disabled
- [ ] Audit existing deployments for unauthenticated access

---

### 2. Network Exposure Restrictions

Most exposed MCP servers are accidental. Developers spin up servers for local testing and forget to restrict network binding.

**Implementation checklist**:
- [ ] Bind MCP servers to localhost (127.0.0.1) unless remote access is explicitly required
- [ ] If remote access is needed, place servers behind VPCs with private endpoints
- [ ] Use IP allowlisting to restrict access to known corporate networks
- [ ] Deploy Web Application Firewalls (WAF) in front of internet-facing servers
- [ ] Scan your IP ranges for exposed MCP servers (port 3000-3100 are common defaults)

---

### 3. Assume Prompt Injection Will Succeed

MCP servers inherit the blast radius of the tools they wrap. If a server wraps cloud credentials, filesystems, or deployment pipelines, design access controls assuming the agent will be compromised.

**Implementation checklist**:
- [ ] Apply least-privilege permissions to all MCP server credentials
- [ ] Separate read and write permissions—read by default, write via step-up authorization
- [ ] Implement output filtering to prevent sensitive data from reaching untrusted destinations
- [ ] Log all tool invocations with full context for forensic analysis

---

### 4. Human Approval for High-Risk Actions

Treat the AI agent like a fast but literal employee who will do exactly what instructed—including things you didn't mean.

**Require explicit human confirmation before**:
- [ ] Sending external communications (email, Slack, API calls)
- [ ] Deleting any data
- [ ] Accessing credentials or secrets
- [ ] Modifying infrastructure or deployments
- [ ] Any action with financial impact

---

### 5. Inventory and Monitor MCP Exposure

Traditional endpoint detection sees MCP servers as normal Python or Node processes started by legitimate applications. You need tooling that specifically identifies MCP servers.

**Implementation checklist**:
- [ ] Inventory all MCP servers in your environment
- [ ] Monitor for new MCP server deployments
- [ ] Alert on MCP servers binding to non-localhost addresses
- [ ] Integrate MCP server logs with your SIEM
- [ ] Set up alerts for failed authentication attempts

---

## Common Implementation Pitfalls

From the official MCP documentation, avoid these mistakes:

- **Don't implement token validation yourself**. Use off-the-shelf, well-tested libraries. Rolling your own means you're likely to get it wrong.
- **Use short-lived access tokens**. If stolen, they limit the attacker's window.
- **Always validate tokens**. Just because your server received a token doesn't mean it's valid or meant for your server. Verify the audience (`aud`) claim matches your server.
- **Store tokens in secure, encrypted storage**. Implement robust cache eviction policies for expired tokens.
- **Enforce HTTPS in production**. Don't accept tokens over plain HTTP except for localhost during development.
- **Use least-privilege scopes**. Don't use catch-all scopes. Split access per tool or capability.
- **Don't log credentials**. Never log `Authorization` headers, tokens, codes, or secrets.
- **Return proper challenges**. On 401, include `WWW-Authenticate` with `Bearer`, `realm`, and `resource_metadata` so clients can discover how to authenticate.
- **Control Dynamic Client Registration (DCR)**. Unauthenticated DCR means anyone can register any client with your authorization server.

---

## Enterprise Remediation Checklist

Before your next security review, verify:

- [ ] **No unauthenticated MCP servers** in any environment (dev, staging, production)
- [ ] **All MCP dependencies updated** to patched versions
- [ ] **OAuth 2.1 with PKCE** enabled for all user-facing MCP flows
- [ ] **Network binding** restricted to localhost or private networks
- [ ] **Least-privilege IAM policies** applied to all MCP server credentials
- [ ] **Human approval gates** implemented for destructive or sensitive operations
- [ ] **Comprehensive logging** capturing all MCP tool invocations
- [ ] **Alerting configured** for suspicious patterns
- [ ] **Regular scanning** for exposed MCP servers on corporate networks
- [ ] **Incident response plan** updated to include MCP compromise scenarios

---

## Timeline of MCP Security Incidents

| Date | Incident | Impact |
|------|----------|--------|
| Nov 2024 | MCP launches without mandatory authentication | Insecure defaults established |
| Oct 2025 | Invariant Labs demonstrates WhatsApp exfiltration | Tool poisoning attack proven |
| Q4 2025 | Supabase Cursor agent compromise | Privileged access + untrusted input exploited |
| Jan 2026 | CVE-2025-49596 (MCP Inspector) | Full system compromise via debugging tool |
| Jan 2026 | CVE-2025-6514 (mcp-remote) | RCE via malicious MCP server |
| Jan 2026 | CVE-2025-52882 (Claude Code extensions) | Arbitrary file access on dev machines |
| Jan 2026 | Clawdbot exposure discovered (1,862 servers) | Widespread unauthenticated exposure confirmed |

---

## Sources and Further Reading

- [MCP Authorization Tutorial](https://modelcontextprotocol.io/docs/tutorials/security/authorization)
- [MCP Official Security Best Practices](https://modelcontextprotocol.io/specification/draft/basic/security_best_practices)
- [MCP Authorization Specification](https://modelcontextprotocol.io/specification/latest/basic/authorization)
- [Red Hat: MCP Security Risks and Controls](https://www.redhat.com/en/blog/model-context-protocol-mcp-understanding-security-risks-and-controls)
- [AuthZed: Timeline of MCP Security Breaches](https://authzed.com/blog/timeline-mcp-breaches)
- [Strobes: MCP Critical Vulnerabilities](https://strobes.co/blog/mcp-model-context-protocol-and-its-critical-vulnerabilities/)
- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [MCP Security Best Practices (This Repository)](./security-best-practices-2025.md)

### Related Standards

MCP authorization builds on these well-established standards:

- [OAuth 2.1](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-13) - The core authorization framework
- [RFC 8414](https://datatracker.ietf.org/doc/html/rfc8414) - Authorization Server Metadata discovery
- [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) - Dynamic Client Registration
- [RFC 9728](https://datatracker.ietf.org/doc/html/rfc9728) - Protected Resource Metadata
- [RFC 8707](https://datatracker.ietf.org/doc/html/rfc8707) - Resource Indicators

---

## Contributing

Found additional vulnerabilities or have remediation suggestions? Please open an issue or PR. For security vulnerabilities in this repository, contact the maintainers privately per [SECURITY.md](../SECURITY.md).

---

← [Back to Governance](./INDEX.md) | [Security Best Practices](./security-best-practices-2025.md) | [Back to Home](../README.md)
