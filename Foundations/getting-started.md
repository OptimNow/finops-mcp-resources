# Getting Started with FinOps MCP Resources

Welcome! This repository is designed to help FinOps practitioners experiment with **Model Context Protocol (MCP) servers** and turn LLMs into practical FinOps copilots.

If you’re new here, here’s the best path to get started:

---

## 1. Start with the AWS Pricing MCP Tutorial
Your first stop should be the [AWS Pricing MCP Quickstart](/tutorials/01-aws-pricing-mcp-quickstart.md). It’s a step-by-step guide that walks you through:
- Installing the AWS Pricing MCP server  
- Connecting it to a client  
- Running your first real-time cloud pricing queries  

This is the easiest way to see MCP in action for FinOps.

---

## 2. Set Up an MCP Client
Once you've tried AWS Pricing, explore how to use different MCP **clients**.

As of January 2026, we provide comprehensive guides for **9 major MCP clients**:

### **For Developers & Technical Teams**
- [Claude Code](/clients/8.%20claude-code.md) - Remote MCP, task workflows, enterprise controls
- [Kiro](/clients/9.%20kiro.md) - Agentic IDE with spec-driven development (AWS-focused, preview)
- [VS Code](/clients/1.%20vscode.md) - Flexible, extensible, familiar to engineers
- [Cursor](/clients/4.%20cursor.md) - Modern IDE with MCP support

### **For Business & Finance Stakeholders**
- [ChatGPT](/clients/5.%20chatgpt.md) - Most accessible, huge user base (added March 2025)
- [Claude Desktop](/clients/2.%20claude.md) - Fast prototyping, conversational
- [Microsoft Copilot](/clients/7.%20copilot.md) - Microsoft 365 integration
- [Google Gemini](/clients/6.%20gemini.md) - GCP integration, Vertex AI (added April 2025)

### **For Cloud-Specific Use Cases**
- [Amazon Q](/clients/3.%20amazonQ.md) - AWS-native assistant

**Not sure which to choose?** Check our [Client Comparison Guide](/clients/Comparison.md) with detailed pros/cons for FinOps professionals.

---

## 3. Review Governance & Security
Before scaling further, check out the **security and permissions** guidance:  
- [AWS privileges for pricing](/tooling-governance/security-privileges-aws.md)  
- [General governance guidelines](/tooling-governance/)  

This will help you set up safe, scoped credentials and avoid giving MCP servers unnecessary permissions.

---

## 4. Explore FinOps Use Cases
Once your setup is running, try applying MCP to real FinOps challenges:
- Compare EC2 and VM costs across regions  
- Generate automated monthly budget variance reports  
- Run “what-if” simulations for commitment coverage  
- Spot anomalies in billing data  

👉 And most importantly: **come up with your own use cases!**  
This repo is a sandbox — test, prototype, and see how MCP can accelerate your FinOps workflows.

---

## 5. Discover More MCP Servers

The MCP ecosystem has grown to **10,000+ active servers** as of January 2026. Here's how to discover FinOps-relevant servers:

### Official MCP Registry
- **URL**: [MCP Registry](https://modelcontextprotocol.io/registry) (check modelcontextprotocol.io)
- **What it is**: Official server directory launched by the MCP community
- **Growth**: 407% increase from initial batch, with servers for AWS, Azure, GCP, and third-party FinOps tools

### Community Directories
- **MCPdb**: Claims 10,000+ servers and clients - [mcpdb.org](https://mcpdb.org)
- **Awesome MCP Servers**: Curated GitHub list - [github.com/punkpeye/awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers)
- **Official MCP Servers**: Anthropic's reference implementations - [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)

### What to Look For
When discovering MCP servers for FinOps:
- **Cloud provider integrations**: AWS Cost Explorer, Azure Cost Management, GCP BigQuery billing
- **Third-party platforms**: Vantage, CloudHealth, Cloudability, Apptio
- **Governance tools**: Tagging compliance, policy enforcement, security scanning
- **Data sources**: Billing exports, usage metrics, reservation/savings plan data

---

## Next Steps
- Try additional MCP servers beyond AWS Pricing (Azure, GCP, Vantage, etc.)
- Browse the MCP Registry to discover new FinOps-relevant servers
- Share feedback or new use cases in Issues/PRs
- Join the wider conversation at [modelcontextprotocol.io](https://modelcontextprotocol.io/)

---

💡 **Tip:** Start small, validate results against raw billing data, and always apply governance guardrails before scaling MCP in enterprise environments.

