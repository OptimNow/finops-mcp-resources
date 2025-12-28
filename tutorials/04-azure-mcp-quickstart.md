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

The Azure MCP server lets you query **FinOps Hub** and **Azure Cost Management** data directly in your MCP client (like VS Code). Use it for:

- Cost analysis
- Optimization insights
- FinOps workflow automation

------



## 2. Prerequisites

- An **Azure subscription** with a [FinOps Hub](https://learn.microsoft.com/en-us/cloud-computing/finops/toolkit/hubs/finops-hubs-overview?utm_source=chatgpt.com#create-a-new-hub) and **Data Explorer** enabled. *👉 For setup details, see the [Azure FinOps Hub guide](https://learn.microsoft.com/en-us/cloud-computing/finops/toolkit/hubs/finops-hubs-overview?utm_source=chatgpt.com#create-a-new-hub).*
- **VS Code** installed and running. 
- Azure permissions: assign yourself or your service principal the **Cost Management Reader** role (and optionally **Reader** at resource group level for more detail)



------

## 3. Installation (1-click in VS Code)

1. Open this repo in VS Code.
2. Click the **Install in VS Code** badge : [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=Azure+MCP+Server&config={"command":"npx","args":["-y","@azure%2Fmcp@latest","server","start"]}&utm_source=chatgpt.com)

That’s it — VS Code sets up the MCP server for you.

------



## 4. Minimal Config

If you want to check it manually, add this to your `mcp.json`:

```
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

------



## 5. Test it

In your MCP client, try:

```
Give me an overview of my Azure FinOps setup: list my resource groups, storage accounts, and then dive into the <your storage account Id> storage account to show containers and any cost report files
```

You should get this:

[![img](https://cdn.loom.com/sessions/thumbnails/bcc9417c27cf448a94ca8718b0c66130-d6ba898b6e3b13f4-full-play.gif)](https://www.loom.com/share/bcc9417c27cf448a94ca8718b0c66130)



---

## 6. Next steps

- Review role requirements: [Azure MCP IAM guidance](../governance/security-privileges-azure.md)
- Explore FinOps Hub queries in the Data Explorer
- Extend with tagging, budgets, and anomaly detection

  
