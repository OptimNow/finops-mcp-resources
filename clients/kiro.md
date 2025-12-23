# Kiro

**Access Kiro**: https://kiro.dev

Kiro is an agentic AI IDE built on the Code OSS foundation (the same base as VS Code) that combines native MCP support with spec-driven development, agent hooks, and autonomous coding capabilities. Created with backing from AWS, Kiro targets developers who want AI assistance from prototype to production.

## MCP Support
- **Native MCP integration**: First-class support for Model Context Protocol from launch
- **Remote MCP servers**: Connect to cloud-hosted MCP servers without local installation
- **Kiro Powers**: Pre-packaged MCP tools and framework expertise loaded dynamically
- **Transport methods**: Supports both STDIO (local) and Streamable HTTP (remote)
- **Configuration levels**: Workspace-specific (`.kiro/settings/mcp.json`) and user-level (`~/.kiro/settings/mcp.json`)

---

✅ **Pros for a Cloud FinOps professional**
- **Developer-native**: Built on VS Code foundation, familiar to engineers managing infrastructure as code.
- **MCP-first design**: Native MCP integration with remote server support from day one.
- **Spec-driven workflows**: Translates natural language prompts into testable requirements (EARS notation).
- **Agent hooks**: Automate FinOps tasks on file save (e.g., cost validation, tagging checks, policy enforcement).
- **AWS integration**: Deep integration with AWS services, ideal for AWS-focused FinOps teams.
- **Claude Sonnet 4**: Powered by Anthropic's latest agentic model, excellent for complex cost analysis.
- **Free during preview**: Currently free with generous usage limits (50 agent interactions/month on free tier).
- **Database MCP support**: Strong support for querying billing databases (SQL Server, PostgreSQL, MySQL via MCP).
- **Steering files**: Customize AI behavior per project for consistent FinOps workflows.

⚠️ **Cons for a Cloud FinOps professional**
- **Technical barrier**: IDE-based interface requires developer skills; not accessible to non-technical stakeholders.
- **Early stage**: Still in public preview (as of 2025); some features may be incomplete or unstable.
- **AWS bias**: Strong AWS focus may limit multi-cloud FinOps use cases (Azure, GCP).
- **Usage limits**: Free tier caps at 50 agent interactions/month; Pro tier ($19/month) needed for heavy use.
- **Developer-centric UI**: Optimized for code, not cost dashboards or financial reporting.
- **Limited adoption**: Smaller user base compared to established tools (VS Code, Claude, ChatGPT).
- **Pricing uncertainty**: Preview pricing may change when generally available.
- **Not a governance layer**: Lacks built-in approval workflows, audit trails, or FinOps-specific guardrails.

---

## MCP Configuration

Kiro supports MCP servers through two configuration scopes:

### Workspace-Level (Project-Specific)
**Location**: `.kiro/settings/mcp.json`

```json
{
  "mcpServers": {
    "aws-pricing": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-aws-pricing"],
      "env": {
        "AWS_REGION": "us-east-1"
      }
    },
    "postgres-billing": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${secrets:billing-db-url}"
      }
    }
  }
}
```

### User-Level (Global)
**Location**: `~/.kiro/settings/mcp.json` (macOS/Linux) or `%USERPROFILE%\.kiro\settings\mcp.json` (Windows)

Same format as workspace-level, but applies to all Kiro projects for that user.

### Remote MCP Servers
```json
{
  "mcpServers": {
    "remote-cost-analysis": {
      "url": "https://mcp.finops-corp.example.com/sse",
      "transport": "http",
      "headers": {
        "Authorization": "Bearer ${secrets:mcp-token}"
      }
    }
  }
}
```

