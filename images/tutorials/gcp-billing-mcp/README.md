# GCP Billing MCP Tutorial Screenshots

This directory contains screenshots for the [GCP Billing Export MCP Setup Tutorial](../../../tutorials/Tutorial-GCP-Billing-MCP.md).

## Required Screenshots

### Prerequisites Section (Steps 1a-1f)

1. **01-billing-export-settings.png**
   - **What to capture**: GCP Console → Billing → Billing Export page showing BigQuery export configured
   - **Key elements**: Active billing export with dataset name visible
   - **Privacy**: Blur any account IDs or real billing amounts

2. **02-gcloud-install.png**
   - **What to capture**: Terminal showing `gcloud --version` output
   - **Key elements**: Version number displayed
   - **Privacy**: None needed

3. **03-gcloud-auth.png**
   - **What to capture**: Browser window during `gcloud auth application-default login` flow
   - **Key elements**: Google sign-in page or permissions screen
   - **Privacy**: Blur email address if visible

4. **04-gcloud-project-set.png**
   - **What to capture**: Terminal showing `gcloud projects list` and `gcloud config set project` commands
   - **Key elements**: List of projects and successful project selection
   - **Privacy**: OK to show project IDs (they're not sensitive)

5. **05-api-enabled.png**
   - **What to capture**: Terminal showing output from `gcloud services list --enabled` with BigQuery visible
   - **Key elements**: bigquery.googleapis.com in the list
   - **Privacy**: None needed

6. **06-node-pnpm-install.png**
   - **What to capture**: Terminal showing `node -v`, `npm -v`, and `pnpm -v` output
   - **Key elements**: Version numbers displayed
   - **Privacy**: None needed

---

### Installation Section (Steps 2a-2c)

7. **07-git-clone.png**
   - **What to capture**: Terminal showing `git clone https://github.com/krzko/google-cloud-mcp.git` output
   - **Key elements**: Successful clone with file count
   - **Privacy**: None needed

8. **08-pnpm-install.png**
   - **What to capture**: Terminal showing `pnpm install` output (last few lines showing success)
   - **Key elements**: "dependencies installed" or similar success message
   - **Privacy**: None needed

9. **09-pnpm-build.png**
   - **What to capture**: Terminal showing `pnpm build` output and `ls dist` showing index.js
   - **Key elements**: Build success and dist directory contents
   - **Privacy**: None needed

---

### VS Code Configuration (Steps 3a-3f)

10. **10-vscode-gemini-extension.png**
    - **What to capture**: VS Code Extensions marketplace showing Gemini Code Assist extension
    - **Key elements**: Extension name, install button or "installed" status
    - **Privacy**: None needed

11. **11-vscode-open-settings.png**
    - **What to capture**: VS Code Command Palette showing "Open User Settings (JSON)" option
    - **Key elements**: Command palette with search result highlighted
    - **Privacy**: None needed

12. **12-vscode-settings-json.png**
    - **What to capture**: VS Code settings.json file showing MCP configuration
    - **Key elements**: Full `geminicodeassist.mcpServers` configuration block
    - **Privacy**: Blur username in file paths if desired

13. **13-vscode-reload.png**
    - **What to capture**: VS Code Command Palette showing "Reload Window" command
    - **Key elements**: Command highlighted
    - **Privacy**: None needed

14. **14-vscode-output-mcp.png**
    - **What to capture**: VS Code Output panel (select "Gemini Code Assist" from dropdown) showing MCP server connection logs
    - **Key elements**: "google-cloud-mcp" initialization messages
    - **Privacy**: None needed

15. **15-vscode-gemini-query.png**
    - **What to capture**: VS Code showing Gemini response to "What are my top 5 GCP services by cost over the past 30 days?"
    - **Key elements**: Query text and AI response with billing data
    - **Privacy**: Blur any real cost amounts or project names if desired (or use sample data)

---

### Gemini CLI Configuration (Steps 4a-4e)

16. **16-gemini-cli-install.png**
    - **What to capture**: Terminal showing `npm install -g @google/gemini-cli` and `gemini --version`
    - **Key elements**: Installation success and version number
    - **Privacy**: None needed

17. **17-gemini-settings-dir.png**
    - **What to capture**: Terminal showing creation of `~/.gemini` directory
    - **Key elements**: Command and successful execution
    - **Privacy**: None needed

18. **18-gemini-settings-json.png**
    - **What to capture**: Text editor showing `~/.gemini/settings.json` with MCP configuration
    - **Key elements**: Full MCP server configuration
    - **Privacy**: Blur username in file paths if desired

19. **19-gemini-cli-mcp-list.png**
    - **What to capture**: Gemini CLI showing output of `/mcp list` command
    - **Key elements**: "google-cloud-mcp" in the list of available servers
    - **Privacy**: None needed

20. **20-gemini-cli-query.png**
    - **What to capture**: Gemini CLI response to billing query (e.g., "What are my top 5 GCP services by cost in the last 30 days?")
    - **Key elements**: Query text and AI response with cost data
    - **Privacy**: Blur any real cost amounts or project names if desired (or use sample data)

---

## Screenshot Guidelines

### Technical Requirements
- **Format**: PNG (preferred)
- **Resolution**: Minimum 1280px wide
- **File Size**: Optimize to < 500KB per image
- **Theme**: Consistent light or dark theme across similar screenshots

### Privacy & Security
- **Blur sensitive data**:
  - Real billing amounts (unless using sample data)
  - Email addresses
  - Internal project names (optional)
- **OK to show**:
  - GCP project IDs (not sensitive)
  - API names
  - Version numbers
  - File paths (with username optionally blurred)

### Optimization Tools
- [TinyPNG](https://tinypng.com/) - Online compression
- [ImageOptim](https://imageoptim.com/) - Mac app
- [Squoosh](https://squoosh.app/) - Web-based

### Capture Tips
1. **Crop tightly** - Focus on relevant UI/terminal area
2. **Maximize text readability** - Ensure terminal text is clear
3. **Include enough context** - Don't crop too tightly
4. **Use real workflow** - Actually run the commands as you screenshot

---

## Quick Start for Capturing All Screenshots

**Estimated time**: 30-40 minutes for all 20 screenshots

### Part 1: Prerequisites (screenshots 1-6)
1. Open GCP Console → Billing Export (screenshot 1)
2. Open terminal, run version checks (screenshots 2, 5, 6)
3. Run `gcloud auth` flow (screenshot 3)
4. Run `gcloud projects` commands (screenshot 4)

### Part 2: Installation (screenshots 7-9)
1. Open terminal, clone repo (screenshot 7)
2. Run `pnpm install` (screenshot 8)
3. Run `pnpm build` and `ls dist` (screenshot 9)

### Part 3: VS Code Setup (screenshots 10-15)
1. Open VS Code, go to Extensions (screenshot 10)
2. Open Command Palette (screenshots 11, 13)
3. Edit settings.json (screenshot 12)
4. Check Output panel (screenshot 14)
5. Query Gemini with billing question (screenshot 15)

### Part 4: Gemini CLI Setup (screenshots 16-20)
1. Install Gemini CLI in terminal (screenshot 16)
2. Create settings directory (screenshot 17)
3. Edit settings.json (screenshot 18)
4. Start Gemini and run `/mcp list` (screenshot 19)
5. Query billing data (screenshot 20)

---

## Uploading Screenshots

Once captured and optimized:

1. **Place files in this directory**: `images/tutorials/gcp-billing-mcp/`
2. **Use exact filenames** as listed above (e.g., `01-billing-export-settings.png`)
3. **Verify markdown references** work by viewing the tutorial on GitHub
4. **Commit and push** to the repository

---

**Need help?** See the main [images/README.md](../../README.md) for detailed screenshot guidelines.
