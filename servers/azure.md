# ![Azure Logo](https://www.vectorlogo.zone/logos/microsoft_azure/microsoft_azure-icon.svg) Azure MCP Servers

## ⚠️ Important: Official Azure MCP Lacks FinOps Capabilities

The **official Azure MCP server** (`@azure/mcp`) is a **remote MCP server** that provides Azure resource management and monitoring, but **does NOT expose cost management or billing APIs**. If you need FinOps/cost analysis capabilities, see the community alternatives below.

## Available Azure MCP Servers

| **Server Name** | **Official?** | **Type** | **Cost/Billing APIs?** | **Production Ready?** | **Install** |
|:----------------|:--------------|:---------|:----------------------|:---------------------|:------------|
| [**@azure/mcp**](https://github.com/Azure/azure-mcp) | ✅ Yes (Microsoft) | Remote | ❌ No | ✅ Yes | [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=Azure%20MCP%20Server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40azure%2Fmcp%40latest%22%2C%22server%22%2C%22start%22%5D%7D) |
| [**azure-finops-mcp-server**](https://github.com/julianobarbosa/azure-finops-mcp-server) ⭐ **Recommended for FinOps** | ❌ No (Community) | Local | ✅ Yes | ✅ Yes | `uv pip install azure-finops-mcp-server` |
| [**finopshub-mcp**](https://github.com/mc5eamus/finopshub-mcp) | ❌ No (Community) | Local | ✅ Yes | ❌ No (Proof-of-concept) | Clone + manual setup |

### Which Server Should You Use?

**For resource management and monitoring**: Use the **official @azure/mcp** server. It provides access to Azure subscriptions, resource groups, storage, AKS, logs, metrics, and diagnostics. Easy to install via `npx`, officially supported by Microsoft.

**For FinOps/cost analysis**: Use **julianobarbosa/azure-finops-mcp-server** (community, but production-ready):
- 97.7% test coverage, 100% architecture compliance
- GitHub Actions CI/CD pipeline
- 4.9x faster performance with parallel processing
- Secure (credentials remain local via Azure CLI)
- Features: `get_cost` (cost analysis with tag filtering) and `run_finops_audit` (unused resource detection)

**Avoid finopshub-mcp** unless experimenting: early proof-of-concept with no test coverage or CI/CD pipeline.



---

## ⚙️ Prerequisites

### For Official @azure/mcp Server:
- An **Azure subscription**
- **Visual Studio Code** with **Cline extension** installed
- Azure permissions: **Reader** role at subscription or resource group level

### For Community FinOps Servers:
- **FinOps Hub** with **Data Explorer** enabled (for finopshub-mcp)
  - Follow Microsoft's guide: [Create a new FinOps Hub](https://learn.microsoft.com/en-us/cloud-computing/finops/toolkit/hubs/finops-hubs-overview#create-a-new-hub)
- **Azure CLI** configured with appropriate credentials (for azure-finops-mcp-server)
- **Python 3.10+** (for both community servers)

---

## 🔐 Required Azure Permissions

**For official @azure/mcp**:
- **Reader** role at subscription or resource group level

**For community FinOps servers**:
- **Cost Management Reader** role on the subscription to allow cost and usage queries
- Optionally, **Reader** role on resource groups for detailed resource-level insights

👉 See [full Azure MCP IAM/RBAC guidance](../governance/security-privileges-azure.md) for details.

---

## 🛠 Configuration Examples

### Official @azure/mcp Server

**For Cline** (`cline_mcp_settings.json`):
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

### Community azure-finops-mcp-server

Install via uv/pipx:
```bash
uv pip install azure-finops-mcp-server
# or
pipx install azure-finops-mcp-server
```

Then configure in your MCP client settings. See the [azure-finops-mcp-server documentation](https://github.com/julianobarbosa/azure-finops-mcp-server) for details.

---

## 📚 Tutorials

- [Azure MCP Quickstart Tutorial](../tutorials/04-azure-mcp-quickstart.md) - Step-by-step guide with Cline setup and comparison of all three servers
