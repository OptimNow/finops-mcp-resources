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

The **AWS MCP Remote Server** is AWS's first remote, managed MCP server - a fundamental shift in how you interact with AWS services through AI.

### One Server Replaces Dozens

Previously, you needed to install and maintain separate MCP servers for each AWS service: one for Pricing, another for Cost Explorer, another for CloudWatch, and so on. The AWS MCP Remote Server replaces all of them. It intelligently orchestrates access to 15,000+ AWS APIs, automatically selecting the right tools and services based on your natural language requests. Ask about EC2 costs, and it knows to query the Pricing API. Request billing analysis, and it coordinates between Cost Explorer and your usage data. No more juggling multiple server installations.

### Remote & Managed by AWS

AWS hosts and maintains the entire infrastructure. There's no local installation, no version updates to manage, no dependency conflicts to resolve. The server is always current with the latest AWS APIs, documentation, and best practices. You configure it once in your MCP client and AWS handles everything else - updates, scaling, and reliability.

### Built-in Intelligence with Agent SOPs

The server includes Agent Standard Operating Procedures (SOPs) - pre-built workflows that encode AWS best practices. When you ask for cost optimization recommendations, the server doesn't just fetch raw data. It applies proven FinOps methodologies, validates commands for accuracy, and structures responses following AWS's recommended patterns. This intelligence accelerates your work from idea to implementation while maintaining security controls and compliance standards.

### Why It Matters for FinOps

Traditional local MCP servers required you to know which specific server to query for pricing data versus billing data versus optimization recommendations. The AWS MCP Remote Server handles this orchestration automatically. It unifies cost management, pricing, billing, and optimization insights into a single natural language interface. IAM-based access control provides enterprise-grade security, centralized audit logs simplify compliance, and AWS's infrastructure ensures the performance needed for large-scale data retrieval across your entire cloud environment.

---

## 📋 Prerequisites

### Required
- **AWS Account** with active credentials
- **IAM permissions** to create users and policies (or admin help)
- **MCP-compatible client**: Claude Desktop, VS Code, or Claude Code (see [all clients](../clients/INDEX.md))

### Recommended
- Basic familiarity with AWS IAM
- Understanding of AWS regions (we'll use `us-east-1` / North Virginia in this tutorial)

---

## 🚀 Quick Start

### Step 1: Create IAM User for MCP Access

1. **Navigate to IAM Console**:
   - Go to [AWS IAM Console](https://console.aws.amazon.com/iam/)
   - Click **Users** → **Create user**

2. **Create User**:
   - User name: `mcp-aws-user` (or your preferred name)
   - Click **Next**

3. **Set permissions**:
   - Select **Attach policies directly**
   - For now, skip selecting policies (we'll add the specific MCP policy in Step 2)
   - Click **Next** → **Create user**

4. **Create Access Key**:
   - After creating the user, click on the user name to view details
   - Go to the **Security credentials** tab
   - Scroll down to **Access keys** section
   - Click **Create access key**

5. **Select Use Case**:
   - Choose **Command Line Interface (CLI)**
   - Check the confirmation box: "I understand the above recommendation..."
   - Click **Next**

6. **Set Description Tag** (Optional):
   - Description: `MCP Server Access` (optional but recommended)
   - Click **Create access key**

7. **Save Credentials**:
   - **Copy** or **Download .csv file** with:
     - Access Key ID
     - Secret Access Key
   - ⚠️ **IMPORTANT**: Store these credentials securely - you won't be able to see the secret key again
   - Click **Done**

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
- Default region: `us-east-1` (North Virginia)
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
export AWS_REGION="us-east-1"
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
        "AWS_REGION=us-east-1"
      ],
      "env": {
        "AWS_PROFILE": "mcp-aws"
      }
    }
  }
}
```

**Notes**:
- This tutorial uses `us-east-1` (North Virginia). You can change to your preferred AWS region
- Replace `mcp-aws` with your AWS profile name if different
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
        "AWS_REGION=us-east-1"
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
  --args "mcp-proxy-for-aws@latest,https://aws-mcp.us-east-1.api.aws/mcp,--metadata,AWS_REGION=us-east-1" \
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
List my EC2 instances in us-east-1
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
Analyze my AWS resources in us-east-1 and suggest cost optimization opportunities
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
