← [Back to Tutorials](./INDEX.md) | [Home](../README.md)

---

# AWS Cost Analysis with Kiro CLI

**Tutorial 2 of 7** | ⏱️ **Time**: 20-30 minutes | 💻 **Level**: Beginner

**Last Updated**: January 2026

---

## 🎯 What You'll Learn

- Use Kiro CLI (formerly Amazon Q CLI) for AWS cost analysis with MCP
- Integrate Kiro CLI with the AWS Remote MCP server for FinOps workflows
- Execute automated cost estimation queries using natural language
- Apply AI-driven best practices for cloud cost management
- Leverage AWS-native remote MCP server capabilities for billing insights

---

## Overview

**Official Resources**:
- [AWS Remote MCP Server Documentation](https://docs.aws.amazon.com/mcp/)
- [Kiro CLI Website](https://kiro.dev/)

Learn how to combine Kiro CLI with the AWS Remote MCP server for intelligent, AI-driven cost analysis using natural language queries. This tutorial uses the same remote MCP server from Tutorial 7, eliminating local installation complexity.

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

## Step 2: Install Kiro CLI

**Note**: Kiro CLI is the successor to Amazon Q CLI. If you already have Amazon Q CLI installed, you can update it to Kiro CLI.

### Option A: macOS (Homebrew)

```bash
brew install --cask amazon-q
# After installation, run: q update
```

### Option B: macOS (DMG)

Download directly: https://desktop-release.q.us-east-1.amazonaws.com/latest/Amazon%20Q.dmg

### Option C: Linux / WSL2

1. **Download Kiro CLI (via Amazon Q installer)**:
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

4. **Update to Kiro CLI** (if prompted):
   ```bash
   q update
   ```

5. **Restart your terminal** (or WSL2)

6. **Verify installation**:
   ```bash
   kiro-cli --version
   # Or if still using 'q' command:
   q --version
   ```

   You should see Kiro CLI version information

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

## Step 5: Configure AWS Remote MCP Server for Kiro CLI

**Important**: Complete Tutorial 7 first to set up IAM permissions for the AWS Remote MCP server. You'll need:
- IAM user with `aws-mcp:InvokeMcp` permission
- AWS profile configured (Step 3 of Tutorial 7)

**Note**: Kiro CLI reads MCP configuration from `~/.kiro/settings/mcp.json`, not from `~/.aws/amazonq/mcp.json`.

1. **Create the MCP configuration directory**:
   ```bash
   mkdir -p ~/.kiro/settings
   ```

2. **Create the MCP configuration file**:
   ```bash
   nano ~/.kiro/settings/mcp.json
   ```

   Or use any text editor to create `~/.kiro/settings/mcp.json`

3. **Add the AWS Remote MCP server configuration**:

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
   - `mcp-aws` is the AWS profile name from Tutorial 7. Replace it if you used a different name
   - This is the same remote MCP server configuration from Tutorial 7
   - No local installation needed - the server is hosted by AWS!
   - **Important**: Make sure the `AWS_PROFILE` value matches an AWS profile configured in your WSL environment

4. **Save and exit** (Ctrl+X, then Y, then Enter if using nano)

---

## Step 6: Test Your Setup

1. **Start Kiro CLI chat**:
   ```bash
   kiro-cli chat
   # Or if still using the 'q' command:
   # q chat
   ```

2. **Verify MCP server loaded**: You should see a message indicating the AWS Remote MCP server is loaded

3. **Test with a cost analysis query**:
   ```
   What's the monthly cost of running a t3.large EC2 instance in us-east-1?
   ```

4. **Trust the MCP tool**: Kiro CLI will ask for permission to use the MCP tool. Type `t` to trust it.

5. **Try a more complex query**:
   ```
   Please create a cost analysis for a simple web application with:
   - Application Load Balancer
   - Two t3.medium EC2 instances
   - RDS db.t3.medium MySQL database
   - Assume 730 hours of usage per month
   - Moderate traffic of about 100 GB data transfer
   ```

6. **Review the cost analysis**: Kiro CLI should generate a detailed cost breakdown using the AWS Remote MCP server's access to 15,000+ AWS APIs

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

### Kiro CLI / Amazon Q CLI not found after installation
- **Solution**: Restart your terminal or run `source ~/.bashrc` (Linux/WSL) or `source ~/.zshrc` (macOS)
- **Update to Kiro**: Run `q update` to upgrade from Amazon Q CLI to Kiro CLI

### MCP server not loading ("connection closed: initialize response")
- **Check the correct config file**: Kiro CLI reads from `~/.kiro/settings/mcp.json`, NOT from `~/.aws/amazonq/mcp.json`
- **Check IAM permissions**: Ensure your IAM user has `aws-mcp:InvokeMcp` permission (see Tutorial 7, Step 2)
- **Verify AWS profile**: Confirm the profile name in `~/.kiro/settings/mcp.json` matches your AWS CLI profile from Tutorial 7
- **Check the config file**: Ensure `~/.kiro/settings/mcp.json` has valid JSON syntax
- **Verify uvx is installed**: Run `uvx --version`
- **Test the remote server**: Try using the same config in Claude Desktop (Tutorial 7) to verify it works
- **Check AWS credentials in WSL**: Run `aws configure list --profile mcp-aws` to ensure the profile exists in your WSL environment

### "Permission denied" when running commands
- **WSL users**: Don't run commands with `sudo` unless specifically instructed
- **Check file permissions**: Ensure your user owns the `~/.aws/amazonq/` directory

### Cost analysis returns authorization errors
- **IAM Policy**: Verify your IAM user has the `aws-mcp:InvokeMcp` policy attached
- **AWS Profile**: Ensure the profile name in `mcp.json` matches your configured AWS profile
- **Credentials**: Run `aws configure list --profile mcp-aws` to verify credentials are set

---

## What's Next?

- **Tutorial 3**: [Multi-Agent FinOps with Amazon Nova](./03-finops-multi-agent-nova.md) - Build advanced multi-agent cost optimization systems
- **Tutorial 7**: [AWS MCP Remote Server](./07-aws-mcp-remote-server.md) - Try the new remote MCP server (no local installation needed!)

---

## Resources

- [Kiro CLI Official Website](https://kiro.dev/)
- [Kiro CLI License](https://kiro.dev/license)
- [AWS Remote MCP Server Documentation](https://docs.aws.amazon.com/mcp/)
- [Tutorial 7: AWS MCP Remote Server Setup](./07-aws-mcp-remote-server.md) - Complete IAM setup guide
- [Amazon Q Developer CLI (Legacy)](https://github.com/aws/amazon-q-developer-cli)


