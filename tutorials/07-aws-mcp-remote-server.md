← [Back to Tutorials](./INDEX.md) | [Home](../README.md)

---

# AWS MCP Remote Server - Complete AWS Interactions

**Tutorial 7 of 7** | ⏱️ **Time**: 20-30 minutes | 💻 **Level**: Beginner

**Last Updated**: May 2026

---

## 🎯 What You'll Learn

- Set up the official AWS MCP remote server (hosted by AWS)
- Configure IAM permissions for secure remote access using standard IAM context keys
- Query AWS APIs, documentation, and resources through natural language
- Use the `run_script` tool to chain multiple AWS API calls in a single round-trip
- Leverage Skills for AWS tasks (curated, service-team-maintained guidance)

---

## 🌟 Why This MCP Server Changes Everything

The **AWS MCP Remote Server** is AWS's first remote, managed MCP server - a fundamental shift in how you interact with AWS services through AI.

### One Server Replaces Dozens

Previously, you needed to install and maintain separate MCP servers for each AWS service: one for Pricing, another for Cost Explorer, another for CloudWatch, and so on. The AWS MCP Remote Server replaces all of them. It intelligently orchestrates access to 15,000+ AWS APIs, automatically selecting the right tools and services based on your natural language requests. Ask about EC2 costs, and it knows to query the Pricing API. Request billing analysis, and it coordinates between Cost Explorer and your usage data. No more juggling multiple server installations.

### Remote & Managed by AWS

AWS hosts and maintains the entire infrastructure. There's no local installation, no version updates to manage, no dependency conflicts to resolve. The server is always current with the latest AWS APIs, documentation, and best practices. You configure it once in your MCP client and AWS handles everything else - updates, scaling, and reliability.

### Built-in Intelligence with Skills

The server exposes **Skills** — curated packages of instructions, scripts, and reference material that guide an agent through specific AWS tasks. Skills are contributed and maintained by the individual AWS service teams that own each domain, so the guidance reflects current best practice for that service rather than a generic template. When you ask for cost optimization recommendations, the server can pull the relevant Skill on demand via the `retrieve_skill` tool, which keeps the agent focused on the right APIs and steps without consuming unnecessary context. (When listed by your MCP client, the tools appear with an `aws___` prefix — e.g. `aws___retrieve_skill`, `aws___call_aws`, `aws___run_script`.)

The AWS MCP Server is part of a broader **Agent Toolkit for AWS** — an umbrella that bundles the MCP Server, Skills, Plugins (single-install packages for specific clients), and project-level Rules files.

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

### A note on credential methods

This tutorial uses an **IAM user with static access keys** because they are universally available. AWS recommends two alternatives that auto-rotate credentials and avoid the 90-day key-rotation chore:

- **`aws login`** (AWS CLI 2.32.0+) — for users with AWS Management Console credentials. The SDK rotates credentials every 15 minutes within a session of up to 12 hours.
- **`aws configure sso`** — for users on AWS IAM Identity Center / SSO. Cached refresh tokens renew silently.

If either is available to you, skip Step 1 below, run the relevant `aws ...` command in Step 3 instead of `aws configure`, and continue with Step 2 onward. Static IAM access keys (the path documented in Step 1 and Step 3) remain valid but will not auto-rotate.

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

### Step 2: Attach IAM Policy

With the GA release, the AWS MCP Server uses **standard IAM permissions**. There is no separate `aws-mcp:*` action to grant — access is expressed through the same IAM policies you would write for any direct AWS API call. The previous permissions `aws-mcp:InvokeMcp`, `aws-mcp:CallReadOnlyTool`, and `aws-mcp:CallReadWriteTool` no longer have any effect; remove them from any policy where they still appear.

Documentation tools (`search_documentation`, `read_documentation`) require **no authentication** at all. The IAM policy below only governs `call_aws` and `run_script`, which act on AWS resources on the user's behalf.

#### Option A: Using AWS Console

