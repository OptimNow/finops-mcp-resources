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
Once you’ve tried AWS Pricing, explore how to use different MCP **clients**.  
We provide guides for:
- [Cursor](/clients/4.%20cursor.md)
- [Claude](/clients/2.%20claude.md)
- [VS Code](/clients/1.%20vscode.md)  
- [Amazon Q](/clients/3.%20amazon-q.md)  

Each client has strengths and limitations. Pick the one that best fits your workflow.

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

## Next Steps
- Try additional MCP servers beyond AWS Pricing (Azure, GCP, Vantage, etc.)  
- Share feedback or new use cases in Issues/PRs  
- Join the wider conversation at [modelcontextprotocol.io](https://modelcontextprotocol.io/)  

---

💡 **Tip:** Start small, validate results against raw billing data, and always apply governance guardrails before scaling MCP in enterprise environments.

