← [Back to Servers](./INDEX.md) | [Home](../README.md)

---

# JIRA MCP Server

**Last Updated**: March 2026

An MCP server for integrating Atlassian JIRA with FinOps workflows — enabling automated ticket creation, tracking, and project management for cost optimization initiatives.

---

## Overview

The JIRA MCP server connects LLM-driven FinOps workflows to your existing JIRA instance. When an AI agent detects a cost anomaly, tagging violation, or optimization opportunity, it can automatically create and manage JIRA tickets — closing the loop between insight and action.

**Repository**: [atlassian/mcp-server-jira](https://github.com/atlassian/mcp-server-jira)
**Status**: GA
**Category**: Workflow & Collaboration

---

## Capabilities

| Tool | Description |
|------|-------------|
| `create_issue` | Create JIRA issues (stories, tasks, bugs) |
| `search_issues` | Search issues using JQL queries |
| `get_issue` | Retrieve issue details by key |
| `update_issue` | Update issue fields, status, or assignee |
| `add_comment` | Add comments to existing issues |
| `list_projects` | List available JIRA projects |
| `get_transitions` | Get available status transitions |
| `transition_issue` | Move issues through workflow states |

---

## FinOps Use Cases

**Cost Anomaly Ticketing**
When a cost anomaly is detected (e.g., via AWS Cost Explorer MCP), automatically create a JIRA ticket assigned to the responsible team with anomaly details, affected resources, and recommended actions.

**Optimization Tracking**
Create JIRA epics for optimization initiatives (rightsizing, Reserved Instance purchases, idle resource cleanup) and track savings realization through ticket lifecycle.

**Tagging Remediation Workflows**
Pair with the Tagging MCP server: when `find_untagged_resources` discovers violations, create JIRA tickets assigned to resource owners with tag suggestions and compliance deadlines.

---

## Configuration Example

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

---

## Security Notes

- **Requires API token** — use a dedicated service account with minimal permissions
- Scope JIRA permissions to specific projects relevant to FinOps (avoid org-wide admin access)
- API tokens should be stored in environment variables or a secrets manager, never in config files committed to version control

---

← [Back to Servers](./INDEX.md) | [Home](../README.md)