1. Select your user (`mcp-aws-user`)
2. Click **Add permissions** → **Create inline policy**
3. Choose **JSON** tab and paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FinOpsReadOnly",
      "Effect": "Allow",
      "Action": [
        "ce:GetCostAndUsage",
        "ce:GetCostForecast",
        "ce:GetDimensionValues",
        "ce:GetTags",
        "pricing:GetProducts",
        "ec2:Describe*",
        "s3:List*",
        "cloudwatch:Get*",
        "cloudwatch:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

4. Name the policy: `AWSMCPAccess`
5. Click **Create policy**

Because the user we created in Step 1 is dedicated to MCP, a plain `Allow` is enough. The same credentials can still be used with the AWS CLI (useful for the validation step in Step 3), and CloudTrail will record whether each call came through the MCP Server or directly.

#### Option B: Using AWS CLI

```bash
aws iam put-user-policy \
  --user-name mcp-aws-user \
  --policy-name AWSMCPAccess \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "FinOpsReadOnly",
        "Effect": "Allow",
        "Action": [
          "ce:GetCostAndUsage",
          "ce:GetCostForecast",
          "ce:GetDimensionValues",
          "ce:GetTags",
          "pricing:GetProducts",
          "ec2:Describe*",
          "s3:List*",
          "cloudwatch:Get*",
          "cloudwatch:List*"
        ],
        "Resource": "*"
      }
    ]
  }'
```

#### Optional: Telling MCP-initiated calls apart from direct API calls

AWS adds two global condition context keys to every request the MCP Server makes for you:

- `aws:ViaAWSMCPService` — **Boolean**, set to `true` for any request through an AWS managed MCP server.
- `aws:CalledViaAWSMCP` — **String**, contains the service principal of the specific MCP server (for example, `aws-mcp.amazonaws.com`).

If the IAM identity is shared between human and agent use, you can use these keys in `Deny` statements to keep the agent narrower than the human. For example, to block destructive S3 operations specifically when called through any AWS managed MCP server:

```json
{
  "Effect": "Deny",
  "Action": ["s3:DeleteBucket", "s3:DeleteObject"],
  "Resource": "*",
  "Condition": {
    "Bool": {"aws:ViaAWSMCPService": "true"}
  }
}
```

CloudTrail captures both keys, so the same conditions can be used to filter MCP-initiated activity in audit logs.

### Step 3: Configure AWS Credentials

Set up your AWS credentials for the MCP server to use:

#### Option A: AWS Profile (Recommended)

1. **Create AWS CLI profile**:

Open PowerShell (or Terminal on macOS/Linux) - you can run this from any directory:

```bash
aws configure --profile mcp-aws
```

Enter when prompted:
- AWS Access Key ID: `[your key from Step 1]`
- AWS Secret Access Key: `[your secret from Step 1]`
- Default region: `us-east-1` (North Virginia)
- Default output format: `json`

**Note**: No need to navigate to a specific folder - this command creates your AWS profile configuration in your home directory (`~/.aws/`) automatically.

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

**Windows PowerShell**:
```powershell
$env:AWS_ACCESS_KEY_ID="your-access-key"
$env:AWS_SECRET_ACCESS_KEY="your-secret-key"
$env:AWS_REGION="us-east-1"
```

**macOS/Linux (bash/zsh)**:
```bash
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_REGION="us-east-1"
```

**Note**: Environment variables set this way are temporary and only last for the current PowerShell/terminal session. For persistent credentials, use Option A (AWS Profile) instead.

### About `mcp-proxy-for-aws`

The configurations below all start with `uvx mcp-proxy-for-aws@latest`. That invokes the [MCP Proxy for AWS](https://github.com/aws/mcp-proxy-for-aws) — a small open-source program that runs locally on your machine. It bridges between the STDIO transport your MCP client speaks and the HTTPS endpoint where the AWS MCP Server lives, and it signs each request with SigV4 using the AWS profile from Step 3. You don't install it manually; `uvx` downloads and runs the latest version each time your MCP client starts. If you hit credential or signing errors, the proxy is the component handling that part of the flow.

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

Add the `aws-mcp` server to your existing `mcpServers` object. If you already have other MCP servers configured, add this as a new entry:

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

**If you have existing MCP servers**, your config should look like this:

```json
{
  "mcpServers": {
    "existing-server-1": {
      "command": "...",
      "env": {}
    },
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
    },
    "existing-server-2": {
      "command": "...",
      "env": {}
    }
  }
}
```

**Notes**:
- `mcp-aws` is the AWS profile name you created in Step 3 with `aws configure --profile mcp-aws`
- If you used a different profile name in Step 3, replace `mcp-aws` with your chosen profile name
- This tutorial uses `us-east-1` (North Virginia). You can change to your preferred AWS region
- Two regional endpoints are available at GA: `https://aws-mcp.us-east-1.api.aws/mcp` (US East / N. Virginia) and `https://aws-mcp.eu-central-1.api.aws/mcp` (Europe / Frankfurt). Pick the one closest to you for lower latency — both can call APIs in any region. EU-based teams will typically prefer the Frankfurt endpoint.

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

**Note**: `mcp-aws` is the AWS profile name from Step 3. If you used a different profile name, replace it here.

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

**Note**: `mcp-aws` is the AWS profile name from Step 3. If you used a different profile name, replace it in the `--env` parameter.

---

### Step 5: Multi-Step FinOps Workflows with `run_script`

The GA release introduces a `run_script` tool that runs short Python scripts in a server-side sandbox. The script inherits the caller's IAM permissions, has no network access outside AWS, and lets the agent chain multiple AWS API calls in a single round-trip rather than making one MCP call per operation.

For FinOps workflows that touch many APIs, this matters. A "top services by month-over-month growth" analysis would otherwise require many separate `call_aws` round-trips. With `run_script`, the agent generates one Python script that pulls the data, computes the deltas locally, and returns only the ranked result.

**Example prompt**:

```
Pull Cost Explorer data for the last 30 days and the prior 30 days,
then return the top 5 AWS services by month-over-month growth as a
ranked list with absolute and percentage change.
```

In response, the agent writes a single script that calls `GetCostAndUsage` for both periods, aggregates by service, sorts by delta, and returns the top 5 — replacing what would otherwise be several turns of back-and-forth and reducing both latency and token usage.

The script runs in an isolated sandbox: no network egress outside AWS, no persistent state between invocations, and only the IAM permissions you have granted to the user or role. For most FinOps work — cost aggregation, trend analysis, commitment portfolio reviews — `run_script` is more efficient than orchestrating dozens of individual API calls from the client side.

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
- Pull the relevant Skills for cost optimization
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
- Apply the relevant Reserved Instance Skill
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

### Issue: "AccessDenied" on `call_aws` or `run_script`

**Solution**: Check IAM permissions on the underlying AWS action — there is no separate `aws-mcp:*` permission to grant since GA.

1. Verify the policy is attached to your user:
```bash
aws iam list-user-policies --user-name mcp-aws-user
```

2. Get the policy document:
```bash
aws iam get-user-policy \
  --user-name mcp-aws-user \
  --policy-name AWSMCPAccess
```

3. Confirm the `Action` list covers the AWS API the agent is calling (e.g. `ce:GetCostAndUsage`, `pricing:GetProducts`).
4. If you used a `Condition` block with `aws:ViaAWSMCPService`, confirm the call is actually arriving through the MCP Server and not directly via the CLI/SDK.

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

For production use, scope the IAM policy to the specific AWS actions your agent needs. If the same IAM identity is used for both human and agent activity, follow the AWS-documented pattern of an explicit `Deny` for actions you don't want the agent to perform, scoped via the `aws:ViaAWSMCPService` (or `aws:CalledViaAWSMCP`) context key:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FinOpsReadOnlyAllow",
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
    },
    {
      "Sid": "BlockDestructiveActionsViaMCP",
      "Effect": "Deny",
      "Action": [
        "ec2:Terminate*",
        "ec2:Stop*",
        "rds:Delete*",
        "s3:DeleteBucket",
        "s3:DeleteObject"
      ],
      "Resource": "*",
      "Condition": {
        "Bool": {"aws:ViaAWSMCPService": "true"}
      }
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
- [Cost Analysis with Kiro CLI](./02-amazon-kiro-cli-cost-analysis.md) - AWS-native FinOps workflows
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
- ✅ Set up an AWS IAM user with standard, least-privilege FinOps permissions
- ✅ Configured AWS credentials securely
- ✅ Connected to the AWS MCP Server (now generally available)
- ✅ Accessed 15,000+ AWS APIs through natural language
- ✅ Used `run_script` to chain multiple AWS calls in a single round-trip
- ✅ Tested FinOps workflows backed by service-team-maintained Skills

**Key takeaway**: The AWS MCP remote server eliminates local installation complexity while providing comprehensive, managed access to all AWS services, documentation, and best practices - perfect for production FinOps workflows.

---

← [Back to Tutorials](./INDEX.md) | [Home](../README.md) | [Next: Multi-Cloud Setup](./04-azure-mcp-quickstart.md)
