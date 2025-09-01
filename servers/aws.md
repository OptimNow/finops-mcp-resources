# AWS MCP Servers

A comprehensive guide to AWS-related Model Context Protocol (MCP) servers for FinOps and cloud management.

## ![AWS Logo](https://www.vectorlogo.zone/logos/amazon_aws/amazon_aws-icon.svg) Official AWS MCP Servers

### AWS Labs MCP Collection
**Repository**: [awslabs/mcp](https://github.com/awslabs/mcp)  
**Documentation**: [awslabs.github.io/mcp](https://awslabs.github.io/mcp/)  
Below is the list of AWS-maintained MCP servers you can run today.  
Each entry links to the GitHub repo and includes quick-install buttons for Cursor and VS Code.


| Server | Description | Repo | Quick Install |
|:------|:------------|:-----|:--------------|
| **AWS Pricing MCP** | Query AWS price lists and simulate costs | [🔗 GitHub](https://github.com/awslabs/mcp/tree/main/src/aws-pricing-mcp-server) | [![Cursor](https://img.shields.io/badge/Install-Cursor-blue?logo=cursor&logoColor=white)](https://cursor.sh/mcp?source=https://github.com/awslabs/mcp/tree/main/src/aws-pricing-mcp-server) <br> [![VS Code](https://img.shields.io/badge/Install-VS%20Code-green?logo=visualstudiocode&logoColor=white)](https://marketplace.visualstudio.com/items?itemName=AWS.aws-pricing-mcp) |
| **AWS Cost Explorer MCP** | Retrieve AWS Cost Explorer data for FinOps use cases | [🔗 GitHub](https://github.com/awslabs/mcp/tree/main/src/aws-ce-mcp-server) | [![Cursor](https://img.shields.io/badge/Install-Cursor-blue?logo=cursor&logoColor=white)](https://cursor.sh/mcp?source=https://github.com/awslabs/mcp/tree/main/src/aws-ce-mcp-server) <br> [![VS Code](https://img.shields.io/badge/Install-VS%20Code-green?logo=visualstudiocode&logoColor=white)](https://marketplace.visualstudio.com/items?itemName=AWS.aws-ce-mcp) |
| **AWS Health MCP** | Access AWS Health events (incidents, advisories) | [🔗 GitHub](https://github.com/awslabs/mcp/tree/main/src/aws-health-mcp-server) | [![Cursor](https://img.shields.io/badge/Install-Cursor-blue?logo=cursor&logoColor=white)](https://cursor.sh/mcp?source=https://github.com/awslabs/mcp/tree/main/src/aws-health-mcp-server) <br> [![VS Code](https://img.shields.io/badge/Install-VS%20Code-green?logo=visualstudiocode&logoColor=white)](https://marketplace.visualstudio.com/items?itemName=AWS.aws-health-mcp) |

---

📝 **Notes**
- These MCP servers are **officially maintained by AWS**.  
- Install via Cursor or VS Code for the smoothest experience.  
- Contributions are welcome via PRs to [AWS Labs MCP repo](https://github.com/awslabs/mcp). 
---

## 💰 Specialized FinOps Servers

### AWS FinOps MCP Server (Community)
**Repository**: [ravikiranvm/aws-finops-mcp-server](https://github.com/ravikiranvm/aws-finops-mcp-server)  
**Status**: 🧪 Community Project

Specialized MCP server focused specifically on AWS financial operations and cost management.

#### Why Use This Server?
- **FinOps-First Design**: Built specifically for cloud financial management
- **Cost Optimization Focus**: Tools designed for cost analysis and optimization
- **Simplified Interface**: Streamlined for financial operations teams
- **Custom Workflows**: Pre-built workflows for common FinOps tasks

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


