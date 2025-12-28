← [Back to Tutorials](./INDEX.md) | [Home](../README.md)

---

# AWS MCP Remote Server - Complete AWS Interactions

**Tutorial 7 of 7** | ⏱️ **Time**: 20-30 minutes | 💻 **Level**: Beginner

**Last Updated**: January 2026

---

## 🎯 What You'll Learn

- Set up the official AWS MCP remote server (hosted by AWS)
- Configure IAM permissions for secure remote access
- Query AWS APIs, documentation, and resources through natural language
- Leverage Agent Standard Operating Procedures (SOPs) for AWS tasks

---

## 🌟 Why This MCP Server Changes Everything

The **AWS MCP Server** is AWS's first **remote, managed MCP server** - a game-changer for cloud operations:

### Remote & Managed Architecture
- **No local installation required** - AWS hosts and manages the infrastructure
- **Always up-to-date** - AWS maintains the latest API support and documentation
- **Enterprise-grade reliability** - Built on AWS's scalable infrastructure
- **Zero maintenance burden** - No version updates, no dependency management

### Comprehensive AWS Coverage
- **15,000+ AWS APIs** - Access all AWS services through one unified interface
- **Latest documentation** - Real-time access to AWS docs, API references, and What's New posts
- **Getting Started guides** - Built-in tutorials and best practices for AWS services
- **Complete resource access** - Query, manage, and analyze your AWS infrastructure

### Agent Standard Operating Procedures (SOPs)
- **Pre-built workflows** - Codified best practices for common AWS tasks
- **Accelerated automation** - Reduce time from idea to implementation
- **FinOps optimization** - Cost analysis, resource optimization, and savings recommendations
- **Accurate execution** - Command validation and security controls built-in

### Why It Matters for FinOps
Traditional MCP servers require local installation, version management, and manual updates. The AWS MCP remote server eliminates this overhead while providing:
- **Unified cost management** - Pricing, billing, and optimization in one place
- **Enhanced security** - IAM-based access control with AWS's enterprise security model
- **Better performance** - Optimized for AWS API interactions and large-scale data retrieval
- **Simplified compliance** - Centralized audit logs and governance controls

---

## 📋 Prerequisites

### Required
- **AWS Account** with active credentials
- **IAM permissions** to create users and policies (or admin help)
- **MCP-compatible client**: Claude Desktop, VS Code, or Claude Code (see [all clients](../clients/INDEX.md))

### Recommended
- Basic familiarity with AWS IAM
- Understanding of AWS regions (we'll use `us-west-2` in examples)

---

## 🚀 Quick Start

### Step 1: Create IAM User for MCP Access

1. **Navigate to IAM Console**:
   - Go to [AWS IAM Console](https://console.aws.amazon.com/iam/)
   - Click **Users** → **Create user**

2. **Create User**:
   - User name: `mcp-aws-user` (or your preferred name)
   - Access type: **Programmatic access**
   - Click **Next**

3. **Skip group assignment** (we'll attach policy directly)
   - Click **Next** → **Create user**

4. **Save credentials**:
   - Download the CSV or copy:
     - Access Key ID
     - Secret Access Key
   - ⚠️ **Store securely** - you won't see the secret key again

### Step 2: Attach Required IAM Policy

The AWS MCP server requires the `aws-mcp:InvokeMcp` permission to function.

#### Option A: Using AWS Console

1. Select your user (`mcp-aws-user`)
2. Click **Add permissions** → **Create inline policy**
3. Choose **JSON** tab and paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "aws-mcp:InvokeMcp"
      ],
      "Resource": "*"
    }
  ]
}
```

4. Name the policy: `AWSMCPAccess`
5. Click **Create policy**

#### Option B: Using AWS CLI

```bash
aws iam put-user-policy \
  --user-name mcp-aws-user \
  --policy-name AWSMCPAccess \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": ["aws-mcp:InvokeMcp"],
        "Resource": "*"
      }
    ]
  }'
```

### Step 3: Configure AWS Credentials

Set up your AWS credentials for the MCP server to use:

#### Option A: AWS Profile (Recommended)

1. **Create AWS CLI profile**:

```bash
aws configure --profile mcp-aws
```

Enter when prompted:
- AWS Access Key ID: `[your key from Step 1]`
- AWS Secret Access Key: `[your secret from Step 1]`
- Default region: `us-west-2` (or your preferred region)
- Default output format: `json`

2. **Verify profile**:

```bash
aws sts get-caller-identity --profile mcp-aws
```

Expected output:
```json
{
  "UserId": "AIDACKCEVSQ6C2EXAMPLE",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/mcp-aws-user"
}
```

#### Option B: Environment Variables

Alternatively, set environment variables:

```bash
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_REGION="us-west-2"
```

### Step 4: Install and Configure MCP Client

#### For Claude Desktop (macOS/Windows)

1. **Install uv** (Python package manager):

**macOS/Linux**:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows** (PowerShell):
```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

