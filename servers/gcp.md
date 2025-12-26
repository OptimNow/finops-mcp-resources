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
| [GCP Compute MCP](https://docs.cloud.google.com/compute/docs/reference/mcp) | Google Compute Engine management via MCP | See installation guide below |

---

## 🔧 Google Compute Engine MCP Installation

**Documentation**: [GCP Compute MCP Reference](https://docs.cloud.google.com/compute/docs/reference/mcp)
**Package**: `@google/mcp-server-compute`
**Status**: ⚠️ **Not Yet Available** - Package not found in npm registry as of January 2026

The Google Compute Engine MCP is referenced in Google Cloud documentation but the npm package `@google/mcp-server-compute` **does not currently exist** in the public registry.

### Current Status

As of January 2026:
- ✅ **Official Google MCP Servers** (BigQuery, GKE, GCE, Google Maps) are available at [github.com/Google/mcp](https://github.com/Google/mcp)
- ❌ **Standalone `@google/mcp-server-compute` package** is not yet published to npm
- 🔄 **Google Compute Engine MCP** may be part of the official Google MCP servers repository instead

### Alternative: Use Official Google MCP Servers

Instead of the standalone package, try using the **official Google Compute Engine (GCE) server** from Google's MCP repository:

**Repository**: [https://github.com/Google/mcp](https://github.com/Google/mcp)

Check the repository for installation instructions for the GCE MCP server.

---

### Installation Guide (When Available)

The following instructions are for **when the package becomes available**:

The Google Compute Engine MCP would allow Gemini (and other MCP clients) to manage GCE resources, query instance pricing, and optimize VM costs.

### Prerequisites

Before configuring the connection, ensure your environment is ready:

1. **Google Cloud Project**: Have a project ID ready where you want to manage Compute Engine resources
2. **Enable Required APIs**: In Google Cloud Console, enable:
   - Compute Engine API
   - Gemini for Google Cloud API
3. **Permissions**: Your user account (or service account) must have:
   - `Compute Admin` role (or similar) to perform resource management actions
   - Billing permissions for cost analysis
4. **Authentication**: Run `gcloud auth application-default login` to authenticate

### Installation Options

You can use the GCE MCP in two main ways:

#### Option A: Gemini CLI (Terminal)

The Gemini CLI is an open-source agent that supports MCP natively.

**1. Install the Gemini CLI:**

```bash
npm install -g @google/gemini-cli
```

**2. Authenticate:**

```bash
gcloud auth application-default login
```

**3. Configure the MCP Server:**

Create or edit `~/.gemini/settings.json` and add:

```json
{
  "mcpServers": {
    "google-compute": {
      "url": "https://compute.googleapis.com/mcp",
      "transport": "http",
      "env": {
        "GOOGLE_CLOUD_PROJECT": "YOUR_PROJECT_ID"
      }
    }
  }
}
```

**Note**: The exact URL might vary based on region or preview version. Check the [official reference](https://docs.cloud.google.com/compute/docs/reference/mcp) for the latest endpoint.

**4. Run Gemini:**

```bash
gemini
```

Verify the server is active by typing `/mcp list` in the CLI.

---

#### Option B: Gemini Code Assist (VS Code)

**1. Open VS Code Settings:**

Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac) and type "Open User Settings (JSON)"

**2. Add MCP Configuration:**

Add this to your `settings.json`:

```json
"geminicodeassist.mcpServers": {
  "gce": {
    "command": "npx",
    "args": ["-y", "@google/mcp-server-compute"],
    "env": {
      "GOOGLE_CLOUD_PROJECT": "YOUR_PROJECT_ID"
    }
  }
}
```

**3. Reload VS Code:**

Press `Ctrl+Shift+P` → "Reload Window" or restart VS Code.

### Troubleshooting

**First: Verify Package Exists**

```bash
npm view @google/mcp-server-compute
```

If you get `404 Not Found`, the package is not yet available. Check:
1. [Google/mcp GitHub repository](https://github.com/Google/mcp) for official servers
2. [Google Cloud MCP documentation](https://docs.cloud.google.com/mcp/overview) for updates
3. Google Cloud blog for announcements about new MCP servers

**If the package exists and the GCE MCP server doesn't connect:**

1. **Check API Enablement:**

   **Linux/Mac:**
   ```bash
   gcloud services list --enabled | grep compute
   gcloud services list --enabled | grep aiplatform
   ```

   **Windows PowerShell:**
   ```powershell
   gcloud services list --enabled | Select-String compute
   gcloud services list --enabled | Select-String aiplatform
   ```

2. **Verify Authentication:**
   ```bash
   gcloud auth application-default print-access-token
   ```
   Should return a valid access token.

3. **Check Project ID:**
   ```bash
   gcloud config get-value project
   ```
   Ensure it matches `YOUR_PROJECT_ID` in your configuration.

4. **View VS Code Extension Logs:**
   - Open Output panel (`Ctrl+Shift+U` or `Cmd+Shift+U`)
   - Select "Gemini Code Assist" from dropdown
   - Look for MCP connection errors

5. **Verify Package Installation:**
   ```bash
   npm list -g @google/mcp-server-compute
   ```

6. **Check Permissions:**
   ```bash
   gcloud projects get-iam-policy YOUR_PROJECT_ID \
     --flatten="bindings[].members" \
     --filter="bindings.members:user:YOUR_EMAIL"
   ```

### Common Issues

| **Issue** | **Solution** |
|:----------|:------------|
| `Error: compute.googleapis.com API not enabled` | Run `gcloud services enable compute.googleapis.com` |
| `Error: PERMISSION_DENIED` | Add `Compute Admin` role: `gcloud projects add-iam-policy-binding YOUR_PROJECT_ID --member="user:YOUR_EMAIL" --role="roles/compute.admin"` |
| `Error: Application Default Credentials not found` | Run `gcloud auth application-default login` |
| `npx: command not found` | Install Node.js and npm first |
| VS Code extension doesn't show MCP | Ensure Gemini Code Assist extension is installed and updated |

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
