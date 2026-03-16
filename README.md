# AI for FinOps - Cloud FinOps MCP Resources

> Practical **Model Context Protocol (MCP)** resources for **Cloud FinOps**: pricing, budgets, anomaly checks, automation — with security guardrails.

[![Links](https://img.shields.io/badge/links-checked-brightgreen)]()
[![License](https://img.shields.io/badge/license-Apache--2.0-blue)]()
[![Good First Issues](https://img.shields.io/github/issues/OptimNow/finops-mcp-resources/good%20first%20issue)]()

---

## Recent Updates (March 2026)

- **New: Workflow & Collaboration Servers** — Added documentation for [FinOps Tagging Compliance](./servers/tagging.md), [JIRA](./servers/jira.md), and [Slack](./servers/slack.md) MCP servers
- **Registry Backfill** — [`registry.yaml`](./servers/configs/registry.yaml) now contains all 18 documented MCP servers as the single source of truth
- **Repository Streamlined** — Consolidated `use-cases/` into tutorials, trimmed INDEX files, fixed broken links, removed unused directory references
- **MCP Authentication Vulnerabilities Guide** (January 2026) — Critical security risks and remediation for MCP deployments ([read more](./governance/mcp-authentication-vulnerabilities-2026.md))
- **AWS MCP Remote Server** — GA with 15,000+ AWS APIs and Agent SOPs ([tutorial](./tutorials/07-aws-mcp-remote-server.md))

---

## Start Here
- [Download a client](clients/comparison.md)
- [Run your first MCP](tutorials/01-aws-pricing-quickstart.md)
- [Explore the FinOps MCPs](servers/)

---

## Repository Structure

- [/foundations](./foundations) — background notes, architecture, getting started
- [/servers](./servers) — registry of MCP servers (AWS, Azure, GCP, Tagging, JIRA, Slack)
- [/clients](./clients) — tested MCP clients (Claude, ChatGPT, Gemini, Copilot, Cursor, Kiro, etc.)
- [/tutorials](./tutorials) — runnable step-by-step guides
- [/governance](./governance) — security checklists, threat models, deployment guidance

---

## What is MCP?

<img src="./images/MCP_USB.jpeg" alt="MCP Architecture - Hub & Spoke Model" width="50%">

*MCP connects AI clients to multiple data sources and services through a standardized protocol*

MCP is an **open standard protocol** that lets **LLMs act as agents** by safely connecting to external tools (servers) like AWS Cost Explorer, a GCP BigQuery dataset with billing exports, an Azure storage account holding cost data, or 3rd-party cloud finops solutions like Vantage.

**Industry Adoption (March 2026)**:
In December 2025, Anthropic donated MCP to the **Agentic AI Foundation** (Linux Foundation), with founding support from Anthropic, Block, and OpenAI, plus backing from Google, Microsoft, AWS, Cloudflare, and Bloomberg. As of March 2026, the ecosystem has grown to **10,000+ active MCP servers** and is supported by all major AI platforms.

In FinOps, MCP unlocks:
- Faster cost simulations
- Real-time tagging compliance
- Forecasting and Cost Simulations
- Cost Optimization recommendations

But also raises **governance and security** challenges — this repo addresses both sides.

---

## Contributing
We welcome contributions:
1. Fork the repo and create a branch
2. Add your MCP server, tutorial, or use case
3. Open a PR with a clear description

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.
New to MCP or FinOps? Start with issues labeled **good first issue**.

---

## Governance
- [Code of Conduct](CODE_OF_CONDUCT.md)
- [License](LICENSE)

---

## Community
- Join discussions in the [FinOps Foundation Slack](https://www.finops.org/slack/)
- Follow updates on [LinkedIn](https://linkedin.com/in/jeanlatiere)
- Share your use cases, raise issues, propose servers

---

## License
This project is licensed under the [Apache 2.0 License](LICENSE).
