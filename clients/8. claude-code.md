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

1. **Plugin MCP Servers**: Pre-packaged servers with automatic setup (no manual config)
2. **Project-level config**: `.claude/mcp.json` for project-specific MCP servers
3. **User-level config**: `~/Library/Application Support/Claude/mcp.json` (macOS) or `%APPDATA%\Claude\mcp.json` (Windows)
4. **Enterprise config**: `managed-mcp.json` for centralized IT control with allowlists/denylists
5. **Remote servers**: Connect to cloud-hosted MCP servers via SSE or HTTP transports

Refer to [Claude Code MCP Documentation](https://code.claude.com/docs/en/mcp) for detailed setup.

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
