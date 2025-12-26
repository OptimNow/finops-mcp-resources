# Google Gemini

**Access Gemini**: https://gemini.google.com

Google Gemini is Google's conversational AI platform that added native MCP support in April 2025, with integration across Gemini 2.5 Pro API, SDK, and related Google Cloud infrastructure.

## MCP Support Timeline
- **April 2025**: Native MCP support announced for Gemini 2.5 Pro API and SDK
- **December 2025**: Google joined as platinum member of the Agentic AI Foundation (AAIF)

---

✅ **Pros for a Cloud FinOps professional**
- **GCP-native integration**: Seamless connection to BigQuery billing exports, Cloud Asset Inventory, and GCP Cost Management APIs.
- **Enterprise-ready**: Built on Google Cloud infrastructure with strong security, compliance, and data residency controls.
- **Multi-modal capabilities**: Process billing CSVs, cost dashboards, architecture diagrams, and generate cost visualizations.
- **API and SDK access**: Programmatic MCP integration for automation and custom FinOps workflows.
- **No data egress**: For GCP-focused teams, data stays within Google's ecosystem.
- **Vertex AI integration**: Leverage Gemini in Vertex AI for production-grade FinOps agents.

⚠️ **Cons for a Cloud FinOps professional**
- **GCP bias**: While multi-cloud is supported, Gemini naturally excels with Google Cloud data sources.
- **Pricing complexity**: Gemini API pricing varies by model tier, context window, and usage patterns.
- **Maturity**: MCP support is newer than Claude/ChatGPT; some features may be less polished.
- **Limited consumer access**: Full MCP capabilities primarily available through Google Cloud APIs, not consumer Gemini interface.
- **Vendor lock-in**: Deep integration with GCP may make multi-cloud FinOps workflows harder.
- **Hallucination risk**: Like all LLMs, requires validation of cost calculations and recommendations.

---

## Official Google MCP Servers

**Repository**: [Google/mcp](https://github.com/Google/mcp)
**Status**: Officially maintained by Google

Google provides **4 official remote MCP servers** that work seamlessly with Gemini:

| **MCP Server** | **Description** | **Use Cases** |
|:---------------|:----------------|:--------------|
| **[Google Maps (Grounding Lite)](https://github.com/Google/mcp)** | Location-based queries and geocoding | Regional cost analysis, data center location optimization |
| **[BigQuery](https://github.com/Google/mcp)** | Query BigQuery datasets including billing exports | Cost analysis, spend trends, FinOps reporting |
| **[Google Kubernetes Engine (GKE)](https://github.com/Google/mcp)** | GKE cluster management and monitoring | Container cost optimization, cluster rightsizing |
| **[Google Compute Engine (GCE)](https://github.com/Google/mcp)** | Compute instance management and cost data | VM rightsizing, instance cost optimization |

All official Google MCP servers are **remote-first** (SSE/HTTP transports), eliminating the need for local installation.

---

## Community MCP Servers for GCP

In addition to Google's official servers, the community has developed MCP servers for GCP services:

### GCP Compute MCP
**Documentation**: [GCP Compute MCP Reference](https://docs.cloud.google.com/compute/docs/reference/mcp)

Community-built MCP server for Google Compute Engine with extended capabilities beyond the official GCE server.

**For a complete list of community GCP MCP servers**, see the [MCP Registry](https://registry.mcp.run/) or [servers/gcp.md](../servers/gcp.md).

---

## MCP Configuration

Gemini supports MCP through multiple access points:

### 1. Gemini API (Programmatic)
Configure MCP servers via the Gemini 2.5 Pro API for automation workflows.

### 2. Vertex AI (Enterprise)
Use MCP-enabled Gemini models in Vertex AI for production-grade FinOps agents with enterprise controls.

### 3. Google Cloud SDK Integration
Integrate MCP servers into Google Cloud workflows, Cloud Functions, and Cloud Run.

### Configuration Example

```json
{
  "mcpServers": {
    "bigquery-billing": {
      "url": "https://mcp.googleapis.com/bigquery",
      "transport": "sse",
      "env": {
        "GOOGLE_CLOUD_PROJECT": "your-project-id",
        "GOOGLE_APPLICATION_CREDENTIALS": "/path/to/credentials.json"
      }
    },
    "gce-cost-optimization": {
      "url": "https://mcp.googleapis.com/compute",
      "transport": "sse",
      "env": {
        "GOOGLE_CLOUD_PROJECT": "your-project-id"
      }
    }
  }
}
```

Refer to [Google Cloud's Gemini MCP documentation](https://cloud.google.com/vertex-ai/docs/generative-ai/gemini) for detailed setup instructions.

---

## FinOps Use Cases

**Recommended for:**
- GCP-centric FinOps teams leveraging BigQuery billing exports
- Enterprise deployments requiring data residency in Google Cloud
- Multi-modal cost reporting (combining charts, spreadsheets, architecture diagrams)
- FinOps automation integrated with Google Cloud workflows and Cloud Functions

**Not recommended for:**
- AWS or Azure-first organizations (Claude/ChatGPT may be better fits)
- Teams without Google Cloud infrastructure or expertise
- Consumer/SMB use cases needing simple chat interfaces (consumer Gemini has limited MCP)

---

👉 **In short**: Gemini + MCP is ideal for GCP-native FinOps teams who want to keep cost data within Google's ecosystem, with strong enterprise controls and Vertex AI integration, but may require more technical setup than ChatGPT or Claude.
