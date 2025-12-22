# AI for FinOps - Cloud FinOps MCP Resources

> Practical **Model Context Protocol (MCP)** resources for **Cloud FinOps**: pricing, budgets, anomaly checks, automation — with security guardrails.

[![Links](https://img.shields.io/badge/links-checked-brightgreen)]()
[![License](https://img.shields.io/badge/license-Apache--2.0-blue)]()
[![Good First Issues](https://img.shields.io/github/issues/OptimNow/finops-mcp-resources/good%20first%20issue)]()

---

## 🚀 Start Here
- ✅ [Download a client](clients/Comparison.md) 
- ✅ [Run your first MCP](tutorials/01-aws-pricing-mcp-quickstart.md)  
- ✅ [Explore the FinOps MCPs and pick the one that best fits your use case](servers/)  


---

## 📂 Repository Structure

- [/foundations](./foundations) → background notes, whitepapers, blog summaries  
- [/servers](./servers) → registry of available MCP servers (pricing, tagging, governance)  
- [/clients](./clients) → tested MCP clients (Claude, Cursor, VS Code, Q, etc.)  
- [/tutorials](./tutorials) → runnable guides (step by step)  
- [/finops-use-cases](./finops-use-cases) → applied scenarios (budgeting, anomaly detection, tagging compliance)  
- [/tooling-governance](./tooling-governance) → security checklists, threat models, deployment guidance  
- [/presentations](./presentations) → slides, abstracts, LinkedIn drafts  
- [/resources](./resources) → external links (FinOps WG docs, repos, talks, videos)  


---

## 🧩 What is MCP?
MCP is an **open standard protocol** that lets **LLMs act as agents** by safely connecting to external tools (servers) like AWS Cost Explorer, a GCP BigQuery dataset with billing exports, an Azure storage account holding cost data, or 3rd-party cloud finops solutions like Vantage.

**Industry Adoption (2025)**:
In December 2025, Anthropic donated MCP to the **Agentic AI Foundation** (Linux Foundation), with founding support from Anthropic, Block, and OpenAI, plus backing from Google, Microsoft, AWS, Cloudflare, and Bloomberg. This ensures MCP remains open, neutral, and community-driven as critical AI infrastructure.

The ecosystem has grown to **10,000+ active MCP servers** and is now supported by major AI platforms including ChatGPT, Claude, Gemini, Microsoft Copilot, VS Code, Cursor, and Amazon Q.

In FinOps, MCP unlocks:
- Faster cost simulations
- Real-time tagging compliance
- Forecasting and Cost Simulations
- Cost Optimization recommendations

But also raises **governance and security** challenges — this repo addresses both sides.

---

## 🛠️ Contributing
We welcome contributions:
1. Fork the repo and create a branch
2. Add your MCP server, tutorial, or use case
3. Open a PR with a clear description  

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.  
New to MCP or FinOps? Start with issues labeled **good first issue**.

---

## 🔐 Governance
- [Code of Conduct](CODE_OF_CONDUCT.md)  
- [License](LICENSE)  

---

## 🌍 Community
- Join discussions in the [FinOps Foundation Slack](https://www.finops.org/slack/)  
- Follow updates on [LinkedIn](https://linkedin.com/in/jeanlatiere)  
- Share your use cases, raise issues, propose servers  

---

## 📜 License
This project is licensed under the [Apache 2.0 License](LICENSE).
