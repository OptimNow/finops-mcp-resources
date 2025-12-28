← [Back to Servers](./INDEX.md) | [Home](../README.md) | [Azure Servers](./azure.md) | [GCP Servers](./gcp.md)

---

# AWS MCP Servers

**Last Updated**: January 2026

A comprehensive guide to AWS-related Model Context Protocol (MCP) servers for FinOps and cloud management.

---

## ⭐ AWS MCP Remote Server - Generally Available

**Official AWS Documentation**: [AWS MCP User Guide](https://docs.aws.amazon.com/aws-mcp/latest/userguide/what-is-mcp-server.html)
**Repository**: [awslabs/mcp](https://github.com/awslabs/mcp)
**Status**: Generally Available (GA)

### 🎯 Start Here for Complete AWS Interactions!

AWS's **first remote, managed MCP server** is now Generally Available. This hosted solution eliminates local installation complexity while providing comprehensive access to all AWS services, documentation, and best practices.

#### Why Choose the Remote Server?

**Zero Installation Overhead**
- No local dependencies or version management
- AWS hosts and maintains the infrastructure
- Always up-to-date with latest AWS APIs and documentation
- Works with any MCP-compatible client

**Comprehensive AWS Coverage**
- **15,000+ AWS APIs** through one unified interface
- Real-time access to AWS documentation, API references, What's New posts
- Getting Started guides and best practices built-in
- Complete resource management across all AWS services

**Agent Standard Operating Procedures (SOPs)**
- Pre-built workflows for common AWS tasks
- Codified best practices for FinOps automation
- Command validation and security controls
- Accelerated time from idea to implementation

**Enterprise-Grade Reliability**
- Built on AWS's scalable infrastructure
- IAM-based security and access control
- Centralized audit logging and compliance
- Production-ready for enterprise deployments

#### 📚 Step-by-Step Tutorial

**New to the AWS MCP Remote Server?** Follow our complete tutorial:

👉 **[Tutorial: AWS MCP Remote Server - Complete AWS Interactions](../tutorials/07-aws-mcp-remote-server.md)**

Learn how to:
- Set up IAM permissions for secure access
- Configure the remote server in Claude Desktop, VS Code, or Claude Code
- Query AWS APIs, documentation, and resources through natural language
- Use Agent SOPs for FinOps workflows and cost optimization

---

## 🚀 AWS MCP Server (Unified Architecture) - Preview

**Repository**: [awslabs/mcp](https://github.com/awslabs/mcp)
**Announcement**: November 2025
**Status**: Preview (Generally Available Soon)

AWS has introduced a **unified AWS MCP Server** that consolidates multiple AWS MCP capabilities into a single, powerful interface. This represents the future of AWS MCP integration, orchestrating access to AWS services through a streamlined architecture.

### What's New in the Unified AWS MCP Server

**🎯 Consolidated Architecture**
- **Single server** combines AWS API MCP and AWS Knowledge servers
- Access to **15,000+ AWS APIs** through one unified interface
- Simplified configuration and deployment
- Built-in orchestration across AWS services

**📚 Agent Standard Operating Procedures (SOPs)**
- Pre-built workflows for common AWS tasks
- Best practices codified into reusable patterns
- Accelerates FinOps automation and cost optimization
- Reduces time from idea to implementation

**☁️ Remote MCP Server Support**
- AWS Knowledge MCP server is now **Generally Available** (GA)
- AWS's **first remote MCP server** (vs local STDIO)
- Cloud-hosted, scalable infrastructure
- Enhanced security and enterprise controls

### Key Benefits for FinOps

1. **Unified Cost Management**: Access pricing, billing, and optimization APIs through one server
2. **Simplified Setup**: Replace multiple server configurations with a single unified deployment
3. **Enhanced Reliability**: AWS-managed infrastructure with enterprise SLAs
4. **Better Performance**: Optimized for AWS API interactions and data retrieval
5. **Task Workflows**: Leverage MCP Specification 2025-11-25 tasks for long-running cost analyses

### Migration Path

The unified AWS MCP Server is designed to work alongside existing individual servers:
- **Current users**: Continue using individual servers (Pricing, Cost Explorer, CloudWatch, etc.)
- **New users**: Consider starting with the unified AWS MCP Server for simplified setup
- **Enterprise teams**: Evaluate remote MCP deployment for centralized management

👉 **Learn more**: [AWS MCP Server Documentation](https://awslabs.github.io/mcp/)

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
- [Cost Analysis with Amazon Q](../tutorials/02-amazon-q-cost-analysis.md)
- [FinOps Multi-Agent with Nova](../tutorials/03-finops-multi-agent-nova.md)

### Security & Deployment
- [AWS IAM Policies for MCP Servers](../governance/security-aws-iam-policies.md)
- [Remote MCP Servers Guide](../governance/remote-mcp-servers.md)
- [MCP Security Best Practices 2025](../governance/security-best-practices-2025.md)

### MCP Clients
- [All MCP Clients](../clients/INDEX.md)
- [Amazon Q for FinOps](../clients/amazon-q.md)
- [Claude Code](../clients/claude-code.md) - Remote MCP support

---

← [Back to Servers](./INDEX.md) | [Home](../README.md) | [Next: Azure Servers](./azure.md)

