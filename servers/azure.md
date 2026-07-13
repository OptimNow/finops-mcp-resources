# Azure MCP Servers for Cloud Cost Management

**Last Updated**: July 2026

A guide to Azure MCP servers for FinOps, cloud cost management, and AI-powered cost analysis. Covers the official Azure MCP servers (including the new ARM / FinOps MCP Server preview) and community alternatives for cost optimization and billing workflows.

> **🆕 What changed in mid-2026:** Microsoft now has an official FinOps story for MCP. On June 23, 2026, Azure announced the **Azure FinOps MCP Server (public preview)** — the FinOps-facing packaging of the **Azure Resource Manager (ARM) MCP Server** (public preview since May 2026) — to "connect cost and usage intelligence into agent workflows." Separately, the official `@azure/mcp` server gained an **Azure retail pricing tool** for cost estimation. See [From insight to action](https://azure.microsoft.com/en-us/blog/from-insight-to-action-the-next-phase-of-agentic-cloud-operations/) and [Introducing the ARM MCP Server](https://techcommunity.microsoft.com/blog/azuregovernanceandmanagementblog/introducing-the-azure-resource-manager-mcp-server/4517521).

## Which Azure MCP server should I use for FinOps?

Until mid-2026, no official Microsoft MCP server exposed cost APIs — that is no longer true, but the picture is nuanced:

- The **official `@azure/mcp` server** now includes a **retail pricing tool** (list prices, reservation and savings plan rates) — good for **cost estimation before deployment**, but it still does **not** expose your actual spend (Cost Management / billing data).
- The **official ARM MCP Server** (branded **Azure FinOps MCP Server** for cost workflows, public preview) gives agents natural-language **Azure Resource Graph** queries across your whole estate — powerful for inventory-driven waste detection (orphaned disks, unattached IPs, untagged resources) — with cost and usage intelligence rolling out under the FinOps branding.
- For querying your **actual cost/billing data today**, the community **azure-finops-mcp-server** remains the most direct option.

## Available Azure MCP Servers

| **Server Name** | **Official?** | **Type** | **Cost/Billing APIs?** | **Production Ready?** | **Install** |
|:----------------|:--------------|:---------|:----------------------|:---------------------|:------------|
| [**ARM MCP Server**](https://github.com/Azure/Azure-Resource-Manager-MCP) (aka **Azure FinOps MCP Server**) 🆕 | ✅ Yes (Microsoft) | Remote | 🟡 Rolling out (preview) | 🟡 Public preview | Open [aka.ms/JoinARMMCP](https://aka.ms/JoinARMMCP) in VS Code |
| [**@azure/mcp**](https://github.com/Azure/azure-mcp) | ✅ Yes (Microsoft) | Remote | ⚠️ Retail pricing only (no billing data) | ✅ Yes | [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=Azure%20MCP%20Server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40azure%2Fmcp%40latest%22%2C%22server%22%2C%22start%22%5D%7D) |
| [**azure-finops-mcp-server**](https://github.com/julianobarbosa/azure-finops-mcp-server) ⭐ **Recommended for billing data today** | ❌ No (Community) | Local | ✅ Yes (Cost Management) | ✅ Yes | `uv pip install azure-finops-mcp-server` |
| [**finopshub-mcp**](https://github.com/mc5eamus/finopshub-mcp) | ❌ No (Community) | Local | ✅ Yes | ❌ No (Proof-of-concept) | Clone + manual setup |

### Which Server Should You Use?

**For estate-wide inventory and waste detection (official, preview)**: Use the **ARM MCP Server**. It generates, validates, and executes Azure Resource Graph queries from natural language ("show me all disks not attached to a VM", "find resources created in the last 30 days without required tags") — the backbone of inventory-driven FinOps. Azure is expanding it with cost and usage intelligence under the **Azure FinOps MCP Server** branding. Caveats: it currently requires **VS Code + GitHub Copilot** (Claude support announced as next), and it also includes **ARM template deployment tools** — see the governance note below.

**For pre-deployment cost estimation (official)**: Use the **@azure/mcp** server's [pricing tool](https://learn.microsoft.com/en-us/azure/developer/azure-mcp-server/tools/azure-pricing) — retail prices by SKU/region, including reservation and savings plan rates. Also your go-to for resource management and monitoring (subscriptions, resource groups, storage, AKS, logs, metrics, diagnostics).

**For actual cost/billing analysis today**: Use **julianobarbosa/azure-finops-mcp-server** (community, but production-ready):
- 97.7% test coverage, 100% architecture compliance
- GitHub Actions CI/CD pipeline
- 4.9x faster performance with parallel processing
- Secure (credentials remain local via Azure CLI)
- Features: `get_cost` (cost analysis with tag filtering) and `run_finops_audit` (unused resource detection)

**For FinOps hubs users**: Microsoft's official path is to connect AI agents to your hub's Data Explorer via the Azure MCP server — see [Configure AI agents for FinOps hubs](https://learn.microsoft.com/en-us/cloud-computing/finops/toolkit/hubs/configure-ai). **Avoid finopshub-mcp** unless experimenting: early proof-of-concept with no test coverage or CI/CD pipeline, largely superseded by the official guidance.

> **⚠️ Governance note on the ARM MCP Server:** unlike the read-only community FinOps servers, the ARM MCP Server ships **write tools** (`create_template_deployment`, `cancel_arm_template_deployment`) alongside its Resource Graph query tools. All operations run under the signed-in user's RBAC, and Microsoft documents using Azure Policy to block deployments via the MCP server. For FinOps use, run it with a **Reader + Resource Graph-scoped identity** and/or disable the deployment tools in your MCP client.

---

## ⚙️ Prerequisites

### For Official ARM MCP Server (public preview):
- **VS Code** with a **GitHub Copilot subscription** (other clients, including Claude, announced as coming)
- Valid **Azure account**; sign-in happens on install via [aka.ms/JoinARMMCP](https://aka.ms/JoinARMMCP)

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

**For official ARM MCP Server**:
- **Reader** + **Resource Graph** read access on the target scope for query tools
- **Contributor** on target resource groups only if you intend to use the deployment tools (not recommended for FinOps-only use)
- All operations execute as the signed-in user — the agent's permissions are exactly your permissions

**For official @azure/mcp**:
- **Reader** role at subscription or resource group level (retail pricing queries need no billing permissions — they hit public price lists)

**For community FinOps servers**:
- **Cost Management Reader** role on the subscription to allow cost and usage queries
- Optionally, **Reader** role on resource groups for detailed resource-level insights

👉 See the [governance section](../governance/INDEX.md) for security best practices and IAM guidance.

---

## 🛠 Configuration Examples

### Official ARM MCP Server (public preview)

1. Open [https://aka.ms/JoinARMMCP](https://aka.ms/JoinARMMCP) — VS Code launches automatically
2. Click **Install** under *Azure Resource Manager MCP Server*
3. Sign in with your Azure credentials
4. In VS Code Chat, open **Configure Tools** and enable the six ARM MCP tools (for FinOps-only use, consider leaving the three deployment tools disabled)

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

- [Azure MCP Quickstart Tutorial](../tutorials/04-azure-mcp-quickstart.md) - Step-by-step guide with Cline setup and comparison of all four servers
