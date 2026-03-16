← [Back to Servers](./INDEX.md) | [Home](../README.md)

---

# Slack MCP Server

**Last Updated**: March 2026

An MCP server for integrating Slack with FinOps workflows — enabling cost alerts, budget notifications, and team collaboration directly in Slack channels.

---

## Overview

The Slack MCP server allows AI agents to send messages, read channels, and interact with Slack workspaces. In a FinOps context, this enables automated notifications when cost thresholds are breached, anomalies are detected, or optimization actions require team attention.

**Repository**: [modelcontextprotocol/servers - slack](https://github.com/modelcontextprotocol/servers/tree/main/src/slack)
**Status**: GA
**Category**: Workflow & Collaboration

---

## Capabilities

| Tool | Description |
|------|-------------|
| `send_message` | Post messages to Slack channels |
| `read_channel` | Read recent messages from a channel |
| `list_channels` | List available channels in the workspace |
| `search_messages` | Search message history |
| `add_reaction` | Add emoji reactions to messages |
| `get_thread` | Read threaded conversations |
| `create_channel` | Create new Slack channels |

---

## FinOps Use Cases

**Cost Anomaly Alerts**
When a cost spike is detected, post a structured alert to #finops-alerts with the affected account, service, magnitude of change, and a link to the cost dashboard. Tag the responsible team for rapid response.

**Budget Threshold Notifications**
Notify #finance or team-specific channels when budget consumption reaches 80%, 90%, or 100% thresholds, including current spend, forecasted month-end spend, and recommended actions.

**Cross-Team Collaboration**
Post optimization recommendations to relevant team channels (e.g., "3 idle EC2 instances in staging detected — projected savings: $420/mo") and track acknowledgment via thread replies or reactions.

---

## Configuration Example

```json
{
  "mcpServers": {
    "slack": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-slack"],
      "env": {
        "SLACK_BOT_TOKEN": "xoxb-your-bot-token",
        "SLACK_TEAM_ID": "T0123456789"
      }
    }
  }
}
```

---

## Security Notes

- **Use a dedicated Slack bot** with minimal channel permissions — restrict to FinOps-related channels only
- Bot tokens (`xoxb-`) are preferred over user tokens (`xoxp-`) for traceability and least-privilege
- Store tokens in environment variables or a secrets manager
- Consider setting up Slack's audit logs to monitor bot activity

---

← [Back to Servers](./INDEX.md) | [Home](../README.md)
