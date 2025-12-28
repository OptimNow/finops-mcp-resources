← [Back to Tutorials](./INDEX.md) | [Home](../README.md)

---

# AWS Cost Analysis with Amazon Q

**Tutorial 2 of 7** | ⏱️ **Time**: 20-30 minutes | 💻 **Level**: Beginner

**Last Updated**: January 2026

---

## 🎯 What You'll Learn

- Use Amazon Q CLI for AWS cost analysis with MCP
- Integrate Amazon Q with MCP servers for FinOps workflows
- Execute automated cost estimation queries using natural language
- Apply AI-driven best practices for cloud cost management
- Leverage AWS-native assistant capabilities for billing insights

---

## Overview

**Blog Post**: [AWS Costs Estimation using Amazon Q CLI and AWS Cost Analysis MCP](https://aws.amazon.com/fr/blogs/machine-learning/aws-costs-estimation-using-amazon-q-cli-and-aws-cost-analysis-mcp/)
**Status**: ✅ Official AWS Blog

Learn how to combine Amazon Q CLI with MCP servers for intelligent, AI-driven cost analysis using natural language queries.

---

## Prerequisites

- **Windows users**: WSL2 (Windows Subsystem for Linux) installed
- **macOS/Linux users**: Terminal access
- **Python 3.10+** (for running MCP servers)
- **AWS Builder ID** (free, no AWS account required)
- **uv** package manager (we'll install this)

---

## Step 1: Install WSL2 (Windows Only)

**macOS/Linux users**: Skip to Step 2

**Windows users**: Amazon Q CLI requires WSL2 (Windows Subsystem for Linux) since there's no native Windows version yet.

1. **Open PowerShell as Administrator**

2. **Install WSL2**:
   ```powershell
   wsl --install
   ```

3. **Restart your computer** when prompted

4. **Set up Ubuntu**: After restart, Ubuntu will auto-launch. Create a username and password

5. **Update Ubuntu**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

---

## Step 2: Install Amazon Q CLI

### Option A: macOS (Homebrew)

```bash
brew install --cask amazon-q
```

### Option B: macOS (DMG)

Download directly: https://desktop-release.q.us-east-1.amazonaws.com/latest/Amazon%20Q.dmg

### Option C: Linux / WSL2

1. **Download Amazon Q CLI**:
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf "https://desktop-release.q.us-east-1.amazonaws.com/latest/q-x86_64-linux.zip" -o "q.zip"
   ```

2. **Unzip the package**:
   ```bash
   unzip q.zip
   ```

3. **Run the installer**:
   ```bash
   cd q
   ./install.sh
   ```

4. **Restart your terminal** (or WSL2)

5. **Verify installation**:
   ```bash
   q --version
   ```

   You should see output like: `Amazon Q Developer CLI v1.x.x`

---

## Step 3: Authenticate with AWS Builder ID

1. **Start the login process**:
   ```bash
   q login
   ```

2. **Open the generated link**: Amazon Q will display a URL and a verification code

3. **Login with your email**:
   - Open the link in your browser
   - Sign in with your email (or create a free AWS Builder ID)
   - Enter the verification code when prompted

4. **Confirm authentication**: Return to your terminal - you should see a success message

---

## Step 4: Install uv Package Manager

Amazon Q uses MCP servers that run via `uvx`. Install `uv` (Python package manager):

**macOS/Linux/WSL2**:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**After installation**, restart your terminal or run:
```bash
source ~/.bashrc  # or ~/.zshrc on macOS
```

**Verify installation**:
```bash
uvx --version
```

---

## Step 5: Configure MCP Server for Amazon Q

1. **Create the MCP configuration directory**:
   ```bash
   mkdir -p ~/.aws/amazonq
   ```

2. **Create the MCP configuration file**:
   ```bash
   nano ~/.aws/amazonq/mcp.json
   ```

   Or use any text editor to create `~/.aws/amazonq/mcp.json`

3. **Add the AWS Pricing MCP server configuration**:

   ```json
   {
     "mcpServers": {
       "aws-pricing": {
         "command": "uvx",
         "args": ["mcp-server-aws-pricing"],
         "env": {
           "AWS_REGION": "us-east-1",
           "AWS_PROFILE": "default"
         }
       }
     }
   }
   ```

   **Note**: Replace `default` with your AWS CLI profile name if you use a different one (e.g., `finops`)

4. **Save and exit** (Ctrl+X, then Y, then Enter if using nano)

---

## Step 6: Test Your Setup

1. **Start Amazon Q CLI chat**:
   ```bash
   q chat
   ```

2. **Verify MCP server loaded**: You should see a message indicating the AWS Pricing MCP server is loaded

3. **Test with a cost analysis query**:
   ```
   Please create a cost analysis for a simple web application with:
   - Application Load Balancer
   - Two t3.medium EC2 instances
   - RDS db.t3.medium MySQL database
   - Assume 730 hours of usage per month
   - Moderate traffic of about 100 GB data transfer
   ```

4. **Trust the MCP tool**: Amazon Q will ask for permission to use the MCP tool. Type `t` to trust it.

5. **Review the cost analysis**: Amazon Q should generate a detailed cost breakdown with real-time AWS pricing data

---

## Example Queries to Try

Once your setup is working, try these queries:

### Basic Cost Estimation
```
What's the monthly cost of running a t3.large EC2 instance in us-east-1?
```

### Multi-Service Analysis
```
Estimate costs for:
- 5 t3.medium EC2 instances
- 500 GB EBS storage
- 1 TB S3 storage
- 200 GB data transfer out
```

### Cost Comparison
```
Compare the cost of running a workload on t3.medium vs t3.large instances in us-east-1 and us-west-2
```

### Optimization Recommendations
```
I'm running 10 t3.xlarge instances 24/7. Should I use Reserved Instances or Savings Plans?
```

---

## Troubleshooting

### Amazon Q CLI not found after installation
- **Solution**: Restart your terminal or run `source ~/.bashrc` (Linux/WSL) or `source ~/.zshrc` (macOS)

### MCP server not loading
- **Check the config file**: Ensure `~/.aws/amazonq/mcp.json` has valid JSON syntax
- **Verify uvx is installed**: Run `uvx --version`
- **Check the server name**: Must be `mcp-server-aws-pricing` (not `awslabs-aws-pricing-mcp-server`)

### "Permission denied" when running commands
- **WSL users**: Don't run commands with `sudo` unless specifically instructed
- **Check file permissions**: Ensure your user owns the `~/.aws/amazonq/` directory

### Cost analysis returns errors
- **Verify AWS region**: Ensure your `AWS_REGION` in `mcp.json` is valid (e.g., `us-east-1`)
- **Check AWS profile**: If using a specific AWS CLI profile, update the `AWS_PROFILE` value

---

## What's Next?

- **Tutorial 3**: [Multi-Agent FinOps with Amazon Nova](./03-finops-multi-agent-nova.md) - Build advanced multi-agent cost optimization systems
- **Tutorial 7**: [AWS MCP Remote Server](./07-aws-mcp-remote-server.md) - Try the new remote MCP server (no local installation needed!)

---

## Resources

- [AWS Blog: Amazon Q CLI + MCP for Cost Analysis](https://aws.amazon.com/blogs/machine-learning/aws-costs-estimation-using-amazon-q-cli-and-aws-cost-analysis-mcp/)
- [Amazon Q Developer CLI on GitHub](https://github.com/aws/amazon-q-developer-cli)
- [AWS Pricing MCP Server Documentation](https://github.com/awslabs/mcp-server-aws-pricing)
- [How to Install Amazon Q CLI on Windows](https://builder.aws.com/content/2ySpxVfiIsy46THpP6YdlYgQZpD/how-to-install-amazon-q-cli-on-windows)