Refer to [Kiro MCP Documentation](https://kiro.dev/docs/mcp/) for detailed configuration.

---

## Kiro Powers (Pre-Packaged MCP Tools)

Kiro Powers provide a unified approach for MCP tools and framework expertise, packaged together and loaded dynamically. Instead of manually configuring individual MCP servers, you can activate Powers for common use cases.

**Example Powers relevant to FinOps**:
- **AWS Power**: Connect to AWS services (Cost Explorer, Pricing API, CloudWatch)
- **Database Powers**: Query billing exports in PostgreSQL, MySQL, SQL Server
- **GitHub Power**: Analyze infrastructure-as-code for cost implications

Activate via Kiro command palette: `Kiro: Enable Power`

---

## Spec-Driven Development for FinOps

Kiro's core innovation is spec-driven development, which translates natural language prompts into explicit, testable requirements using EARS notation (Easy Approach to Requirements Syntax).

**FinOps Example**:
```
User prompt: "Add cost tagging validation to our Terraform pipeline"

Kiro generates spec:
- WHEN a Terraform plan is created
- THE SYSTEM SHALL validate that all resources have required cost allocation tags
- IF tags are missing, THE SYSTEM SHALL fail the plan with a list of non-compliant resources
- WHEN validation passes, THE SYSTEM SHALL append cost estimates to the plan output
```

This structured approach ensures FinOps automation is testable, auditable, and meets real requirements.

---

## Agent Hooks for FinOps Automation

Agent hooks trigger AI agents on events like file save, running autonomously in the background.

**FinOps Use Cases**:
1. **On Terraform file save**: Validate cost allocation tags, estimate monthly costs, check for unattached EBS volumes
2. **On CloudFormation save**: Analyze for cost optimization opportunities (e.g., missing lifecycle policies)
3. **On billing export update**: Generate anomaly alerts, update cost dashboards, trigger Slack notifications
4. **On budget threshold**: Create incident tickets, notify stakeholders, suggest remediation

Configure via steering files (`.kiro/steering/`).

---

## FinOps Use Cases

**Recommended for:**
- **DevOps/Platform Engineering teams** managing infrastructure as code (Terraform, CloudFormation, CDK)
- **AWS-centric FinOps teams** leveraging AWS Cost Explorer, Pricing API, and billing databases
- **Technical FinOps practitioners** comfortable with IDE-based workflows
- **Cost optimization in CI/CD**: Automated cost validation before deploying infrastructure changes
- **Database-driven cost analysis**: Querying billing exports in PostgreSQL/MySQL via MCP servers
- **Spec-driven cost policies**: Translating FinOps requirements into testable specifications

**Not recommended for:**
- **Non-technical finance or business stakeholders** (use ChatGPT, Claude Desktop, or Copilot instead)
- **Visual cost reporting and dashboards** (Kiro is code-focused, not visualization-focused)
- **Multi-cloud teams with Azure/GCP focus** (AWS bias may limit utility)
- **Quick demos or executive presentations** (developer UI less polished than consumer AI tools)
- **Teams needing production-grade stability** (still in public preview as of 2025)

---

## Kiro vs Other MCP Clients

**Kiro vs VS Code**:
- Kiro is built on VS Code foundation but adds agentic capabilities, spec-driven development, and native MCP
- VS Code requires extensions for MCP; Kiro has it built-in
- Kiro has agent hooks and autonomous coding; VS Code relies on GitHub Copilot for AI

**Kiro vs Claude Code**:
- Both are developer-focused MCP clients with CLI and IDE integrations
- Claude Code is Anthropic's official CLI; Kiro is a full IDE experience
- Kiro adds spec-driven development and agent hooks; Claude Code is more lightweight
- Both support remote MCP servers

**Kiro vs ChatGPT/Claude Desktop**:
- Kiro is IDE-based for developers; ChatGPT/Claude are conversational for business users
- Kiro integrates with file systems, git, CI/CD; ChatGPT/Claude are standalone
- Kiro better for infrastructure-as-code; ChatGPT/Claude better for ad-hoc cost queries

---

## Pricing (as of 2025 Public Preview)

**Current Preview Pricing** (subject to change at general availability):

| Tier | Price/Month | Agent Interactions | Best For |
|------|-------------|-------------------|----------|
| **Free** | $0 | 50/month | Individual developers, experimentation |
| **Pro** | $19 | 1,000/month | Active FinOps practitioners |
| **Pro+** | $39 | 3,000/month | Heavy users, team leads |

**Note**: Agent interactions include MCP tool calls, code generation, spec creation, etc.

During public preview, Kiro is free with generous usage limits.

---

## Getting Started with Kiro for FinOps

1. **Download Kiro**: Visit [kiro.dev](https://kiro.dev) and install for your platform (macOS, Windows, Linux)
2. **Configure MCP servers**: Set up AWS Pricing, Cost Explorer, or billing database MCP servers in `.kiro/settings/mcp.json`
3. **Enable relevant Powers**: Activate AWS Power for Cost Explorer integration
4. **Create steering files**: Run `Kiro: Setup Steering for Project` and customize for FinOps workflows
5. **Set up agent hooks**: Configure hooks for Terraform/CloudFormation saves to validate cost tags
6. **Start spec-driven development**: Use natural language to define FinOps requirements, let Kiro generate testable specs

---

## AWS Integration

Kiro has strong AWS backing and integration:
- **AWS Built MCP support**: Integrated into Amazon Bedrock, Kiro, Strands, AgentCore, and Amazon Quick Suite
- **AWS blog coverage**: Featured in [AWS blog post on MCP for SQL Server professionals](https://aws.amazon.com/blogs/modernizing-with-aws/introducing-kiro-and-mcp-tools-for-sql-server-professionals/)
- **Cost Explorer MCP**: Native support for querying AWS cost data via MCP
- **CloudFormation/CDK support**: Analyze infrastructure-as-code for cost implications

This makes Kiro particularly attractive for AWS-first FinOps teams.

---

## Additional Resources

- [Kiro Official Website](https://kiro.dev)
- [Kiro MCP Documentation](https://kiro.dev/docs/mcp/)
- [Kiro Remote MCP Servers](https://kiro.dev/blog/introducing-remote-mcp/)
- [Kiro GitHub Repository](https://github.com/kirodotdev/Kiro)
- [AWS Blog: Kiro and MCP Tools](https://aws.amazon.com/blogs/modernizing-with-aws/introducing-kiro-and-mcp-tools-for-sql-server-professionals/)
- [Introducing Kiro Blog Post](https://kiro.dev/blog/introducing-kiro/)

---

👉 **In short**: Kiro is an agentic IDE with native MCP support, ideal for AWS-focused DevOps and Platform Engineering teams who want to integrate FinOps checks into infrastructure-as-code workflows — but requires developer skills and is still in public preview with evolving features.
