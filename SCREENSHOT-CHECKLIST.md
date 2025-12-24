# Screenshot Checklist

This checklist helps you capture the screenshots needed to complete the visual documentation for the FinOps MCP Resources repository.

## ✅ What's Already Done

1. **Architecture Diagrams** - Added Mermaid.js diagrams to `governance/remote-mcp-servers.md`:
   - Local vs Remote MCP comparison
   - Enterprise cloud-hosted pattern
   - Mid-sized team shared server pattern
   - Hybrid local + remote pattern

2. **Image Directory Structure** - Created organized folders for all screenshots:
   - `images/clients/` - 9 client subdirectories
   - `images/tutorials/` - 5 tutorial subdirectories
   - `images/architecture/` - Optional static diagrams

3. **Screenshot Guidelines** - Comprehensive guide in `images/README.md`:
   - File naming conventions (kebab-case)
   - Image requirements (PNG, < 500KB, 1280px+ wide)
   - Privacy and optimization best practices

4. **Example Placeholders** - Added to `clients/chatgpt.md` as template

## 📸 Priority Screenshots to Capture

### High Priority (Most Impact)

#### ChatGPT (`images/clients/chatgpt/`)
- [xx ] `integrations-menu.png` - Settings → Integrations menu
- [x ] `mcp-config-panel.png` - MCP server configuration interface
- [x ] `mcp-tool-usage.png` - ChatGPT querying AWS pricing via MCP
- [x ] `available-tools-indicator.png` - UI showing connected servers

#### Kiro (`images/clients/kiro/`)
- [ ] `workspace-config.png` - Workspace-level MCP configuration
- [ ] `kiro-config-json.png` - `.kiro/config.json` file structure
- [ ] `agent-hooks-example.png` - Agent hooks in action

#### Claude Code (`images/clients/claude-code/`)
- [ ] `remote-server-config.png` - Remote MCP server setup
- [ ] `status-bar-servers.png` - Status bar showing MCP servers
- [ ] `tool-invocation.png` - Invoking AWS Cost Explorer MCP

### Medium Priority (Good to Have)

#### VS Code (`images/clients/vscode/`)
- [ ] `mcp-extension-install.png` - Installing MCP extension
- [ ] `server-config-ui.png` - Server configuration interface

#### Amazon Q (`images/clients/amazon-q/`)
- [ ] `amazon-q-mcp-setup.png` - Amazon Q MCP configuration
- [ ] `cost-explorer-query.png` - Querying Cost Explorer

#### Claude Desktop (`images/clients/claude-desktop/`)
- [ ] `config-file-location.png` - JSON config file location
- [ ] `connected-servers.png` - UI showing connected servers

### Lower Priority (Nice to Have)

#### Gemini (`images/clients/gemini/`)
- [ ] `gemini-mcp-setup.png` - Gemini MCP integration
- [ ] `bigquery-integration.png` - BigQuery billing query

#### Copilot (`images/clients/copilot/`)
- [ ] `copilot-studio-mcp.png` - Copilot Studio configuration
- [ ] `azure-integration.png` - Azure Cost Management query

#### Cursor (`images/clients/cursor/`)
- [ ] `cursor-mcp-settings.png` - Cursor MCP settings
- [ ] `server-installation.png` - Installing MCP server

## 📝 How to Capture Screenshots

### 1. Setup Each Client
For each client (ChatGPT, Kiro, Claude Code, etc.):
1. Install/open the client application
2. Configure at least one MCP server (e.g., AWS Pricing)
3. Test that the connection works

### 2. Capture Key Steps
Take screenshots of:
1. **Where to find MCP settings** (menu location, settings panel)
2. **Configuration interface** (JSON config, UI panels)
3. **Successful connection** (indicators, status bars)
4. **Tool in action** (actual MCP query and result)

