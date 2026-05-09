# AWS MCP Servers for Cloud Cost Optimization

**Last Updated**: May 2026

A comprehensive guide to AWS MCP servers for FinOps automation, cloud cost optimization, and AI-powered cost management. Covers the AWS MCP Server (15,000+ APIs, GA on May 6, 2026), Pricing API, Cost Explorer, CloudWatch, Billing & Cost Management, and community FinOps servers.

← [Back to Servers](./INDEX.md) | [Home](../README.md) | [Azure Servers](./azure.md) | [GCP Servers](./gcp.md)

---

## ⭐ AWS MCP Server — Generally Available (May 6, 2026)

**Official AWS Documentation**: [Agent Toolkit for AWS — User Guide](https://docs.aws.amazon.com/agent-toolkit/latest/userguide/mcp-server.html)
**Announcement**: [The AWS MCP Server is now generally available](https://aws.amazon.com/blogs/aws/the-aws-mcp-server-is-now-generally-available/)
**Local proxy** (used by MCP clients): [aws/mcp-proxy-for-aws](https://github.com/aws/mcp-proxy-for-aws) — a small open-source helper that bridges STDIO to the remote HTTPS endpoint and signs requests with SigV4 using your local AWS credentials. Distributed via `uvx mcp-proxy-for-aws@latest`; no manual install.
**Status**: Generally Available (GA)

### 🎯 Start Here for Complete AWS Interactions

The AWS MCP Server is the managed, remote MCP entry point for AWS — one endpoint, 15,000+ AWS APIs, no local server installation. As of GA, it ships inside the broader **Agent Toolkit for AWS**, which bundles the MCP Server, **Skills**, **Plugins**, and project-level **Rules files**.

#### What changed at GA

For FinOps users, three changes matter most:

1. **Simpler IAM model.** The previous `aws-mcp:InvokeMcp`, `aws-mcp:CallReadOnlyTool`, and `aws-mcp:CallReadWriteTool` permissions are gone — they no longer have any effect, and AWS recommends removing them. Access is now expressed through standard IAM policies on the underlying AWS actions (`ce:GetCostAndUsage`, `pricing:GetProducts`, etc.). Two global condition context keys are automatically added to every MCP-initiated request: `aws:ViaAWSMCPService` (Boolean, `true` for any AWS managed MCP server) and `aws:CalledViaAWSMCP` (String, the specific MCP service principal — e.g. `aws-mcp.amazonaws.com`). The AWS-documented pattern is to use them in `Deny` statements when the same IAM identity is shared between human and agent use, so the agent stays narrower than the human.
2. **`run_script` tool.** A short Python script runs server-side in an isolated sandbox, inherits the caller's IAM permissions, has no network egress outside AWS, and lets the agent chain multiple AWS calls in a single round-trip. For multi-step FinOps work — month-over-month growth analysis, commitment portfolio reviews, multi-account aggregation — this avoids the round-trip overhead of issuing one `call_aws` per API operation.
3. **Documentation tools require no authentication.** `search_documentation` and `read_documentation` work without IAM credentials. Only `call_aws` and `run_script` need IAM.

`call_aws` itself also now supports file uploads and long-running operations, which makes it usable for tasks that previously needed bespoke tooling.

#### Capabilities

**One endpoint, 15,000+ AWS APIs**
- `call_aws` — executes any AWS API operation with the caller's IAM credentials; supports file uploads and long-running operations
- `run_script` — runs a short Python script in a server-side sandbox; no network access outside AWS; inherits IAM permissions; ideal for chaining multiple API calls
- `search_documentation` and `read_documentation` — retrieve current AWS documentation; no authentication required
- `retrieve_skill` — loads a specific Skill on demand so the agent only consumes context it actually needs

When listed by an MCP client, these tools appear with an `aws___` prefix (e.g. `aws___call_aws`).

**Skills** (replacing the previous Agent SOPs)
- Curated packages of instructions, scripts, and reference material for specific AWS tasks
- Loaded on demand so they do not consume unnecessary context
- **Contributed and maintained by the individual AWS service teams** that own each domain — meaning the guidance reflects current best practice for each service rather than a generic template

**Regional endpoints** (pick the one closest to your users)
- `https://aws-mcp.us-east-1.api.aws/mcp` — US East / N. Virginia
- `https://aws-mcp.eu-central-1.api.aws/mcp` — Europe / Frankfurt

Both endpoints can call APIs in any AWS region.

**Observability and audit**
- CloudWatch metrics published under the `AWS-MCP` namespace — separate visibility on agent traffic
- CloudTrail captures all API calls; combined with the `aws:ViaAWSMCPService` context key, you can cleanly distinguish human and agent activity in audit data

**Pricing**
- No charge for the AWS MCP Server itself. You pay only for the AWS resources used and any data transfer.

#### 📚 Step-by-Step Tutorial

**New to the AWS MCP Server?** Follow our tutorial:

👉 **[Tutorial: AWS MCP Remote Server — Complete AWS Interactions](../tutorials/07-aws-mcp-remote-server.md)**

Learn how to:
- Set up an IAM policy that uses the new context keys
- Configure the server in Claude Desktop, VS Code, or Claude Code (us-east-1 or eu-central-1)
- Query AWS APIs, documentation, and resources through natural language
- Use `run_script` for multi-step FinOps workflows
- Use Skills for cost optimization and other AWS tasks

---

## ![AWS Logo](https://www.vectorlogo.zone/logos/amazon_aws/amazon_aws-icon.svg) Individual AWS MCP Servers

**Repository**: [awslabs/mcp](https://github.com/awslabs/mcp)
**Documentation**: [awslabs.github.io/mcp](https://awslabs.github.io/mcp/)

These **individual MCP servers** provide granular access to specific AWS services. They continue to be supported and are ideal for teams that need fine-grained control over specific AWS APIs. The unified AWS MCP Server (above) is designed to work alongside these servers, not replace them.

**Available Individual Servers:**

---




| **Server Name** | **Description** | **Install** |
|:----------------|:----------------|:-------------|
| [AWS Pricing MCP Server](https://awslabs.github.io/mcp/servers/aws-pricing-mcp-server/) | AWS service pricing and cost estimates | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-red?logo=cursor&logoColor=white)](https://cursor.com/en/install-mcp?name=awslabs.aws-pricing-mcp-server&config=ewogICAgImNvbW1hbmQiOiAidXZ4IGF3c2xhYnMuYXdzLXByaWNpbmctbWNwLXNlcnZlckBsYXRlc3QiLAogICAgImVudiI6IHsKICAgICAgIkZBU1RNQ1BfTE9HX0xFVkVMIjogIkVSUk9SIiwKICAgICAgIkFXU19QUk9GSUxFIjogInlvdXItYXdzLXByb2ZpbGUiLAogICAgICAiQVdTX1JFR0lPTiI6ICJ1cy1lYXN0LTEiCiAgICB9LAogICAgImRpc2FibGVkIjogZmFsc2UsCiAgICAiYXV0b0FwcHJvdmUiOiBbXQogIH0K) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=AWS%20Pricing%20MCP%20Server&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22awslabs.aws-pricing-mcp-server%40latest%22%5D%2C%22env%22%3A%7B%22FASTMCP_LOG_LEVEL%22%3A%22ERROR%22%2C%22AWS_PROFILE%22%3A%22your-aws-profile%22%2C%22AWS_REGION%22%3A%22us-east-1%22%7D%2C%22disabled%22%3Afalse%2C%22autoApprove%22%3A%5B%5D%7D) |
| [AWS Cost Explorer MCP Server](https://awslabs.github.io/mcp/servers/cost-explorer-mcp-server/) | Detailed cost analysis and reporting | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-red?logo=cursor&logoColor=white)](https://cursor.com/en/install-mcp?name=awslabs.cost-explorer-mcp-server&config=eyJjb21tYW5kIjoidXZ4IGF3c2xhYnMuY29zdC1leHBsb3Jlci1tY3Atc2VydmVyQGxhdGVzdCIsImVudiI6eyJBV1NfUFJPRklMRSI6InlvdXItYXdzLXByb2ZpbGUiLCJBV1NfUkVHSU9OIjoidXMtZWFzdC0xIiwiRkFTVE1DUF9MT0dfTEVWRUwiOiJFUlJPUiJ9LCJkaXNhYmxlZCI6ZmFsc2UsImF1dG9BcHByb3ZlIjpbXX0%3D) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=Cost%20Explorer%20MCP%20Server&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22awslabs.cost-explorer-mcp-server%40latest%22%5D%2C%22env%22%3A%7B%22AWS_PROFILE%22%3A%22your-aws-profile%22%2C%22AWS_REGION%22%3A%22us-east-1%22%2C%22FASTMCP_LOG_LEVEL%22%3A%22ERROR%22%7D%2C%22disabled%22%3Afalse%2C%22autoApprove%22%3A%5B%5D%7D) |
| [Amazon CloudWatch MCP Server](https://awslabs.github.io/mcp/servers/aws-cloudwatch-mcp-server/) | Metrics, alarms, and logs analysis | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-red?logo=cursor&logoColor=white)](https://cursor.com/en/install-mcp?name=awslabs.cloudwatch-mcp-server&config=ewogICAgImF1dG9BcHByb3ZlIjogW10sCiAgICAiZGlzYWJsZWQiOiBmYWxzZSwKICAgICJjb21tYW5kIjogInV2eCBhd3NsYWJzLmNsb3Vkd2F0Y2gtbWNwLXNlcnZlckBsYXRlc3QiLAogICAgImVudiI6IHsKICAgICAgIkFXU19QUk9GSUxFIjogIltUaGUgQVdTIFByb2ZpbGUgTmFtZSB0byB1c2UgZm9yIEFXUyBhY2Nlc3NdIiwKICAgICAgIkZBU1RNQ1BfTE9HX0xFVkVMIjogIkVSUk9SIgogICAgfSwKICAgICJ0cmFuc3BvcnRUeXBlIjogInN0ZGlvIgp9) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=CloudWatch%20MCP%20Server&config=%7B%22autoApprove%22%3A%5B%5D%2C%22disabled%22%3Afalse%2C%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22awslabs.cloudwatch-mcp-server%40latest%22%5D%2C%22env%22%3A%7B%22AWS_PROFILE%22%3A%22%5BThe%20AWS%20Profile%20Name%20to%20use%20for%20AWS%20access%5D%22%2C%22FASTMCP_LOG_LEVEL%22%3A%22ERROR%22%7D%2C%22transportType%22%3A%22stdio%22%7D) |
| [AWS Billing & Cost Management MCP Server](https://awslabs.github.io/mcp/servers/billing-cost-management-mcp-server/) | Comprehensive billing and cost management with optimization recos | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-red?logo=cursor&logoColor=white)](https://cursor.com/en/install-mcp?name=awslabs.billing-cost-management-mcp-server&config=ewogICAgImNvbW1hbmQiOiAidXZ4IGF3c2xhYnMuYmlsbGluZy1jb3N0LW1hbmFnZW1lbnQtbWNwLXNlcnZlckBsYXRlc3QiLAogICAgImVudiI6IHsKICAgICAgIkZBU1RNQ1BfTE9HX0xFVkVMIjogIkVSUk9SIiwKICAgICAgIkFXU19QUk9GSUxFIjogInlvdXItYXdzLXByb2ZpbGUiLAogICAgICAiQVdTX1JFR0lPTiI6ICJ1cy1lYXN0LTEiCiAgICB9LAogICAgImRpc2FibGVkIjogZmFsc2UsCiAgICAiYXV0b0FwcHJvdmUiOiBbXQogIH0K) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=AWS%20Billing%20and%20Cost%20Management%20MCP%20Server&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22awslabs.billing-cost-management-mcp-server%40latest%22%5D%2C%22env%22%3A%7B%22FASTMCP_LOG_LEVEL%22%3A%22ERROR%22%2C%22AWS_PROFILE%22%3A%22your-aws-profile%22%2C%22AWS_REGION%22%3A%22us-east-1%22%7D%2C%22disabled%22%3Afalse%2C%22autoApprove%22%3A%5B%5D%7D) |
| [CFM Tips MCP Server](https://github.com/aws-samples/sample-cfm-tips-mcp) | Cost optimization playbooks & actionable savings recos | Manual install: see [instructions here](https://github.com/aws-samples/sample-cfm-tips-mcp#installation) |
| [AWS for SAP MCP Server](https://github.com/awslabs/mcp) | SAP ERP integration via dynamic service catalog discovery and CloudWatch telemetry; covers finance, procurement, logistics, supply chain workflows. GA May 1, 2026 ([release notes](https://github.com/awslabs/mcp/releases)). | Manual install via CloudFormation: see [awslabs/mcp](https://github.com/awslabs/mcp) |

---

📝 **Notes**
- These MCP servers are **officially maintained by AWS**.  
- Install via Cursor or VS Code for the smoothest experience and manual install with the CFM Tips MCP.  
- Contributions are welcome via PRs to [AWS Labs MCP repo](https://github.com/awslabs/mcp). 
---



## 💰 AWS FinOps MCP Server (Community)

**Repository**: [ravikiranvm/aws-finops-mcp-server](https://github.com/ravikiranvm/aws-finops-mcp-server)  
**Status**: 🧪 Community Project

Specialised MCP server focused specifically on AWS financial operations and cost management.


#### Use Cases:
- Cost anomaly detection
- Budget monitoring and alerting
- Resource utilization analysis
- Cost allocation and chargeback
- Reserved Instance optimization

---



## ⚙️ Configuration Requirements

### ✅ Prerequisites
- **MCP Client**: one of [Cursor](https://cursor.com), [Claude Desktop](https://modelcontextprotocol.io/clients/claude), [VS Code MCP Extension](https://marketplace.visualstudio.com/items?itemName=aws), or another MCP-compatible client.  
- **MCP Server Configs**: install the MCP server of your choice (Pricing, Cost Explorer, CloudWatch, Billing, or CFM Tips).  
- **Local Setup**:
  - [Node.js 18+](https://nodejs.org) (needed to run the `uvx` commands in configs)  
  - [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) installed and configured with profiles  
  - A configured `~/.aws/credentials` file or environment variables (`AWS_PROFILE`, `AWS_REGION`)  



### 🔧 Integration Examples

#### With Claude Desktop
Example of json: 

```json
{
  "mcpServers": {
    "aws-cost": {
      "command": "node",
      "args": ["/path/to/aws-mcp-server"],
      "env": {
        "AWS_REGION": "us-east-1"
      }
    }
  }
}
```

### With VS Code
Install the MCP extension and configure the AWS server in your workspace settings.

---



### 🔐 Required AWS Permissions

All AWS MCP servers run in **read-only mode**.  
For least privilege, create a **dedicated IAM user or role** and attach a minimal policy that grants only the actions needed (Pricing, Cost Explorer, CloudWatch, Billing, and CFM Tips services).  

👉 Full JSON policy is provided here:  
[**AWS MCP Servers — Least-Privilege IAM Policy**](../governance/security-aws-iam-policies.md)  

> In short: Pricing requires `pricing:GetProducts`, Cost Explorer requires `ce:Get*` actions, CloudWatch uses `cloudwatch:Get*` and `logs:Get*`, Billing relies on Cost Explorer/CUR reads, and CFM Tips also needs `Describe*` access for EC2, RDS, Lambda, and optimization APIs.

---



## 🛠️ Testing & Validation

### Before Production Use:
1. **Test with small datasets** first
2. **Validate cost calculations** against AWS console Cost Explorer
3. **Check API rate limits** for your use case
4. **Verify permissions** are correctly configured
5. **Test error handling** scenarios

### Monitoring Your Usage:
- Track MCP server performance
- Monitor AWS API calls and costs
- Set up alerts for unusual activity
- Regular validation of cost data accuracy

---



## 🤝 Community & Support

- **Official AWS Support**: For AWS Labs servers
- **GitHub Issues**: For community servers
- **AWS FinOps Community**: Join discussions and share experiences
- **MCP Discord**: General MCP support and discussions

---

## 🔗 Related Resources

### Getting Started
- [AWS Pricing Quickstart Tutorial](../tutorials/01-aws-pricing-quickstart.md)
- [Cost Analysis with Kiro CLI](../tutorials/02-amazon-kiro-cli-cost-analysis.md)
- [FinOps Multi-Agent with Nova](../tutorials/03-finops-multi-agent-nova.md)

### Security & Deployment
- [AWS IAM Policies for MCP Servers](../governance/security-aws-iam-policies.md)
- [Remote MCP Servers Guide](../governance/remote-mcp-servers.md)
- [MCP Security Best Practices 2025](../governance/security-best-practices-2025.md)

### MCP Clients
- [All MCP Clients](../clients/INDEX.md)
- [Kiro](../clients/kiro.md) - AWS-focused agentic IDE
- [Claude Code](../clients/claude-code.md) - Remote MCP support

---

← [Back to Servers](./INDEX.md) | [Home](../README.md) | [Next: Azure Servers](./azure.md)

