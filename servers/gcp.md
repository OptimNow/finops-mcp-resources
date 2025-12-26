# ![GCP Logo](https://www.vectorlogo.zone/logos/google_cloud/google_cloud-icon.svg) GCP MCP Servers

**Last Updated**: January 2026

A comprehensive guide to Google Cloud Platform (GCP) MCP servers for FinOps and cloud management.

---

## 🚀 Official Google MCP Servers

**Repository**: [Google/mcp](https://github.com/Google/mcp)
**Status**: Officially maintained by Google
**Announcement**: 2025

Google provides **4 official remote MCP servers** that work seamlessly with Gemini and other MCP clients:

| **MCP Server** | **Description** | **FinOps Use Cases** | **Transport** |
|:---------------|:----------------|:---------------------|:--------------|
| **[Google Maps (Grounding Lite)](https://github.com/Google/mcp)** | Location-based queries and geocoding | Regional cost analysis, data center location optimization | Remote (SSE/HTTP) |
| **[BigQuery](https://github.com/Google/mcp)** | Query BigQuery datasets including billing exports | Cost analysis, spend trends, FinOps reporting | Remote (SSE/HTTP) |
| **[Google Kubernetes Engine (GKE)](https://github.com/Google/mcp)** | GKE cluster management and monitoring | Container cost optimization, cluster rightsizing | Remote (SSE/HTTP) |
| **[Google Compute Engine (GCE)](https://github.com/Google/mcp)** | Compute instance management and cost data | VM rightsizing, instance cost optimization | Remote (SSE/HTTP) |

**Key Benefits:**
- ✅ **Remote-first architecture** - No local installation required
- ✅ **Official Google support** - Maintained by Google Cloud team
- ✅ **Native Gemini integration** - Optimized for Google's AI platform
- ✅ **Enterprise-ready** - Built on Google Cloud infrastructure with security controls

---

## 💰 Community GCP MCP Servers

In addition to Google's official servers, the community has developed MCP servers for GCP FinOps:

| **Server Name** | **Description** | **Install** |
|:----------------|:----------------|:-------------|
| [GCP MCP Server](https://github.com/krzko/google-cloud-mcp) | Query cost and usage data from **GCP Billing Export in BigQuery** | Manual install (see below) |
| [GCP Compute MCP](https://docs.cloud.google.com/compute/docs/reference/mcp) | Extended Compute Engine capabilities beyond official GCE server | See Google Cloud documentation |

---

## 🛠️ Community Server Installation: GCP Billing Export MCP

The following installation guide is for the **community GCP MCP server** by krzko, which provides access to GCP Billing Export data in BigQuery.

### ⚙️ Prerequisites

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
