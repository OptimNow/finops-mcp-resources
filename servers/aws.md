# AWS MCP Servers

A comprehensive guide to AWS-related Model Context Protocol (MCP) servers for FinOps and cloud management.

## ![AWS Logo](https://www.vectorlogo.zone/logos/amazon_aws/amazon_aws-icon.svg) Official Labs MCP Collection

**Repository**: [awslabs/mcp](https://github.com/awslabs/mcp)  
**Documentation**: [awslabs.github.io/mcp](https://awslabs.github.io/mcp/)  
Below is the list of AWS-maintained MCP servers you can run today.  
Each entry links to the GitHub repo and includes quick-install buttons for Cursor and VS Code.


| **Server Name** | **Description** | **Install** |
|:----------------|:----------------|:-------------|
| [AWS Pricing MCP Server](https://awslabs.github.io/mcp/servers/aws-pricing-mcp-server/) | AWS service pricing and cost estimates | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-blue?logo=cursor&logoColor=white)](https://cursor.com/en/install-mcp?name=awslabs.aws-pricing-mcp-server&config=ewogICAgImNvbW1hbmQiOiAidXZ4IGF3c2xhYnMuYXdzLXByaWNpbmctbWNwLXNlcnZlckBsYXRlc3QiLAogICAgImVudiI6IHsKICAgICAgIkZBU1RNQ1BfTE9HX0xFVkVMIjogIkVSUk9SIiwKICAgICAgIkFXU19QUk9GSUxFIjogInlvdXItYXdzLXByb2ZpbGUiLAogICAgICAiQVdTX1JFR0lPTiI6ICJ1cy1lYXN0LTEiCiAgICB9LAogICAgImRpc2FibGVkIjogZmFsc2UsCiAgICAiYXV0b0FwcHJvdmUiOiBbXQogIH0K) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-green?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=AWS%20Pricing%20MCP%20Server&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22awslabs.aws-pricing-mcp-server%40latest%22%5D%2C%22env%22%3A%7B%22FASTMCP_LOG_LEVEL%22%3A%22ERROR%22%2C%22AWS_PROFILE%22%3A%22your-aws-profile%22%2C%22AWS_REGION%22%3A%22us-east-1%22%7D%2C%22disabled%22%3Afalse%2C%22autoApprove%22%3A%5B%5D%7D) |
| [AWS Cost Explorer MCP Server](https://awslabs.github.io/mcp/servers/aws-cost-explorer-mcp-server/) | Detailed cost analysis and reporting | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-blue?logo=cursor&logoColor=white)](https://marketplace.cursorapi.com/aws-cost-explorer-mcp) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-green?logo=visualstudiocode&logoColor=white)](https://marketplace.visualstudio.com/items?itemName=aws.aws-cost-explorer-mcp) |
| [Amazon CloudWatch MCP Server](https://awslabs.github.io/mcp/servers/aws-cloudwatch-mcp-server/) | Metrics, alarms, logs analysis, and troubleshooting | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-blue?logo=cursor&logoColor=white)](https://marketplace.cursorapi.com/aws-cloudwatch-mcp) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-green?logo=visualstudiocode&logoColor=white)](https://marketplace.visualstudio.com/items?itemName=aws.aws-cloudwatch-mcp) |
| [AWS Billing & Cost Management MCP Server](https://awslabs.github.io/mcp/servers/aws-billing-mcp-server/) | Billing data with optimization recommendations | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-blue?logo=cursor&logoColor=white)](https://marketplace.cursorapi.com/aws-billing-mcp) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-green?logo=visualstudiocode&logoColor=white)](https://marketplace.visualstudio.com/items?itemName=aws.aws-billing-mcp) |
| [CFM Tips MCP Server](https://github.com/aws-samples/sample-cfm-tips-mcp) | Cost optimization playbooks & actionable savings recommendations | [![Install Cursor](https://img.shields.io/badge/Install-Cursor-blue?logo=cursor&logoColor=white)](https://github.com/aws-samples/sample-cfm-tips-mcp#installation) <br> [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-green?logo=visualstudiocode&logoColor=white)](https://github.com/aws-samples/sample-cfm-tips-mcp#installation) |

---

📝 **Notes**
- These MCP servers are **officially maintained by AWS**.  
- Install via Cursor or VS Code for the smoothest experience.  
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


## 🚀 Quick Start Guide

### 1. Choose Your MCP Server

| Server | Best For | Complexity | Maintenance |
|--------|----------|------------|-------------|
| **AWS Labs Official** | Comprehensive AWS integration | Medium | AWS-maintained |
| **AWS FinOps Community** | Specialized FinOps workflows | Low | Community-maintained |
| **AWS Samples CFM** | Learning and prototyping | Low | Sample code |

### 2. Installation Steps

#### For AWS Labs MCP:
```bash
# Install the main package
npm install @aws/mcp-server-aws

# Configure AWS credentials
aws configure
```

#### For Community FinOps Server:
```bash
# Clone and install
git clone https://github.com/ravikiranvm/aws-finops-mcp-server
cd aws-finops-mcp-server
npm install
```

### 3. Configuration Requirements

**Prerequisites:**
- AWS CLI configured with appropriate permissions
- Node.js 18+ installed
- MCP client (Claude, VS Code, etc.)

**Required AWS Permissions:**
- `cost-explorer:*` (for cost analysis)
- `ce:*` (for Cost Explorer API)
- `pricing:*` (for pricing data)
- `cloudwatch:*` (for monitoring)

---



## 🔧 Integration Examples

### With Claude Desktop
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

## 📊 Server Comparison Matrix

| Feature | AWS Labs Official | Community FinOps | AWS Samples CFM |
|---------|-------------------|------------------|-----------------|
| **Cost Analysis** | ✅ Comprehensive | ✅ Specialized | ✅ Basic |
| **Pricing Data** | ✅ Real-time | ✅ Limited | ❌ |
| **Documentation** | ✅ Extensive | ⚠️ Community | ⚠️ Sample-focused |
| **Maintenance** | ✅ AWS-backed | ⚠️ Community | ⚠️ Sample code |
| **Learning Curve** | Medium | Low | Low |
| **Production Ready** | ✅ | ⚠️ Evaluate | ❌ |

---

## 🛠️ Testing & Validation

### Before Production Use:
1. **Test with small datasets** first
2. **Validate cost calculations** against AWS console
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


