# Claude Code

**Download Claude Code**: https://claude.ai/download (includes Claude Code CLI)

Claude Code is Anthropic's official CLI and IDE integration for Claude, designed specifically for developers. It added remote MCP server support in June 2025, enabling connections to hundreds of external tools and data sources.

## MCP Support Timeline
- **Initial release**: Native MCP support from launch (Anthropic created MCP)
- **June 2025**: Remote MCP server support added, eliminating need for local server management
- **December 2025**: Enhanced with task workflows and advanced authorization from MCP spec 2025-11-25

---

✅ **Pros for a Cloud FinOps professional**
- **Developer-first**: Built for engineers and DevOps teams managing cloud infrastructure as code.
- **MCP-native**: Created by Anthropic (MCP's inventor), ensuring first-class protocol support.
- **Remote MCP servers**: Connect to cloud-hosted MCP servers without local setup or maintenance.
- **Multi-scope configuration**: Configure MCP servers at project, user, or enterprise levels.
- **Plugin ecosystem**: Pre-packaged MCP servers for common tools (Sentry, Linear, GitHub, etc.).
- **IDE integration**: Works directly in your development environment alongside infrastructure code.
- **Task workflows**: Support for long-running cost analyses and optimizations (spec 2025-11-25).
- **Enterprise controls**: Policy-based allowlists/denylists for MCP servers in managed environments.

⚠️ **Cons for a Cloud FinOps professional**
- **Technical barrier**: Requires CLI/IDE familiarity; not accessible to non-technical FinOps stakeholders.
- **Developer-centric**: UI/UX optimized for code, not cost reports or financial dashboards.
- **Subscription required**: Claude Pro or Enterprise needed for full MCP capabilities.
- **Limited visualization**: Text-based interface less suited for charts, graphs, and visual cost trends.
- **Setup complexity**: Configuring remote MCP servers, authentication, and permissions requires technical expertise.
- **Not a governance layer**: Lacks built-in approval workflows, audit trails, or FinOps-specific guardrails.

---

## MCP Configuration

Claude Code supports multiple MCP configuration methods:

### 1. Local MCP Servers (STDIO)

Create a configuration file at one of these locations:
- **Project-level**: `.claude/mcp.json` (project-specific servers)
- **User-level**:
  - macOS: `~/Library/Application Support/Claude/mcp.json`
  - Windows: `%APPDATA%\Claude\mcp.json`
  - Linux: `~/.config/Claude/mcp.json`

**Example configuration for AWS Pricing MCP Server:**

```json
{
  "mcpServers": {
    "aws-pricing": {
      "command": "uvx",
      "args": ["awslabs.aws-pricing-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "your-aws-profile",
        "AWS_REGION": "us-east-1",
        "FASTMCP_LOG_LEVEL": "ERROR"
      }
    },
    "aws-cost-explorer": {
      "command": "uvx",
      "args": ["awslabs.cost-explorer-mcp-server@latest"],
      "env": {
        "AWS_PROFILE": "your-aws-profile",
        "AWS_REGION": "us-east-1"
      }
    }
  }
}
```

### 2. Remote MCP Servers (SSE/HTTP)

For cloud-hosted MCP servers (available since June 2025):

```json
{
  "mcpServers": {
    "remote-finops-server": {
      "url": "https://mcp.finops-corp.example.com/sse",
      "transport": "sse",
      "headers": {
        "Authorization": "Bearer ${MCP_TOKEN}"
      }
    }
  }
}
```

### 3. Plugin MCP Servers

Install pre-packaged servers with one command:

```bash
# Install AWS pricing MCP plugin
claude mcp install awslabs/aws-pricing-mcp-server

# Install from MCP Registry
claude mcp install <server-name>
```

### 4. Enterprise Configuration

For managed environments with centralized control:

```json
{
  "managedMcp": {
    "allowlist": [
      "awslabs.aws-pricing-mcp-server",
      "awslabs.cost-explorer-mcp-server"
    ],
    "denylist": [
      "untrusted-server"
    ]
  }
}
```

Refer to [Claude Code MCP Documentation](https://docs.claude.ai/docs/model-context-protocol) for detailed setup instructions.

---

## FinOps Use Cases

**Recommended for:**
- DevOps/Platform Engineering teams managing cloud infrastructure as code
- Analyzing Terraform/CloudFormation templates for cost optimization opportunities
- Integrating FinOps checks into CI/CD pipelines
- Long-running cost analyses using task workflows (e.g., multi-region simulations)
- Technical FinOps practitioners comfortable with CLI and code-based workflows

**Not recommended for:**
- Non-technical finance or business stakeholders (use ChatGPT, Claude Desktop, or Copilot)
- Visual cost reporting and dashboards
- Quick demos or executive presentations

---

## Remote MCP Server Support (June 2025)

Claude Code's remote MCP capability eliminates the need to run MCP servers locally:
- **Cloud-hosted servers**: Connect to MCP servers running in AWS, Azure, GCP, or third-party platforms
- **No local management**: No need to install, update, or troubleshoot local MCP processes
- **SSE and HTTP transports**: Support for server-sent events and HTTP-based MCP servers
- **OAuth integration**: Secure authentication to remote servers using OAuth 2.0 and PKCE

This makes Claude Code ideal for enterprise FinOps teams deploying centralized, managed MCP infrastructure.

---

👉 **In short**: Claude Code + MCP is the best choice for technical FinOps practitioners and DevOps teams managing infrastructure as code, with powerful remote server support and task workflows — but requires developer skills and is less accessible to business users.
