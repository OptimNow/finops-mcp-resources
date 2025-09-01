# AWS MCP Servers

A comprehensive guide to AWS-related Model Context Protocol (MCP) servers for FinOps and cloud management.

## 🏗️ Official AWS MCP Servers

### AWS Labs MCP Collection
**Repository**: [awslabs/mcp](https://github.com/awslabs/mcp)  
**Documentation**: [awslabs.github.io/mcp](https://awslabs.github.io/mcp/)  
**Status**: ✅ Official AWS Labs Project

The official AWS Labs MCP server collection provides comprehensive AWS service integration:

#### Core Modules most relevant for Cloud FinOps:
- **Cost & Operations**: AWS pricing, cost analysis, and operational tools
- **CloudWatch**: Monitoring, metrics, and log analysis
- **Documentation**: Direct access to AWS documentation
- **HealthOmics**: Genomics workflow management
- **Pricing**: Real-time AWS pricing data and cost estimation

#### Installation:
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

### Sample CFM Tips MCP
**Repository**: [aws-samples/sample-cfm-tips-mcp](https://github.com/aws-samples/sample-cfm-tips-mcp)  
**Status**: ✅ AWS Samples

Official AWS sample demonstrating Cloud Financial Management (CFM) tips and best practices through MCP.

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

## 💡 FinOps Use Cases

### Cost Analysis Workflows
1. **Daily Cost Monitoring**: Automated daily cost reports
2. **Anomaly Detection**: Identify unusual spending patterns
3. **Budget Tracking**: Monitor budget vs. actual spending
4. **Service-Level Costs**: Break down costs by AWS service

### Optimization Scenarios
1. **Right-Sizing Analysis**: Identify over-provisioned resources
2. **Reserved Instance Planning**: Optimize RI coverage
3. **Spot Instance Opportunities**: Find spot-eligible workloads
4. **Storage Optimization**: Optimize S3 storage classes

### Reporting & Governance
1. **Executive Dashboards**: High-level cost summaries
2. **Chargeback Reports**: Allocate costs to teams/projects
3. **Compliance Reporting**: Cost governance and controls
4. **Forecast Modeling**: Predict future spending

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

## 📈 Next Steps

1. **Start with AWS Labs official server** for comprehensive features
2. **Experiment with community FinOps server** for specialized workflows  
3. **Review AWS blog examples** for implementation patterns
4. **Build custom workflows** based on your organization's needs
5. **Share learnings** with the community

---

## 🤝 Community & Support

- **Official AWS Support**: For AWS Labs servers
- **GitHub Issues**: For community servers
- **AWS FinOps Community**: Join discussions and share experiences
- **MCP Discord**: General MCP support and discussions

---

**Last Updated**: January 2025  
**Maintainer**: [Your Name/Organization]  
**Status**: Active maintenance and updates
