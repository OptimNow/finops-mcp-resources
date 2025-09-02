# AWS Pricing MCP Server Installation Tutorial

## Introduction

This tutorial is designed for everyone who wants to try out Model Context Protocol (MCP) servers, including non-technical users. In this tutorial, MCP servers extend Claude's capabilities by connecting it to external data sources and tools. The AWS Pricing MCP server specifically allows Claude to fetch real-time AWS pricing information and generate cost analysis reports.

We've broken down the installation process into detailed, step-by-step instructions so that anyone can successfully set it up, regardless of their technical background. Each step includes explanations of what you're doing and why, along with troubleshooting tips for common issues.

## Step 0: Prerequisites

Before installing the MCP server, we need to set up the basic tools and software that it depends on. These prerequisites ensure everything works smoothly during installation.RetryClaude can make mistakes. Please double-check responses.

### - Understanding PowerShell
**Why PowerShell?**
PowerShell is Windows' built-in command-line tool for installing software and running system commands. We need it to install the AWS Pricing MCP server using Node.js package manager.

**How to open PowerShell:**
- Press Windows key + X
- Select "Windows PowerShell" or "Terminal"
- You'll see a window with white text on blue/black background

### - Install Node.js
**What is Node.js?**
Node.js lets your computer run JavaScript programs outside web browsers. The MCP server is built with Node.js.

**Check if installed:**
In PowerShell, type:
```powershell
node --version
```
**If not installed:**
- Download from https://nodejs.org (choose LTS version)
- Run installer with default settings
- Restart PowerShell and test again

### - Install AWS CLI
**What is AWS CLI?**
AWS CLI is Amazon's command-line tool that lets your computer connect to AWS services. The MCP server needs it to fetch pricing data.

**Install AWS CLI:**
- Download from https://aws.amazon.com/cli/
- Run installer with default settings
- Restart PowerShell after installation

**Verify installation:**
```powershell
aws --version
```

**Configure AWS user:**
AWS CLI needs a user with proper permissions to access pricing data. You can use your own AWS user or create a dedicated one. For detailed instructions on creating users and configuring access keys, see: https://github.com/OptimNow/finops-mcp-resources/blob/main/tooling-governance/security-privileges-aws.md

### - Install Claude Desktop
Reference the Claude Desktop setup at: https://github.com/OptimNow/finops-mcp-resources/blob/main/clients/2.%20claude.md

---

## Step 1: Install AWS Pricing MCP Server
In PowerShell, run
```powershell
npm install -g @aws/aws-pricing-mcp-server
```

**Success message should show:**
- Package installation progress
- Final message like "added 1 package in 2s"

**If you get "npm is not recognized as an internal or external command":**
- Node.js installation failed or PowerShell needs restart
- Close PowerShell completely
- Reopen PowerShell as Administrator
- Try the npm command again
- If still failing: https://awslabs.github.io/mcp/servers/aws-pricing-mcp-server/

---

## Step 2: Configure Claude Desktop

**Navigate to MCP configuration:**
- Open Claude Desktop
- Click your name/profile in bottom-left
- Select "Settings"
- Click "Developer" from the left menu
- Click "Edit Config" button

**Understanding the config file:**
The Claude Desktop config JSON file tells Claude which MCP servers to connect to. Each entry specifies a server name and how to run it.

---

## Step 3: Configure AWS User (Choose One Option)

### Option 1: Use Your Personal AWS User
Configure your existing credentials:
In PowerShell, run
```powershell
aws configure
```
Add to json config:
```json
{
  "mcpServers": {
    "aws-pricing": {
      "command": "aws-pricing-mcp-server",
      "args": []
    }
  }
}
```

### Option 2: Create Dedicated User (Recommended)
For better security, create a dedicated AWS user instead of using your personal credentials. First, check your current configuration with `aws sts get-caller-identity` to see which user is currently configured. Then create a new IAM user specifically for MCP operations, attach the pricing policy (already defined in this repository), generate access keys for this user, and configure it using `aws configure --profile mcp-user`.

Configure the dedicated user profile:
```powershell
aws configure --profile mcp-user
```
Update JSON config to use the dedicated profile:
```json
{
  "mcpServers": {
    "aws-pricing": {
      "command": "aws-pricing-mcp-server",
      "args": [],
      "env": {
        "AWS_PROFILE": "mcp-user"
      }
    }
  }
}
```
**Save and restart:**
- Save the configuration
- Completely close Claude Desktop
- Reopen Claude Desktop

---
## Step 4: Verify MCP Server is enabled
- Open Claude Desktop
- Click your name/profile in bottom-left
- Select "Settings"
- Look for "awslabs-aws-pricing-mcp-server" in the list
- Ensure the toggle is enabled (blue/on position)
- You should see "running" status next to it

---

## Step 5: Test the MCP Server

Try these prompts in Claude to verify everything works:

**Basic pricing query:**
``` What's the current pricing for EC2 t3.micro instances in us-east-1? ```
*Expected: Detailed pricing breakdown with hourly/monthly costs*

**Regional comparison:**
``` Compare S3 standard storage pricing between us-east-1 and eu-west-1 ```
*Expected: Side-by-side pricing comparison for both regions*

**Service analysis:**

``` Generate a cost report for RDS MySQL db.t3.micro instances ```
*Expected: Comprehensive cost analysis with recommendations*

If queries return pricing data, your MCP server is working correctly.
