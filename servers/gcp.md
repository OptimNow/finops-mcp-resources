# ![GCP Logo](https://www.vectorlogo.zone/logos/google_cloud/google_cloud-icon.svg) GCP MCP Server

The **community GCP MCP server** lets you query **Google Cloud Billing Export** data for FinOps use cases such as cost analysis, anomaly detection, and optimization.  

---

## 🔧 GCP MCP Server

| **Server Name** | **Description** | **Install** |
|:----------------|:----------------|:-------------|
| [GCP MCP Server](https://github.com/krzko/google-cloud-mcp) | Query cost and usage data from **GCP Billing Export in BigQuery**. | Manual install (see below) |

---

## ⚙️ Prerequisites

Before installing, ensure:  
- **Billing Export to BigQuery** is enabled in your GCP project ([guide](https://cloud.google.com/billing/docs/how-to/export-data-bigquery)).  
- **gcloud CLI** installed and authenticated (`gcloud auth application-default login`).  
- **Node.js 18+** and [pnpm](https://pnpm.io/installation) installed.  
- A **Service Account** with minimal permissions (see below).  

---

## 🔐 Required GCP Permissions

- **Billing data**: `roles/billing.viewer`  
- **BigQuery dataset (Billing Export)**: `roles/bigquery.dataViewer`  

👉 Full list of permissions is documented here:  
[**GCP MCP Required IAM Permissions**](../tooling-governance/security-privileges-gcp.md)  

---

## 🛠 Installation

```bash
# Clone the repository
git clone https://github.com/krzko/google-cloud-mcp.git
cd google-cloud-mcp

# Install dependencies
pnpm install

# Build
pnpm build

# Authenticate to Google Cloud
gcloud auth application-default login
```

⚙️ **Client Configuration** (mcp.json)

Update your MCP client config (e.g., in ~/.mcp/servers.json):
```json
{
  "mcpServers": {
    "google-cloud-mcp": {
      "command": "node",
      "args": [
        "/Users/<your-user>/code/google-cloud-mcp/dist/index.js"
      ],
      "env": {
        "GOOGLE_APPLICATION_CREDENTIALS": "/Users/<your-user>/.config/gcloud/application_default_credentials.json"
      }
    }
  }
}
```
📝 **Notes**

This is a **community MCP** (not yet officially supported by Google).

Installation is **manual** — no one-click install button in VS Code or Cursor.

Make sure your **Billing Export dataset** is active before running queries.
