← [Back to Servers](./INDEX.md) | [Home](../README.md)

---

# FinOps Tagging Compliance MCP Server

**Last Updated**: March 2026

An MCP server for enforcing and auditing cloud resource tagging policies — a core FinOps discipline for cost allocation, showback, and chargeback.

---

## Overview

The FinOps Tagging MCP server provides AI-assisted tag governance across cloud environments. It connects your tagging policies to LLM workflows, enabling automated compliance checks, drift detection, and remediation guidance without manual audits.

**Repository**: [finops-tagging-mcp](https://github.com/OptimNow/finops-tagging-mcp) *(community)*
**Status**: Beta
**Category**: Tagging & Governance

---

## Capabilities

| Tool | Description |
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
| `generate_custodian_policy` | Generate Cloud Custodian policies for enforcement |
| `generate_openops_workflow` | Create OpenOps automation workflows |
| `schedule_compliance_audit` | Set up recurring compliance checks |

---

## FinOps Use Cases

**Cost Allocation Accuracy**
Run `check_tag_compliance` across all accounts to identify resources that cannot be attributed to a business unit, then use `suggest_tags` to generate remediation recommendations.

**Tagging Drift Monitoring**
Use `detect_tag_drift` to catch tags that have been modified or removed since last audit — critical for maintaining reliable showback/chargeback data.

**Compliance Reporting**
Generate executive-ready reports with `generate_compliance_report` showing compliance percentages by account, service, and tag key. Export raw data with `export_violations_csv` for integration with FinOps platforms.

---

## Configuration Example

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

---

## Security Notes

- **Read-only by default** — compliance checks do not modify resources
- Policy generation tools (`generate_custodian_policy`, `generate_openops_workflow`) produce configuration files but do not deploy them
- Ensure the IAM role used has read-only access to resource tags (e.g., `tag:GetResources`, `resourcegroupstaggingapi:GetResources`)

---

← [Back to Servers](./INDEX.md) | [Home](../README.md)