2. **Configure Claude Desktop**:

Edit your Claude Desktop config:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

Add this configuration:

```json
{
  "mcpServers": {
    "aws-mcp": {
      "command": "uvx",
      "args": [
        "mcp-proxy-for-aws@latest",
        "https://aws-mcp.us-east-1.api.aws/mcp",
        "--metadata",
        "AWS_REGION=us-west-2"
      ],
      "env": {
        "AWS_PROFILE": "mcp-aws"
      }
    }
  }
}
```

**Notes**:
- Replace `us-west-2` with your preferred AWS region
- Replace `mcp-aws` with your AWS profile name
- The server endpoint is always `https://aws-mcp.us-east-1.api.aws/mcp` (hosted in us-east-1)

3. **Restart Claude Desktop**

#### For VS Code

1. **Install MCP extension**:
   - Search for "Model Context Protocol" in Extensions
   - Install the official MCP extension

2. **Configure in VS Code settings** (`.vscode/settings.json`):

```json
{
  "mcp.servers": {
    "aws-mcp": {
      "command": "uvx",
      "args": [
        "mcp-proxy-for-aws@latest",
        "https://aws-mcp.us-east-1.api.aws/mcp",
        "--metadata",
        "AWS_REGION=us-west-2"
      ],
      "env": {
        "AWS_PROFILE": "mcp-aws"
      }
    }
  }
}
```

3. **Reload VS Code**

#### For Claude Code

Claude Code has built-in support for remote MCP servers:

```bash
# In your project directory
claude-code config mcp add aws-mcp \
  --command uvx \
  --args "mcp-proxy-for-aws@latest,https://aws-mcp.us-east-1.api.aws/mcp,--metadata,AWS_REGION=us-west-2" \
  --env "AWS_PROFILE=mcp-aws"
```

---

## 🧪 Testing Your Setup

### Verify Connection

In your MCP client (Claude Desktop, VS Code, etc.), try these queries:

#### Test 1: Basic AWS Information

**Prompt**:
```
What AWS regions are available?
```

**Expected**: List of AWS regions with descriptions

#### Test 2: Service Documentation

**Prompt**:
```
Show me the latest features for Amazon S3
```

**Expected**: Recent S3 announcements and What's New posts

#### Test 3: API Access

**Prompt**:
```
List my EC2 instances in us-west-2
```

**Expected**: List of your EC2 instances (or empty list if none exist)

#### Test 4: Cost Analysis (FinOps)

**Prompt**:
```
What are the pricing options for t3.medium EC2 instances?
```

**Expected**: Pricing details for t3.medium instances

---

## 💡 Example FinOps Workflows

### Cost Optimization

**Prompt**:
```
Analyze my AWS resources in us-west-2 and suggest cost optimization opportunities
```

The AI will:
- Query your resources using AWS APIs
- Access AWS optimization best practices (SOPs)
- Provide actionable recommendations
- Estimate potential savings

### Budget Monitoring

**Prompt**:
```
Show me my AWS spending for the last 30 days by service
```

The AI will:
- Use Cost Explorer APIs
- Aggregate spending data
- Present breakdowns by service
- Highlight cost trends

### Reserved Instance Analysis

**Prompt**:
```
Should I purchase Reserved Instances for my EC2 workloads?
```

The AI will:
- Analyze your EC2 usage patterns
- Access AWS pricing documentation
- Apply RI recommendation SOPs
- Provide purchase recommendations with ROI calculations

---

## 🔧 Troubleshooting

### Issue: "uvx not found" error

**Solution**: Install `uv`:

**macOS/Linux**:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows**:
```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Then restart your terminal and MCP client.

### Issue: "Not authorized to perform aws-mcp:InvokeMcp"

**Solution**: Check IAM permissions:

1. Verify policy is attached to your user:
```bash
aws iam list-user-policies --user-name mcp-aws-user
```

2. Get policy document:
```bash
aws iam get-user-policy \
  --user-name mcp-aws-user \
  --policy-name AWSMCPAccess
```

3. Ensure it includes `aws-mcp:InvokeMcp` action

### Issue: "Invalid credentials" or "Access Denied"

**Solution**: Verify AWS credentials:

```bash
# Test with your profile
aws sts get-caller-identity --profile mcp-aws

