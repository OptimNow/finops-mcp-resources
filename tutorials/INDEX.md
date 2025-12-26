# FinOps MCP Tutorials

**Last Updated**: January 2026

Step-by-step guides for getting started with Model Context Protocol (MCP) for Cloud FinOps.

---

## 🎯 Recommended Learning Path

Follow these tutorials in order for the best learning experience:

### 1️⃣ **Start Here** → [AWS Pricing MCP Quickstart](./01-aws-pricing-quickstart.md)
- **Time**: 15-20 minutes
- **Prerequisites**: None (beginner-friendly)
- **What you'll learn**:
  - Install your first MCP server
  - Connect to a client (Claude Desktop)
  - Query real-time AWS pricing data
- **Best for**: Complete beginners to MCP

### 2️⃣ **Next** → [Cost Analysis with Amazon Q](./02-amazon-q-cost-analysis.md)
- **Time**: 20-30 minutes
- **Prerequisites**: AWS account, Tutorial 1 completed
- **What you'll learn**:
  - Use Amazon Q for FinOps workflows
  - Analyze AWS cost data with natural language
  - AWS-native MCP integration
- **Best for**: AWS-focused teams

### 3️⃣ **Advanced** → [FinOps Multi-Agent with Nova](./03-finops-multi-agent-nova.md)
- **Time**: 45-60 minutes
- **Prerequisites**: Tutorials 1 & 2 completed
- **What you'll learn**:
  - Multi-agent MCP workflows
  - Amazon Nova integration
  - Complex FinOps orchestration
- **Best for**: Advanced users, DevOps teams

### 4️⃣ **Multi-Cloud** → [Azure MCP Quick Start](./04-azure-mcp-quickstart.md)
- **Time**: 20-30 minutes
- **Prerequisites**: Azure account, Tutorial 1 completed
- **What you'll learn**:
  - Set up MCP for Azure Cost Management
  - Query Azure billing data
  - Multi-cloud MCP setup
- **Best for**: Azure users, multi-cloud teams

### 5️⃣ **Multi-Cloud** → [GCP BigQuery MCP Quick Start](./05-gcp-bigquery-quickstart.md)
- **Time**: 30-40 minutes
- **Prerequisites**: GCP account, Tutorial 1 completed
- **What you'll learn**:
  - Connect MCP to BigQuery billing exports
  - Query GCP cost data with SQL
  - Analyze GCP billing at scale
- **Best for**: GCP users, data-heavy FinOps workflows

### 6️⃣ **Multi-Cloud** → [GCP Billing Export MCP Setup](./Tutorial-GCP-Billing-MCP.md)
- **Time**: 35-45 minutes
- **Prerequisites**: GCP account with Billing Export enabled
- **What you'll learn**:
  - Install and build community GCP Billing MCP server
  - Configure with VS Code (Gemini Code Assist)
  - Configure with Google Gemini CLI
  - Query billing data from BigQuery using AI
- **Best for**: GCP FinOps teams, VS Code users, hands-on learners

---

## 📚 Tutorials by Cloud Provider

### AWS
1. [AWS Pricing MCP Quickstart](./01-aws-pricing-quickstart.md) - Real-time pricing queries
2. [Cost Analysis with Amazon Q](./02-amazon-q-cost-analysis.md) - AWS-native assistant
3. [FinOps Multi-Agent with Nova](./03-finops-multi-agent-nova.md) - Advanced workflows

### Azure
4. [Azure MCP Quick Start](./04-azure-mcp-quickstart.md) - Azure Cost Management

### GCP
5. [GCP BigQuery MCP Quick Start](./05-gcp-bigquery-quickstart.md) - BigQuery billing exports
6. [GCP Billing Export MCP Setup](./Tutorial-GCP-Billing-MCP.md) - Community server with VS Code & Gemini

---

## 🎓 Tutorials by Skill Level

### Beginner
- [AWS Pricing MCP Quickstart](./01-aws-pricing-quickstart.md) ⭐ Start here
- [Cost Analysis with Amazon Q](./02-amazon-q-cost-analysis.md)
- [Azure MCP Quick Start](./04-azure-mcp-quickstart.md)

### Intermediate
- [GCP BigQuery MCP Quick Start](./05-gcp-bigquery-quickstart.md)
- [GCP Billing Export MCP Setup](./Tutorial-GCP-Billing-MCP.md)

### Advanced
- [FinOps Multi-Agent with Nova](./03-finops-multi-agent-nova.md)

---

## 🛠️ What You'll Need

### General Prerequisites
- **MCP Client**: Choose from [9 available clients](../clients/INDEX.md)
  - Recommended for beginners: [Claude Desktop](../clients/claude-desktop.md) or [ChatGPT](../clients/chatgpt.md)
  - Recommended for developers: [Claude Code](../clients/claude-code.md) or [VS Code](../clients/vscode.md)

### Cloud Provider Prerequisites
- **AWS Tutorials**: AWS account with IAM permissions ([see IAM guide](../governance/security-aws-iam-policies.md))
- **Azure Tutorials**: Azure account with Cost Management access
- **GCP Tutorials**: GCP account with BigQuery and billing export setup

### Tools
- Terminal/Command Line (for most tutorials)
- Node.js/npm (for some MCP servers)
- Python (optional, for some advanced tutorials)

---

## 💡 After Completing Tutorials

Once you've completed these tutorials, explore:

### Next Steps
1. **Choose your production client**: Review [Client Comparison](../clients/comparison.md)
2. **Set up security**: Read [MCP Security Best Practices](../governance/security-best-practices-2025.md)
3. **Deploy remote servers**: See [Remote MCP Servers Guide](../governance/remote-mcp-servers.md)
4. **Explore use cases**: Check [FinOps Use Cases](../use-cases/)
5. **Discover more servers**: Browse the [MCP Registry](https://modelcontextprotocol.io/registry)

### Additional Learning Resources
- [MCP Architecture](../foundations/mcp-architecture.md) - Deep dive into how MCP works
- [MCP Specification 2025-11-25](../foundations/mcp-architecture.md#-mcp-specification-2025-11-25-updates) - Latest protocol features
- [All MCP Servers](../servers/) - AWS, Azure, GCP server documentation

---

## 🆘 Getting Help

**Stuck on a tutorial?**
- Check the [Getting Started Guide](../foundations/getting-started.md) for basics
- Review the [Client Documentation](../clients/) for your specific client
- Open an issue on [GitHub](https://github.com/OptimNow/finops-mcp-resources/issues)
- Join the [FinOps Foundation Slack](https://www.finops.org/slack/)

**Found an error?**
- Please open a pull request with corrections
- See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines

---

## 🔜 Upcoming Tutorials

We're working on additional tutorials for:
- **Kiro**: Agentic IDE with spec-driven development for FinOps
- **Microsoft Copilot**: MCP integration with Copilot Studio
- **Google Gemini**: Vertex AI integration for cost analysis
- **Remote MCP deployment**: Cloud-hosted MCP servers on AWS/Azure/GCP
- **Task-based workflows**: Using MCP 2025-11-25 async tasks for long-running cost analyses

Want to contribute a tutorial? See [CONTRIBUTING.md](../CONTRIBUTING.md)!

---

## 📊 Tutorial Statistics

**Total Tutorials**: 6
**Cloud Providers Covered**: AWS (3), Azure (1), GCP (2)
**Difficulty Levels**: Beginner (3), Intermediate (2), Advanced (1)
**Average Completion Time**: 25-40 minutes per tutorial

---

← [Back to Home](../README.md) | [View All Clients](../clients/INDEX.md)
