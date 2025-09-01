# AWS MCP Servers

A comprehensive guide to AWS-related Model Context Protocol (MCP) servers for FinOps and cloud management.

## 🏗️ Official AWS MCP Servers

### AWS Labs MCP Collection
**Repository**: [awslabs/mcp](https://github.com/awslabs/mcp)  
**Documentation**: [awslabs.github.io/mcp](https://awslabs.github.io/mcp/)  
**Status**: ✅ Official AWS Labs Project

The official AWS Labs MCP server collection provides comprehensive AWS service integration to monitor, optimize, and manage your AWS infrastructure and costs.

| Server Name | Description | Install |
|-------------|-------------|---------|
| [AWS Pricing MCP Server](https://awslabs.github.io/mcp/servers/aws-pricing-mcp-server/) | AWS service pricing and cost estimates | <kbd>Install</kbd> [Cursor](https://marketplace.cursorapi.com/aws-pricing-mcp) <br><kbd>Install</kbd> [VS Code](https://marketplace.visualstudio.com/items?itemName=aws.aws-pricing-mcp) |
| [AWS Cost Explorer MCP Server](https://awslabs.github.io/mcp/servers/aws-cost-explorer-mcp-server/) | Detailed cost analysis and reporting | <kbd>Install</kbd> [Cursor](https://marketplace.cursorapi.com/aws-cost-explorer-mcp) <br><kbd>Install</kbd> [VS Code](https://marketplace.visualstudio.com/items?itemName=aws.aws-cost-explorer-mcp) |
| [Amazon CloudWatch MCP Server](https://awslabs.github.io/mcp/servers/aws-cloudwatch-mcp-server/) | Metrics, Alarms, and Logs analysis and operational troubleshooting | <kbd>Install</kbd> [Cursor](https://marketplace.cursorapi.com/aws-cloudwatch-mcp) <br><kbd>Install</kbd> [VS Code](https://marketplace.visualstudio.com/items?itemName=aws.aws-cloudwatch-mcp) |
| [AWS Billing and Cost Management MCP Server](https://awslabs.github.io/mcp/servers/aws-billing-mcp-server/) | Comprehensive billing and cost management with optimization recommendations | <kbd>Install</kbd> [Cursor](https://marketplace.cursorapi.com/aws-billing-mcp) <br><kbd>Install</kbd> [VS Code](https://marketplace.visualstudio.com/items?itemName=aws.aws-billing-mcp) |
| [CFM Tips - Cloud Optimization MCP Server](https://github.com/aws-samples/sample-cfm-tips-mcp) | AWS cost optimization playbooks and actionable savings recommendations | <kbd>Install</kbd> [Manual Setup](https://github.com/aws-samples/sample-cfm-tips-mcp#installation) <br><kbd>Install</kbd> [Amazon Q CLI](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-installing.html) |

---

#### Quick Installation:
```bash
# Via npm
npm install @aws/mcp-server-aws

# Or use the individual servers from the src directory
```


#### Key Features:
- ✅ Official AWS support
- ✅ Comprehensive service coverage
- ✅ Regular updates aligned with AWS services
- ✅ Professional documentation

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


