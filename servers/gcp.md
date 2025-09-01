# ![GCP Logo](https://www.vectorlogo.zone/logos/google_cloud/google_cloud-icon.svg) GCP MCP Server

The GCP MCP server lets you query **Google Cloud Billing Export** data for FinOps use cases such as cost analysis, anomaly detection, and optimization.  

---

## 🔧 GCP MCP Server

| **Server Name** | **Description** | **Install** |
|:----------------|:----------------|:-------------|
| [GCP MCP Server](https://github.com/GoogleCloudPlatform/gcp-mcp) | Query cost and usage data from GCP Billing Export in BigQuery. | [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=GCP%20MCP%20Server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40gcp%2Fmcp%40latest%22%5D%7D) |

---

## ⚙️ Prerequisites

Before installing, ensure:  
- **Billing Export to BigQuery** is enabled in your GCP project ([guide](https://cloud.google.com/billing/docs/how-to/export-data-bigquery)).  
- **gcloud CLI** installed and authenticated (`gcloud auth login`).  
- **Visual Studio Code** (with MCP support) or another MCP-compatible client.  

---

## 🔐 Required GCP Permissions

- **Minimal**: `roles/billing.viewer` to read billing data.  
- **If querying BigQuery exports**: `roles/bigquery.dataViewer` on the dataset used for Billing Export.  

👉 See [full GCP MCP IAM guidance](../tooling-governance/security-privileges-gcp.md) for details.  

---

## 🛠 Example Config (`mcp.json`)

```json
{
  "mcpServers": {
    "gcp-mcp": {
      "command": "npx",
      "args": ["-y", "@gcp/mcp@latest"],
      "env": {
        "GOOGLE_PROJECT_ID": "<your-project-id>",
        "GOOGLE_APPLICATION_CREDENTIALS": "<path-to-service-account.json>"
      }
    }
  }
}
