# VS Code + Cline: MCP Client Setup

**Download Visual Studio Code**: https://code.visualstudio.com/download
**Install Cline Extension**: https://marketplace.visualstudio.com/items?itemName=saoudrizwan.claude-dev

## What is Cline?

**Cline** is the MCP client that runs inside VS Code. While VS Code provides the editor environment, **Cline is the extension that actually connects to MCP servers** and provides the AI chat interface. Think of VS Code as the container and Cline as the AI agent living inside it.

Cline gives you a chat panel in VS Code where you can interact with AI models while they access your MCP servers (like AWS pricing, GCP BigQuery, or Azure resources). This creates a one-stop environment with files, terminal, MCP connections, and AI prompting all in one place.

## Getting Started with Cline

### 1. Install Cline Extension

1. Open VS Code
2. Go to Extensions (Ctrl+Shift+X / Cmd+Shift+X)
3. Search for "Cline"
4. Click **Install** on the extension by saoudrizwan

After installation, you'll see a chat icon in the VS Code sidebar.

### 2. Configure Your API Key

Cline requires an API key to connect to an LLM provider. You have several options:

**Option A: Anthropic API Key** (recommended for Claude models)
- Get your key at: https://console.anthropic.com/
- Best for: Claude Sonnet, Claude Opus models
- Pricing: Pay-per-use (no subscription needed)

**Option B: OpenAI API Key**
- Get your key at: https://platform.openai.com/api-keys
- Best for: GPT-4, GPT-4o models
- Pricing: Pay-per-use

**Option C: OpenRouter** (access to dozens of models)
- Get your key at: https://openrouter.ai/
- Best for: Access to multiple providers (Anthropic, OpenAI, Google, Mistral, etc.)
- Includes models like: Claude, GPT-4, Mistral, DeepSeek Coder, and more
- Pricing: Varies by model, often cheaper than direct API access

### 3. Add Your API Key to Cline

1. Click the Cline icon in VS Code sidebar
2. Click the settings/gear icon in the Cline panel
3. Select your API provider (Anthropic, OpenAI, or OpenRouter)
4. Paste your API key
5. Choose your preferred model

### 4. Configure MCP Servers

MCP server configurations are stored at:
- **Windows**: `C:\Users\<YourUsername>\AppData\Roaming\Code\User\globalStorage\saoudrizwan.claude-dev\settings\cline_mcp_settings.json`
- **Mac/Linux**: `~/.vscode/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`

See the [Azure MCP tutorial](../tutorials/04-azure-mcp-quickstart.md) for an example of configuring MCP servers with Cline.

## Cost Considerations

**Cline itself is free**, but you pay for:
- **API usage**: Charges from Anthropic, OpenAI, or OpenRouter based on tokens used
- **MCP server costs**: Some cloud APIs (like AWS Cost Explorer) charge per query

**Tip**: OpenRouter often offers cheaper pricing than direct API access and gives you flexibility to switch between models without changing API keys.

✅ **Pros for a Cloud FinOps professional**
- One environment: code, command terminal, MCP configs, and even LLM prompts live side by side.
- Easy installs: many GitHub repos include a one-click “Install in VS Code” button, reducing setup friction.
- Huge extension ecosystem: FinOps scripts, cost dashboards, and automation tools are easier to manage.
- Customizable workflows: tailor the environment for reporting, optimization scripts, or MCP testing.
- Familiarity: widely used by devs and ops teams, lowering adoption barriers across orgs.

⚠️ **Cons for a Cloud FinOps professional**
- Dev-oriented bias: heavy IDE, may feel overkill if your FinOps tasks are more reporting/analysis than coding.
- Setup complexity: MCP servers still require config, credentials, and ongoing maintenance.
- Distraction risk: with terminal, code, and AI prompts all in one place, easy to get lost tinkering.
- Not a governance solution: same as Cursor/Claude, lacks guardrails, audit logs, and enterprise-grade access controls.
- Resource usage: can be heavier than simpler FinOps dashboards or CLI tools.



👉 **In short:** VS Code + MCP + Copilot gives you an integrated cockpit for prototyping FinOps automations and testing AI-assisted workflows. It’s a powerful sandbox, but its dev-centric nature and lack of governance controls make it better suited to experimentation than to enterprise-grade FinOps platforms.