# Check profile configuration
cat ~/.aws/credentials
cat ~/.aws/config
```

### Issue: Server not appearing in MCP client

**Solution**:
1. Restart your MCP client completely
2. Check config file syntax (valid JSON)
3. Verify `uvx` is in your PATH
4. Check client logs for error messages

---

## 🔐 Security Best Practices

### Principle of Least Privilege

For production use, extend the IAM policy to grant only the specific AWS permissions needed:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "aws-mcp:InvokeMcp"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "s3:List*",
        "s3:Get*",
        "ce:Get*",
        "ce:Describe*",
        "pricing:GetProducts",
        "cloudwatch:Get*",
        "cloudwatch:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

### Credential Rotation

- Rotate access keys regularly (every 90 days recommended)
- Use AWS Secrets Manager for automated rotation
- Never commit credentials to version control

### Audit Logging

Enable AWS CloudTrail to monitor MCP server API calls:

```bash
aws cloudtrail create-trail \
  --name mcp-audit-trail \
  --s3-bucket-name your-audit-bucket
```

---

## 📊 Comparison: Remote vs. Local MCP Servers

| Feature | AWS MCP Remote Server | Local MCP Servers |
|---------|----------------------|-------------------|
| Installation | One-time config only | Install per server |
| Maintenance | AWS manages updates | Manual updates required |
| Performance | AWS-optimized infrastructure | Depends on local resources |
| API Coverage | 15,000+ AWS APIs | Limited to server scope |
| Documentation | Always current | May lag behind AWS |
| Security | IAM-based, enterprise-grade | Local credential management |
| Cost | Included with AWS | Free (community servers) |
| Best For | Production, enterprise teams | Development, specific APIs |

**Recommendation**: Use the AWS MCP remote server for comprehensive AWS interactions. Supplement with specialized local servers (e.g., Cost Explorer MCP) for specific deep-dive workflows if needed.

---

## 🎓 Next Steps

### Immediate Actions
1. ✅ Complete this tutorial setup
2. 🔍 Test the example workflows above
3. 📚 Explore AWS documentation through the MCP server

### Advanced Learning
- **Tutorial 3**: [FinOps Multi-Agent with Nova](./03-finops-multi-agent-nova.md) - Combine AWS MCP with multi-agent workflows
- **Security Guide**: [AWS IAM Policies for MCP](../governance/security-aws-iam-policies.md)
- **Remote MCP**: [Remote MCP Servers Guide](../governance/remote-mcp-servers.md)

### Production Deployment
- Set up CloudTrail logging for audit compliance
- Implement credential rotation schedules
- Define organization-wide IAM policies
- Integrate with your CI/CD pipelines

---

## 📚 Additional Resources

### Official Documentation
- [AWS MCP Server User Guide](https://docs.aws.amazon.com/aws-mcp/latest/userguide/what-is-mcp-server.html)
- [AWS Labs MCP Repository](https://github.com/awslabs/mcp)
- [MCP Specification](https://modelcontextprotocol.io/specification)

### Related Tutorials
- [AWS Pricing MCP Quickstart](./01-aws-pricing-quickstart.md) - Local server alternative
- [Cost Analysis with Amazon Q](./02-amazon-q-cost-analysis.md) - AWS-native AI assistant
- [All Tutorials](./INDEX.md) - Complete tutorial index

### Community
- [MCP Discord](https://discord.gg/mcp) - General MCP support
- [AWS FinOps Community](https://www.finops.org/slack/) - FinOps discussions
- [GitHub Discussions](https://github.com/awslabs/mcp/discussions) - AWS MCP specific

---

## 🐛 Report Issues

Found a problem with this tutorial?
- Open an issue: [GitHub Issues](https://github.com/OptimNow/finops-mcp-resources/issues)
- Suggest improvements: [Pull Requests](https://github.com/OptimNow/finops-mcp-resources/pulls)

---

## ✨ Summary

**What you accomplished**:
- ✅ Set up AWS IAM user with MCP permissions
- ✅ Configured AWS credentials securely
- ✅ Connected to AWS's first remote MCP server
- ✅ Accessed 15,000+ AWS APIs through natural language
- ✅ Tested FinOps workflows with Agent SOPs

**Key takeaway**: The AWS MCP remote server eliminates local installation complexity while providing comprehensive, managed access to all AWS services, documentation, and best practices - perfect for production FinOps workflows.

---

← [Back to Tutorials](./INDEX.md) | [Home](../README.md) | [Next: Multi-Cloud Setup](./04-azure-mcp-quickstart.md)
