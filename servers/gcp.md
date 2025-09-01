# ![GCP Logo](https://www.vectorlogo.zone/logos/google_cloud/google_cloud-icon.svg) GCP MCP Server

The **community GCP MCP server** lets you query **Google Cloud Billing Export** data for FinOps use cases such as cost analysis, anomaly detection, and optimization.  

---

## 🔧 GCP MCP Server

| **Server Name** | **Description** | **Install** |
|:----------------|:----------------|:-------------|
| [GCP MCP Server](https://github.com/krzko/google-cloud-mcp) | Query cost and usage data from **GCP Billing Export in BigQuery**. | [![Install VS Code](https://img.shields.io/badge/Install-VS%20Code-blue?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=GCP%20MCP%20Server&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22google-cloud-mcp%40latest%22%5D%7D) |

---

## ⚙️ Prerequisites

Before installing, ensure:  
- **Billing Export to BigQuery** is enabled in your GCP project ([guide](https://cloud.google.com/billing/docs/how-to/export-data-bigquery)).  
- **gcloud CLI** installed and authenticated (`gcloud auth login`).  
- **Visual Studio Code** (with MCP support) or another MCP-compatible client.  
- A **Service Account** with minimal permissions (see below).  

---

## 🔐 Required GCP Permissions

- **Billing data**: `roles/billing.viewer`  
- **BigQuery dataset (Billing Export)**: `roles/bigquery.dataViewer`  

---

## 🛠 Example Config (`mcp.json`)

```json
{
  "mcpServers": {
    "gcp-mcp": {
      "command": "npx",
      "args": ["-y", "google-cloud-mcp@latest"],
      "env": {
        "GOOGLE_PROJECT_ID": "<your-project-id>",
        "GOOGLE_APPLICATION_CREDENTIALS": "<path-to-service-account.json>"
      }
    }
  }
}
