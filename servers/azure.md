# Azure MCP Servers

Query and optimize Azure cost and usage data with MCP servers.

---

## 🔧 Azure MCP Servers

| Server | Description | Install |
|:-------|:------------|:---------|
| [Azure Cost MCP](https://github.com/awslabs/mcp/tree/main/src/azure-cost-mcp-server) | Query cost and usage data from Azure Cost Management | [![Cursor](https://img.shields.io/badge/Install-Cursor-red?logo=cursor&logoColor=white)](link-to-cursor) <br> [![VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](link-to-vs) |
| [Azure Resource MCP](https://github.com/awslabs/mcp/tree/main/src/azure-resource-mcp-server) | Query resource usage and optimization data | [![Cursor](https://img.shields.io/badge/Install-Cursor-red?logo=cursor&logoColor=white)](link-to-cursor) <br> [![VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](link-to-vs) |

---

## ⚙️ Prerequisites
- [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) installed and logged in (`az login`)  
- MCP client (Cursor, VS Code MCP extension, Claude Desktop)  
- Subscription access with **Cost Management Reader** role  

---

## 🔐 Required Azure Permissions
- **Minimal**: assign **Cost Management Reader** at the subscription level.  
- **Optional**: add **Reader** role on resource groups if optimization data is needed.  
👉 [Full Azure MCP IAM/RBAC guide](../tooling-governance/security-privileges-azure.md)

---

## 🛠 Example Config (`mcp.json`)
```json
{
  "mcpServers": {
    "azure-cost": {
      "command": "uvx",
      "args": ["azure-cost-mcp-server@latest"],
      "env": {
        "AZURE_SUBSCRIPTION_ID": "<your-subscription-id>"
      }
    }
  }
}
