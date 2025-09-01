# ![Azure Logo](https://www.vectorlogo.zone/logos/microsoft_azure/microsoft_azure-icon.svg) Azure MCP Server

The Azure MCP server lets you query **Azure Cost Management + FinOps Hub** data directly from your MCP client.  
Ideal for cost analysis, optimization, and integration into your FinOps workflows.  

---

## 🔧 Azure MCP Server

| **Server Name** | **Description** | **Install** |
|:----------------|:----------------|:-------------|
| [Azure MCP Server](https://github.com/Azure/azure-mcp?tab=readme-ov-file#-azure-mcp-server) | Query Azure cost data from **FinOps Hub** and explore optimization insights. | [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=Azure%20MCP%20Server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40azure%2Fmcp%40latest%22%2C%22server%22%2C%22start%22%5D%7D) |

---

## ⚙️ Prerequisites

Before installing, make sure you have:  
- **FinOps Hub** with **Data Explorer** enabled in your Azure subscription.  
  - If you don’t already have one, follow Microsoft’s guide: [Create a new FinOps Hub](https://learn.microsoft.com/en-us/cloud-computing/finops/toolkit/hubs/finops-hubs-overview#create-a-new-hub)  
- **Visual Studio Code** with [GitHub Copilot](https://github.com/features/copilot) activated.  
- An MCP-compatible client (VS Code MCP extension recommended).  

---

## 🔐 Required Azure Permissions

- Assign the **Cost Management Reader** role on the subscription to allow cost and usage queries.  
- For deeper resource-level insights, you may also need the **Reader** role on selected resource groups or services.  

👉 See [full Azure MCP IAM/RBAC guidance](../tooling-governance/security-privileges-azure.md) for details.  

---

## 🛠 Example Config (`mcp.json`)

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
