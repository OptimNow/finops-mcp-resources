# FinOps MCP Tutorials

**Last Updated**: January 2026

Step-by-step guides for getting started with Model Context Protocol (MCP) for Cloud FinOps.

---

## 🎯 Recommended Learning Path

Follow these tutorials in order for the best learning experience:

### 1️⃣ **Start Here** → [AWS Pricing MCP Quickstart](./01-aws-pricing-mcp-quickstart.md)
- **Time**: 15-20 minutes
- **Prerequisites**: None (beginner-friendly)
- **What you'll learn**:
  - Install your first MCP server
  - Connect to a client (Claude Desktop)
  - Query real-time AWS pricing data
- **Best for**: Complete beginners to MCP

### 2️⃣ **Next** → [Cost Analysis with Amazon Q](./02.%20cost-analysis-with-AmazonQ.md)
- **Time**: 20-30 minutes
- **Prerequisites**: AWS account, Tutorial 1 completed
- **What you'll learn**:
  - Use Amazon Q for FinOps workflows
  - Analyze AWS cost data with natural language
  - AWS-native MCP integration
- **Best for**: AWS-focused teams

### 3️⃣ **Advanced** → [FinOps Multi-Agent with Nova](./03.finops_multi_agent_with_nova/guidelines.md)
- **Time**: 45-60 minutes
- **Prerequisites**: Tutorials 1 & 2 completed
- **What you'll learn**:
  - Multi-agent MCP workflows
  - Amazon Nova integration
  - Complex FinOps orchestration
- **Best for**: Advanced users, DevOps teams

### 4️⃣ **Multi-Cloud** → [Azure MCP Quick Start](./04.%20Azure%20MCP%20quick%20Start.md)
- **Time**: 20-30 minutes
- **Prerequisites**: Azure account, Tutorial 1 completed
- **What you'll learn**:
  - Set up MCP for Azure Cost Management
  - Query Azure billing data
  - Multi-cloud MCP setup
- **Best for**: Azure users, multi-cloud teams

### 5️⃣ **Multi-Cloud** → [GCP BigQuery MCP Quick Start](./05.%20GCP%20BigQuery%20MCP%20Quick%20Start.md)
- **Time**: 30-40 minutes
- **Prerequisites**: GCP account, Tutorial 1 completed
- **What you'll learn**:
  - Connect MCP to BigQuery billing exports
  - Query GCP cost data with SQL
  - Analyze GCP billing at scale
- **Best for**: GCP users, data-heavy FinOps workflows

---

## 📚 Tutorials by Cloud Provider

### AWS
1. [AWS Pricing MCP Quickstart](./01-aws-pricing-mcp-quickstart.md) - Real-time pricing queries
2. [Cost Analysis with Amazon Q](./02.%20cost-analysis-with-AmazonQ.md) - AWS-native assistant
3. [FinOps Multi-Agent with Nova](./03.finops_multi_agent_with_nova/guidelines.md) - Advanced workflows

### Azure
4. [Azure MCP Quick Start](./04.%20Azure%20MCP%20quick%20Start.md) - Azure Cost Management

### GCP
5. [GCP BigQuery MCP Quick Start](./05.%20GCP%20BigQuery%20MCP%20Quick%20Start.md) - BigQuery billing exports

---

## 🎓 Tutorials by Skill Level

### Beginner
- [AWS Pricing MCP Quickstart](./01-aws-pricing-mcp-quickstart.md) ⭐ Start here
- [Cost Analysis with Amazon Q](./02.%20cost-analysis-with-AmazonQ.md)
- [Azure MCP Quick Start](./04.%20Azure%20MCP%20quick%20Start.md)

### Intermediate
- [GCP BigQuery MCP Quick Start](./05.%20GCP%20BigQuery%20MCP%20Quick%20Start.md)

### Advanced
- [FinOps Multi-Agent with Nova](./03.finops_multi_agent_with_nova/guidelines.md)

---

## 🛠️ What You'll Need

### General Prerequisites
- **MCP Client**: Choose from [9 available clients](../clients/INDEX.md)
  - Recommended for beginners: [Claude Desktop](../clients/2.%20claude.md) or [ChatGPT](../clients/5.%20chatgpt.md)
  - Recommended for developers: [Claude Code](../clients/8.%20claude-code.md) or [VS Code](../clients/1.%20vscode.md)

### Cloud Provider Prerequisites
- **AWS Tutorials**: AWS account with IAM permissions ([see IAM guide](../tooling-governance/security-privileges-aws.md))
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
1. **Choose your production client**: Review [Client Comparison](../clients/Comparison.md)
2. **Set up security**: Read [MCP Security Best Practices](../tooling-governance/mcp-security-2025.md)
3. **Deploy remote servers**: See [Remote MCP Servers Guide](../tooling-governance/remote-mcp-servers.md)
4. **Explore use cases**: Check [FinOps Use Cases](../finops-use-cases/)
5. **Discover more servers**: Browse the [MCP Registry](https://modelcontextprotocol.io/registry)

### Additional Learning Resources
- [MCP Architecture](../Foundations/mcp-architecture.md) - Deep dive into how MCP works
- [MCP Specification 2025-11-25](../Foundations/mcp-architecture.md#-mcp-specification-2025-11-25-updates) - Latest protocol features
- [All MCP Servers](../servers/) - AWS, Azure, GCP server documentation

---

## 🆘 Getting Help

**Stuck on a tutorial?**
- Check the [Getting Started Guide](../Foundations/getting-started.md) for basics
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

**Total Tutorials**: 5
**Cloud Providers Covered**: AWS (3), Azure (1), GCP (1)
**Difficulty Levels**: Beginner (3), Intermediate (1), Advanced (1)
**Average Completion Time**: 25-35 minutes per tutorial

---

← [Back to Home](../README.md) | [View All Clients](../clients/INDEX.md)
