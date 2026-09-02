# MCP Servers for Cloud FinOps and Cost Optimization

**Last Updated**: September 2026

A curated registry of 23 MCP servers for cloud cost optimization, FinOps automation, and AI-powered cost management across AWS, Azure, and GCP. Includes cloud provider billing servers, tagging governance tools, and workflow integrations (JIRA, Slack).

See [`configs/registry.yaml`](./configs/registry.yaml) for the machine-readable registry.

---

## Which MCP servers are available for FinOps?

### AWS Cloud Cost Servers
- **[AWS MCP Servers](./aws.md)** — Remote server (15,000+ APIs), Pricing API, Cost Explorer, CloudWatch, Billing & Cost Management, CFM Tips

### Azure Cloud Cost Servers
- **[Azure MCP Servers](./azure.md)** — 🆕 Official ARM / Azure FinOps MCP Server (public preview), @azure/mcp with retail pricing, community FinOps server for billing data

### GCP Cloud Cost Servers
- **[GCP MCP Servers](./gcp.md)** — BigQuery billing exports, Compute Engine, GKE, community servers

### Multi-cloud Cost Servers
- **[Costory FinOps MCP](https://docs.costory.io/features/mcp)** — Hosted server over an allocation and correlation layer for AWS, GCP, Azure plus SaaS and LLM spend: allocated cost queries, cost-change diffs against deploy events, unit economics, alerts and reports from chat
- **[nable (finops-mcp)](https://github.com/chaandannn/finopsmcp)** — Local-first server for AWS, Azure and GCP plus AI and SaaS spend: cost queries, anomaly detection, rightsizing, LLM token tracking

### AI Cost & FinOps Knowledge Servers
Built by OptimNow, the maintainers of this repository. All three are hosted, read-only and take no credentials.
- **[Cloud FinOps MCP](https://github.com/OptimNow/cloud-finops-skills)** — FinOps knowledge for AI agents: curated references on AWS, Azure, GCP and OCI billing mechanics, commitments, allocation, AI inference economics and data platforms, plus named-pattern waste-detection playbooks with tested queries. Hosted endpoint or `pip install cloud-finops-mcp`
- **[OptimToken MCP (AI Pricing Hub)](https://github.com/OptimNow/ai-pricing-hub-mcp)** — Live, dated LLM prices for 250+ models and compute instance rates across seven clouds: model comparison and recommendation, cost per request and per month from your token shape, cache hit rate and batch eligibility
- **[AI ROI Calculator MCP](https://github.com/OptimNow/ai-roi-calculator-mcp)** — Business case for an LLM use case: ROI, payback, break-even volume and sensitivity analysis from live prices and a 3-layer cost model (inference, harness, business value)

### Workflow & Collaboration
- **[FinOps Tagging Compliance](./tagging.md)** — Tag compliance checking, drift detection, cost attribution gap analysis
- **[JIRA for FinOps](./jira.md)** — Cost anomaly ticketing, optimization tracking, remediation workflows
- **[Slack for FinOps](./slack.md)** — Cost alerts, budget notifications, cross-team collaboration

---

## Where can I find more MCP servers?

- **Official MCP Registry**: [modelcontextprotocol.io/registry](https://modelcontextprotocol.io/registry)
- **MCPdb**: [mcpdb.org](https://mcpdb.org) — 10,000+ servers
- **Awesome MCP Servers**: [github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers)

---

← [Back to Home](../README.md)
