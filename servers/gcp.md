# ![GCP Logo](https://www.vectorlogo.zone/logos/google_cloud/google_cloud-icon.svg) GCP MCP Server

The **community GCP MCP server** lets you query **Google Cloud Billing Export** data for FinOps use cases such as cost analysis, anomaly detection, and optimization.  

| **Server Name** | **Description** | **Install** |
|:----------------|:----------------|:-------------|
| [GCP MCP Server](https://github.com/krzko/google-cloud-mcp) | Query cost and usage data from **GCP Billing Export in BigQuery**. | Manual install (see below) |

---



## ⚙️ Prerequisites

Before installing, ensure:  
- **Billing Export to BigQuery** is enabled in your GCP project ([guide](https://cloud.google.com/billing/docs/how-to/export-data-bigquery)).  

- **gcloud CLI** installed and authenticated (`gcloud auth application-default login`).  ([guide](https://cloud.google.com/sdk/docs/install) to install gcloud CLI)

- Install **Node.js** on Windows using PowerShell:

  1. Download and install Node.js (LTS) via Chocolatey (requires [Chocolatey](https://chocolatey.org/install) installed):

  ```powershell
  choco install -y nodejs-lts
  ```

  2. Verify the installation:

  ````powershell 
  node -v
  npm -v
  ````

  

- Install **[pnpm](https://pnpm.io/installation)** by running the following command in your terminal:

  ```bash
  npm install -g pnpm
  ```

- A **Service Account** with minimal permissions (see below).  

---



## 🔐 Required GCP Permissions

- **Billing data**: `roles/billing.viewer`  
- **BigQuery dataset (Billing Export)**: `roles/bigquery.dataViewer`  

 

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

---
📝 **Notes**

- This is a **community MCP** (not yet officially supported by Google).

- Installation is **manual** — no one-click install button in VS Code or Cursor.

- Make sure your **Billing Export dataset** is active before running queries.
