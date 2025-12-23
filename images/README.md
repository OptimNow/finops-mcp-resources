# Images Directory

This directory contains screenshots and visual assets for the FinOps MCP Resources documentation.

## Directory Structure

```
images/
├── clients/          # Screenshots for client documentation
│   ├── chatgpt/     # ChatGPT MCP screenshots
│   ├── claude-code/ # Claude Code MCP screenshots
│   ├── kiro/        # Kiro MCP screenshots
│   ├── gemini/      # Google Gemini MCP screenshots
│   ├── copilot/     # Microsoft Copilot MCP screenshots
│   ├── vscode/      # VS Code MCP screenshots
│   ├── cursor/      # Cursor MCP screenshots
│   ├── claude-desktop/  # Claude Desktop MCP screenshots
│   └── amazon-q/    # Amazon Q MCP screenshots
│
├── architecture/    # Architecture diagrams (if not using Mermaid)
│   └── (Optional: PNG/SVG exports of diagrams)
│
└── tutorials/       # Tutorial screenshots
    ├── 01-aws-pricing-quickstart/
    ├── 02-amazon-q-cost-analysis/
    ├── 03-finops-multi-agent-nova/
    ├── 04-azure-mcp-quickstart/
    └── 05-gcp-bigquery-quickstart/
```

## Image Guidelines

### File Naming Convention
- Use kebab-case for all filenames
- Be descriptive but concise
- Include step numbers for sequential screenshots

Examples:
- `chatgpt-integrations-menu.png`
- `kiro-workspace-config.png`
- `claude-code-remote-server-setup.png`
- `01-pricing-query-result.png`

### Image Requirements
- **Format**: PNG (preferred) or JPG
- **Resolution**: Minimum 1280px wide for desktop screenshots
- **File Size**: Optimize to < 500KB per image (use compression tools)
- **Content**: Crop to relevant area, remove sensitive information

### Screenshot Best Practices
1. **Consistency**: Use same theme/UI settings across similar screenshots
2. **Clarity**: Ensure text is readable at normal zoom levels
3. **Privacy**: Remove or blur any sensitive data (API keys, account IDs, real billing amounts)
4. **Context**: Include enough UI context to orient users
5. **Annotations**: Add arrows/highlights only when necessary (prefer clean screenshots)

## Client Screenshots Needed

### ChatGPT (`clients/chatgpt/`)
- [ ] `integrations-menu.png` - ChatGPT Integrations menu location
- [ ] `mcp-config-panel.png` - MCP server configuration interface
- [ ] `mcp-tool-usage.png` - ChatGPT using an MCP tool (e.g., AWS pricing query)
- [ ] `available-tools-indicator.png` - UI showing connected MCP servers

### Kiro (`clients/kiro/`)
- [ ] `workspace-config.png` - Workspace-level MCP configuration
- [ ] `kiro-config-json.png` - `.kiro/config.json` file structure
- [ ] `agent-hooks-example.png` - Agent hooks in action
- [ ] `mcp-server-connected.png` - Kiro showing connected MCP server

### Claude Code (`clients/claude-code/`)
- [ ] `remote-server-config.png` - Remote MCP server configuration
- [ ] `status-bar-servers.png` - Status bar showing connected MCP servers
- [ ] `tool-invocation.png` - Claude Code invoking an MCP tool
- [ ] `task-workflow-example.png` - Task workflow with MCP 2025-11-25 spec

### Google Gemini (`clients/gemini/`)
- [ ] `gemini-mcp-setup.png` - Gemini MCP integration setup
- [ ] `bigquery-integration.png` - BigQuery billing export query
- [ ] `vertex-ai-config.png` - Vertex AI MCP configuration

### Microsoft Copilot (`clients/copilot/`)
- [ ] `copilot-studio-mcp.png` - Copilot Studio MCP configuration
- [ ] `azure-integration.png` - Azure Cost Management query
- [ ] `m365-integration.png` - Microsoft 365 integration example

### VS Code (`clients/vscode/`)
- [ ] `mcp-extension-install.png` - MCP extension installation
- [ ] `server-config-ui.png` - Server configuration UI
- [ ] `mcp-output-panel.png` - MCP output/logs panel

### Cursor (`clients/cursor/`)
- [ ] `cursor-mcp-settings.png` - Cursor MCP settings panel
- [ ] `server-installation.png` - Installing MCP server in Cursor
- [ ] `tool-usage-example.png` - Using MCP tool in Cursor

### Claude Desktop (`clients/claude-desktop/`)
- [ ] `config-file-location.png` - Location of config file
- [ ] `server-configuration.png` - Server configuration in JSON
- [ ] `connected-servers.png` - UI showing connected servers

### Amazon Q (`clients/amazon-q/`)
- [ ] `amazon-q-mcp-setup.png` - Amazon Q MCP configuration
- [ ] `cost-explorer-query.png` - Querying Cost Explorer through MCP
- [ ] `aws-integration.png` - AWS-native features

## Tutorial Screenshots Needed

### Tutorial 01: AWS Pricing Quickstart
- [ ] `01-install-claude-desktop.png`
- [ ] `02-config-file-edit.png`
- [ ] `03-first-pricing-query.png`
- [ ] `04-query-results.png`

### Tutorial 02: Amazon Q Cost Analysis
- [ ] `01-amazon-q-setup.png`
- [ ] `02-cost-explorer-connection.png`
- [ ] `03-natural-language-query.png`
- [ ] `04-cost-breakdown.png`

### Tutorial 03: FinOps Multi-Agent with Nova
- [ ] `01-nova-setup.png`
- [ ] `02-multi-agent-config.png`
- [ ] `03-workflow-execution.png`
- [ ] `04-cost-optimization-results.png`

### Tutorial 04: Azure MCP Quick Start
- [ ] `01-azure-mcp-install.png`
- [ ] `02-cost-management-auth.png`
- [ ] `03-billing-query.png`
- [ ] `04-multi-cloud-setup.png`

### Tutorial 05: GCP BigQuery Quick Start
- [ ] `01-bigquery-setup.png`
- [ ] `02-billing-export-config.png`
- [ ] `03-sql-query-example.png`
- [ ] `04-cost-analysis-results.png`

## How to Add Screenshots

1. **Capture Screenshots**: Follow the guidelines above
2. **Optimize Images**: Use tools like TinyPNG, ImageOptim, or Squoosh
3. **Place in Correct Directory**: Match the directory structure above
4. **Verify Markdown Links**: Check that placeholder references work
5. **Test Rendering**: View in GitHub to ensure images display correctly

## Markdown Reference Syntax

Images are referenced in markdown files using:

```markdown
![Alt Text](../images/clients/chatgpt/integrations-menu.png)
```

Or with captions:

```markdown
### Configuration Panel
![ChatGPT MCP Configuration Panel showing the integrations menu](../images/clients/chatgpt/mcp-config-panel.png)
*Figure: ChatGPT MCP server configuration interface*
```

## Questions?

For questions about images or screenshots, see [CONTRIBUTING.md](../CONTRIBUTING.md) or open an issue.
