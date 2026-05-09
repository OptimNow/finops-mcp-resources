# FinOps Tagging Compliance MCP Server

**Last Updated**: May 2026

An MCP server for enforcing and auditing cloud resource tagging policies — a core FinOps discipline for cost allocation, showback, and chargeback. Automate tag compliance checks, detect drift, and close your cost attribution gap using AI-powered cloud cost optimization workflows.

---

## Why does tagging matter for cloud cost optimization?

Tagging is the foundation of FinOps cost allocation. Without consistent, accurate tags, organizations cannot attribute cloud spend to business units, track optimization savings, or run reliable showback/chargeback. Industry data shows that **30-50% of cloud resources** lack the tags needed for accurate cost attribution — leading to unaccountable spend, missed optimization opportunities, and governance blind spots.

Manual tag audits are slow and error-prone. The FinOps Tagging MCP server connects your tagging policies directly to AI agent workflows, enabling continuous automated compliance checks, drift detection, and remediation guidance across AWS, Azure, and GCP environments.

**Repository**: [finops-tagging-mcp](https://github.com/OptimNow/finops-tagging-mcp) *(community)*
**Status**: Beta
**Category**: Tagging & Governance

---

## What can this MCP server do?

| Tool | What it does |
|------|-------------|
| `check_tag_compliance` | Validate resources against your tagging policy |
| `find_untagged_resources` | Discover resources missing required tags |
| `suggest_tags` | AI-powered tag suggestions based on resource metadata |
| `generate_compliance_report` | Produce compliance summaries across accounts |
| `detect_tag_drift` | Identify tags that have changed or been removed |
| `get_tagging_policy` | Retrieve current tagging policy definitions |
| `validate_resource_tags` | Check specific resource tags against policy rules |
| `get_cost_attribution_gap` | Measure unattributed spend due to missing tags |
| `export_violations_csv` | Export violation data for downstream reporting |
| `generate_custodian_policy` | Generate Cloud Custodian policies for automated enforcement |
| `generate_openops_workflow` | Create OpenOps automation workflows |
| `schedule_compliance_audit` | Set up recurring compliance checks |

---

## How does this help FinOps teams?

### Cost Allocation Accuracy
Run `check_tag_compliance` across all accounts to identify resources that cannot be attributed to a business unit. Then use `suggest_tags` to generate AI-powered remediation recommendations — reducing your cost attribution gap without manual spreadsheet work.

### Tagging Drift Monitoring
Use `detect_tag_drift` to catch tags that have been modified or removed since last audit. This is critical for maintaining reliable showback/chargeback data — a single drifted `CostCenter` tag can misattribute thousands of dollars in monthly spend.

### Compliance Reporting for Stakeholders
Generate executive-ready reports with `generate_compliance_report` showing compliance percentages by account, service, and tag key. Export raw data with `export_violations_csv` for integration with FinOps platforms like Vantage, CloudHealth, or Apptio.

### Automated Enforcement Policy Generation
Use `generate_custodian_policy` to create Cloud Custodian policies that automatically enforce your tagging standards. Pair with `generate_openops_workflow` to build end-to-end remediation pipelines that notify resource owners and auto-tag where possible.

### Measuring the Cost Attribution Gap
`get_cost_attribution_gap` calculates how much of your cloud spend cannot be attributed due to missing or invalid tags — giving leadership a clear metric to track governance improvements over time.

---

## How do I set it up?

```json
{
  "mcpServers": {
    "finops-tagging": {
      "command": "npx",
      "args": ["-y", "finops-tagging-mcp"],
      "env": {
        "AWS_PROFILE": "your-profile",
        "TAGGING_POLICY_PATH": "./tagging-policy.yaml"
      }
    }
  }
}
```

### Prerequisites
- An AWS account (or Azure/GCP credentials) with read-only access to resource tags
- A tagging policy file (YAML) defining required tags, allowed values, and enforcement rules
- Node.js 18+ installed locally

---

## What permissions does it need?

- **Read-only by default** — compliance checks do not modify resources
- Policy generation tools (`generate_custodian_policy`, `generate_openops_workflow`) produce configuration files but do not deploy them
- Minimum IAM permissions: `tag:GetResources`, `resourcegroupstaggingapi:GetResources`
- For cost attribution gap analysis: `ce:GetCostAndUsage` (read-only)

---

## How does it work with other MCP servers?

The tagging MCP pairs naturally with other FinOps MCP servers:

- **+ JIRA MCP**: When `find_untagged_resources` discovers violations, automatically create JIRA tickets assigned to resource owners with tag suggestions and compliance deadlines
- **+ Slack MCP**: Post compliance summaries to #finops-governance channels, alert teams when drift is detected
- **+ AWS Cost Explorer MCP**: Correlate untagged resources with their actual spend to prioritize remediation by cost impact

---

← [Back to Servers](./INDEX.md) | [Home](../README.md)
