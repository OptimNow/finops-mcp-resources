# JIRA MCP Server for FinOps Workflow Automation

**Last Updated**: May 2026

An MCP server for integrating Atlassian JIRA with FinOps workflows — enabling automated ticket creation for cost anomalies, optimization tracking, and remediation management. Close the loop between AI-driven cost insights and team action.

---

## Why connect JIRA to your FinOps workflows?

Cloud cost optimization generates insights — cost anomalies, rightsizing recommendations, idle resources, tagging violations. But insights without action are worthless. The gap between "AI detected a cost spike" and "someone actually fixed it" is where most FinOps value is lost.

The JIRA MCP server bridges this gap. When an AI agent detects a cost anomaly via AWS Cost Explorer, a tagging violation via the Tagging MCP, or an optimization opportunity, it can automatically create and manage JIRA tickets — assigned to the right team, with full context, tracked through resolution. This turns FinOps automation from "reporting" into "action."

**Repository**: [atlassian/mcp-server-jira](https://github.com/atlassian/mcp-server-jira)
**Status**: GA
**Category**: Workflow & Collaboration

---

## What can this MCP server do?

| Tool | What it does |
|------|-------------|
| `create_issue` | Create JIRA issues (stories, tasks, bugs) with full field support |
| `search_issues` | Search issues using JQL queries |
| `get_issue` | Retrieve issue details by key |
| `update_issue` | Update issue fields, status, or assignee |
| `add_comment` | Add comments to existing issues |
| `list_projects` | List available JIRA projects |
| `get_transitions` | Get available status transitions for a workflow |
| `transition_issue` | Move issues through workflow states |

---

## How does this help FinOps teams?

### Cost Anomaly Ticketing
When a cost spike is detected (e.g., via AWS Cost Explorer MCP or Azure Cost Management), the AI agent automatically creates a JIRA ticket assigned to the responsible team. The ticket includes anomaly details — affected account, service, magnitude of change, time window — plus recommended investigation steps. No more Slack messages that get buried or emails that go unread.

### Optimization Initiative Tracking
Create JIRA epics for cloud cost optimization initiatives: rightsizing campaigns, Reserved Instance / Savings Plan purchases, idle resource cleanup, storage tier migrations. Track savings realization through the ticket lifecycle — from identification to implementation to validation. This gives FinOps leaders a clear view of optimization ROI.

### Tagging Remediation Workflows
Pair with the [Tagging MCP server](./tagging.md): when `find_untagged_resources` discovers violations, automatically create JIRA tickets assigned to resource owners with AI-generated tag suggestions and compliance deadlines. Track remediation progress and escalate overdue violations.

### Budget Breach Response
When a budget threshold is crossed, create a P1 JIRA ticket with the budget details, current spend trajectory, forecasted month-end overage, and a checklist of immediate actions (freeze non-essential deployments, review recent provisioning changes, contact top-spending teams).

---

## How do I set it up?

```json
{
  "mcpServers": {
    "jira": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-jira"],
      "env": {
        "JIRA_URL": "https://your-org.atlassian.net",
        "JIRA_EMAIL": "your-email@company.com",
        "JIRA_API_TOKEN": "your-api-token"
      }
    }
  }
}
```

### Prerequisites
- An Atlassian Cloud or JIRA Data Center instance
- A JIRA API token ([create one here](https://id.atlassian.com/manage-profile/security/api-tokens))
- A dedicated service account with project-scoped permissions
- Node.js 18+ installed locally

### What JIRA project structure works best for FinOps?

Consider creating a dedicated JIRA project (e.g., `FINOPS`) with issue types:
- **Cost Anomaly** — unexpected spend spikes requiring investigation
- **Optimization** — rightsizing, commitment purchases, resource cleanup
- **Tag Violation** — resources missing required tags
- **Budget Alert** — threshold breaches and forecast overages

---

## What permissions and security controls are needed?

- **Use a dedicated service account** — not a personal account — for traceability and access control
- Scope JIRA permissions to specific FinOps-related projects (avoid org-wide admin access)
- API tokens must be stored in environment variables or a secrets manager — never in config files committed to version control
- Enable JIRA audit logs to monitor automated ticket creation activity
- Consider rate limiting: JIRA Cloud allows ~100 requests/minute per user

---

## How does it work with other MCP servers?

- **+ Tagging MCP**: Auto-create tickets for tagging violations with AI-generated remediation suggestions
- **+ Slack MCP**: Post ticket creation notifications to FinOps Slack channels for visibility
- **+ AWS Cost Explorer MCP**: Enrich anomaly tickets with cost data, affected resources, and historical spend context
- **+ CloudWatch MCP**: Attach relevant metrics and logs to optimization tickets

---

← [Back to Servers](./INDEX.md) | [Home](../README.md)
