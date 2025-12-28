← [Back to Tutorials](./INDEX.md) | [Home](../README.md)

---

# Azure MCP Server — Quickstart

**Tutorial 4 of 7** | ⏱️ **Time**: 20-30 minutes | 💻 **Level**: Beginner

**Last Updated**: January 2026

---

## 🎯 What You'll Learn

- Set up the Azure MCP server for FinOps Hub integration
- Configure Azure Cost Management API access in VS Code
- Query Azure billing data using natural language
- Analyze cost reports and optimization insights
- Implement FinOps workflow automation for Azure

---

## 1. Overview

The **official Azure MCP server** (`@azure/mcp`) is a **remote MCP server** that provides Azure resource management and monitoring capabilities. This means it runs via `npx` and automatically downloads the latest version from npm—no local installation needed.

**⚠️ Important Limitation:** The official Azure MCP server **does NOT currently expose cost management or billing APIs**. For FinOps/cost analysis, see the comparison table in section 6 for community alternatives.

**What the Azure MCP server DOES provide:**
- Resource monitoring (logs, metrics, activity logs)
- Resource management (subscriptions, resource groups, AKS, storage, etc.)
- Diagnostics and health monitoring
- Azure resource queries and inspection

------



## 2. Prerequisites

- An **Azure subscription**
- **VS Code** installed and running
- **Cline extension** installed in VS Code (see step 3a below if you don't have it yet)
- Azure permissions: **Reader** role at subscription or resource group level



------

## 3. Installation

### 3a. Install Cline (MCP Client)

If you don't have Cline installed yet:

1. Open VS Code
2. Go to Extensions (Ctrl+Shift+X)
3. Search for "Cline"
4. Click **Install** on the Cline extension
5. After installation, you'll see a chat icon in the VS Code sidebar

### 3b. Install Azure MCP Server (1-click)

1. Open this repo in VS Code
2. Click the **Install in VS Code** badge: [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=Azure+MCP+Server&config={"command":"npx","args":["-y","@azure%2Fmcp@latest","server","start"]}&utm_source=chatgpt.com)

That's it — VS Code sets up the MCP server for you.

------



## 4. Verify Installation (Optional)

You can verify the installation by checking your MCP configuration file.

**Important:** If you're using **Cline**, the configuration is stored at:
- `C:\Users\<YourUsername>\AppData\Roaming\Code\User\globalStorage\saoudrizwan.claude-dev\settings\cline_mcp_settings.json`

**For other MCP clients**, check:
- Press `Ctrl+P` in VS Code (Quick Open)
- Paste this path: `C:\Users\<YourUsername>\AppData\Roaming\Code\User\mcp.json`
- Replace `<YourUsername>` with your actual Windows username

**Expected configuration:**

The one-click install should have added this configuration:

```json
{
  "mcpServers": {
    "azure-mcp": {
      "command": "npx",
      "args": ["-y", "@azure/mcp@latest", "server", "start"],
      "env": {
        "AZURE_SUBSCRIPTION_ID": "<your-subscription-id>"
      }
    }
  }
}
```

**Note:** You can add your Azure subscription ID to the `env` section for better functionality.

------



## 5. Test it

**Where to test:** Open the **Cline chat panel** in VS Code (click the chat icon in the left sidebar).

In the Cline chat, try this prompt:

```
List my Azure subscriptions and resource groups, then show me the storage accounts in my primary resource group
```

**Example queries you can try:**
- "Show me all resource groups in my subscription"
- "List storage accounts and their containers"
- "Get recent activity logs for my resources"
- "Show metrics for my AKS clusters"

**Note:** For cost and billing queries, the official Azure MCP server doesn't currently support these. See section 6 for community alternatives that provide FinOps capabilities.



---

## 6. Comparison with other Azure MCP servers

There are community-created Azure MCP servers that **DO provide FinOps/cost management features**, but they are **not official** and require **local installation**:

| MCP Server | Official? | Type | Cost/Billing APIs? | Production Ready? | Installation |
|------------|-----------|------|-------------------|-------------------|--------------|
| **@azure/mcp** (this tutorial) | ✅ Yes (Microsoft) | Remote | ❌ No | ✅ Yes | `npx @azure/mcp@latest` |
| **[julianobarbosa/azure-finops-mcp-server](https://github.com/julianobarbosa/azure-finops-mcp-server)** ⭐ **Recommended for FinOps** | ❌ No (Community) | Local | ✅ Yes | ✅ Yes | Python package (`uv pip install azure-finops-mcp-server`) |
| **[mc5eamus/finopshub-mcp](https://github.com/mc5eamus/finopshub-mcp)** | ❌ No (Community) | Local | ✅ Yes | ❌ No (Proof-of-concept) | Clone repo + manual setup (Python, Docker) |

**When to use each:**

- **Use @azure/mcp (official)** for:
  - Resource management and monitoring
  - Official Microsoft support
  - Easy remote installation
  - Long-term maintenance guarantee

- **Use julianobarbosa/azure-finops-mcp-server (⭐ recommended community option)** for:
  - **Cost analysis and billing data**
  - FinOps workflow automation
  - Budget monitoring and cost optimization
  - **Production-grade quality**: 97.7% test coverage, 100% architecture compliance, GitHub Actions CI/CD
  - **Performance optimized**: 4.9x faster with parallel processing
  - **Secure**: All credentials remain local via Azure CLI
  - Features: `get_cost` (cost analysis with tag filtering) and `run_finops_audit` (unused resource detection)

- **Avoid mc5eamus/finopshub-mcp** unless experimenting:
  - Early proof-of-concept (only 4 commits, 2 stars)
  - No test coverage or CI/CD pipeline
  - Experimental features marked as WIP
  - Not recommended for production use

---

## 7. Next steps

- Review role requirements: [Azure MCP IAM guidance](../governance/security-privileges-azure.md)
- Explore Azure resource queries and monitoring capabilities
- **For FinOps/cost analysis**: Set up [julianobarbosa/azure-finops-mcp-server](https://github.com/julianobarbosa/azure-finops-mcp-server) (recommended in section 6)