### 3. Optimize Images
Before adding to repository:
1. **Crop** to relevant UI area (remove unnecessary chrome)
2. **Blur/remove** any sensitive data (API keys, real billing amounts, account IDs)
3. **Optimize** file size using:
   - [TinyPNG](https://tinypng.com/) - Online compression
   - [ImageOptim](https://imageoptim.com/) - Mac app
   - [Squoosh](https://squoosh.app/) - Web-based
4. **Target**: < 500KB per image, PNG format preferred

### 4. Add to Repository
1. **Place files** in correct directory (e.g., `images/clients/chatgpt/`)
2. **Use kebab-case** naming (e.g., `integrations-menu.png`)
3. **Verify** the markdown placeholders display correctly

## 🎨 Screenshot Best Practices

### Consistency
- Use **same theme** across similar screenshots (light/dark mode)
- Use **same zoom level** for UI screenshots
- Capture at **native resolution** (1280px+ wide)

### Privacy
- **Blur** or replace sensitive data:
  - API keys and tokens
  - AWS account IDs
  - Real billing amounts (use sample data instead)
  - Internal hostnames or IP addresses
  - Email addresses

### Clarity
- Ensure **text is readable** at normal zoom
- Include **enough context** (don't crop too tightly)
- Avoid **annotations** unless absolutely necessary (prefer clean screenshots)

## 📚 Tutorial Screenshots (Optional)

If you want to add visual aids to tutorials:

### Tutorial 01: AWS Pricing Quickstart (`images/tutorials/01-aws-pricing-quickstart/`)
- [ ] `01-install-claude-desktop.png`
- [ ] `02-config-file-edit.png`
- [ ] `03-first-pricing-query.png`
- [ ] `04-query-results.png`

### Tutorial 02-05
See `images/README.md` for complete list.

## 🚀 Quick Start Example

**Capturing ChatGPT screenshots (5-10 minutes):**

1. Open ChatGPT Desktop app
2. Navigate to Settings → Integrations
   - **Screenshot** → Save as `integrations-menu.png`
3. Add/configure an MCP server (AWS Pricing)
   - **Screenshot** the configuration panel → Save as `mcp-config-panel.png`
4. Return to main chat, verify MCP tools are available
   - **Screenshot** the indicator → Save as `available-tools-indicator.png`
5. Ask ChatGPT to query AWS EC2 pricing
   - **Screenshot** the query and response → Save as `mcp-tool-usage.png`
6. Optimize all 4 images with TinyPNG
7. Place in `images/clients/chatgpt/`
8. Commit and push (or provide to me to integrate)

## ✅ After Capturing Screenshots

Once you have screenshots:

### Option 1: Add Yourself
1. Place images in correct directories
2. Verify markdown references work (check on GitHub)
3. Commit and push changes

### Option 2: Share with Me
1. Provide screenshots (via file share or repository)
2. Tell me which client/tutorial each screenshot is for
3. I'll integrate them into the documentation with proper markdown syntax

## 📊 Progress Tracking

Track your progress:
- **High Priority**: 11 screenshots across 3 clients (ChatGPT, Kiro, Claude Code)
- **Medium Priority**: 6 screenshots across 3 clients (VS Code, Amazon Q, Claude Desktop)
- **Lower Priority**: 6 screenshots across 3 clients (Gemini, Copilot, Cursor)
- **Tutorials**: Optional, 4-5 screenshots per tutorial

**Recommended approach**: Start with ChatGPT (4 screenshots) as the example, then expand to other high-priority clients.

---

## 🎯 Goal

Complete visual documentation that:
- ✅ Shows users exactly where to configure MCP in each client
- ✅ Demonstrates MCP servers working with real FinOps queries
- ✅ Provides visual confirmation of successful setup
- ✅ Makes the repository more professional and user-friendly

**Current Status**:
- ✅ Architecture diagrams complete (Mermaid.js)
- ✅ Directory structure ready
- ✅ Placeholder markdown added to ChatGPT guide
- ⏳ Screenshots pending (you capture and add)

Good luck! 🚀
