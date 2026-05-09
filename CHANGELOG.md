# Changelog

All notable changes to the FinOps MCP Resources repository are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html) where applicable.

---

## [May 2026] - 2026-05-09

### Changed
- **AWS MCP Server GA (May 6, 2026)** — Updated documentation to reflect general availability. The `aws-mcp:InvokeMcp`, `aws-mcp:CallReadOnlyTool`, and `aws-mcp:CallReadWriteTool` permissions are retired and have no effect; access is now controlled through standard IAM policies. The `aws:ViaAWSMCPService` (Boolean) and `aws:CalledViaAWSMCP` (service principal String) global condition context keys are added to every MCP-initiated request, intended for `Deny`-style scoping per the AWS-documented pattern.
- **Skills replace Agent SOPs** — Agent SOPs are renamed to **Skills** and are now contributed and maintained by individual AWS service teams. Updated `servers/aws.md`, `tutorials/07-aws-mcp-remote-server.md`, and `servers/configs/registry.yaml`.
- **Tutorials/Servers refresh** — `tutorials/07-aws-mcp-remote-server.md` and `servers/aws.md` updated to "May 2026" and rewritten around the GA model. The "Unified Architecture (Preview)" block in `servers/aws.md` is removed (superseded by GA).

### Added
- **`run_script` tool** documented in `tutorials/07-aws-mcp-remote-server.md` and `servers/aws.md` — server-side Python sandbox that lets the agent chain multiple AWS calls in a single round-trip.
- **`retrieve_skill` tool** documented in `tutorials/07-aws-mcp-remote-server.md` and `servers/aws.md` — loads a specific Skill on demand. Both files also note the `aws___` prefix used by MCP clients when listing tools.
- **Frankfurt (`eu-central-1`) endpoint** documented alongside `us-east-1`: `https://aws-mcp.eu-central-1.api.aws/mcp`.
- **Observability narrative** — CloudWatch metrics under the `AWS-MCP` namespace and CloudTrail audit visibility, with the new context keys used to separate human and agent traffic.
- **Pricing line** in `servers/aws.md`: no charge for the server itself; pay only for AWS resources used and data transfer.
- **GA note** in `governance/remote-mcp-servers.md` (one sentence) noting the May 6, 2026 GA and the unauthenticated documentation tools.
- **Credential-method note** in `tutorials/07-aws-mcp-remote-server.md` — points readers to AWS-recommended `aws login` (auto-rotates credentials every 15 minutes) and `aws configure sso` as alternatives to static IAM access keys, while keeping the IAM-user path for users without SSO.
- **MCP Proxy for AWS explanation** in `tutorials/07-aws-mcp-remote-server.md` and `servers/aws.md` — clarifies what `uvx mcp-proxy-for-aws@latest` actually invokes (the local STDIO-to-HTTPS bridge that handles SigV4 signing) and links to the [aws/mcp-proxy-for-aws](https://github.com/aws/mcp-proxy-for-aws) repository.

### Fixed
- Stale references to `aws-mcp:InvokeMcp` removed from `tutorials/02-amazon-kiro-cli-cost-analysis.md`.
- Stale "Agent SOPs" reference updated in `servers/configs/registry.yaml`.

---

## [March 2026] - 2026-03-16

### Added
- **New MCP Server Documentation**
  - `servers/tagging.md` - FinOps Tagging Compliance MCP server (tag compliance, drift detection, remediation)
  - `servers/jira.md` - JIRA MCP server for FinOps workflow integration (anomaly ticketing, optimization tracking)
  - `servers/slack.md` - Slack MCP server for FinOps alerting (cost alerts, budget notifications)
- **Registry Backfill** - `servers/configs/registry.yaml` now contains all 18 documented MCP servers (was 1)
- **Cost Simulation Tutorial** - Moved from `use-cases/` to `tutorials/08-cost-simulation/`

### Changed
- **Repository Streamlined**
  - Fixed broken links in `clients/INDEX.md` (old numbered filenames → current names)
  - Removed phantom directory references (`/presentations`, `/resources`) from README
  - Trimmed all INDEX files — removed repeated boilerplate (Getting Started, Contributing, Related Resources)
  - Updated `servers/INDEX.md` with new "Workflow & Collaboration" section
- **Date Updates** - All references updated from January 2026 to March 2026

### Removed
- `use-cases/` directory — content merged into `tutorials/08-cost-simulation/`

---

## [January 2026] - 2026-01-XX

### Added
- **Phase 1 Quick Wins**: Navigation improvements
  - Created `clients/INDEX.md` - Comprehensive client overview with decision guide
  - Created `tutorials/INDEX.md` - Learning path and tutorial catalog
  - Created `CHANGELOG.md` - This file to track repository changes
  - Created `REPO-STRUCTURE-RECOMMENDATIONS.md` - Detailed improvement plan

### Changed
- **Date Updates**: Updated all references from December 2025 to January 2026
  - `README.md` - Updated "Industry Adoption" section
  - `Foundations/what-is-mcp.md` - Updated "MCP's Evolution" section
  - `Foundations/getting-started.md` - Updated ecosystem stats
  - `clients/Comparison.md` - Added "Last Updated: January 2026" header
  - `UPDATE-SUMMARY-DEC-2025.md` - Updated title and dates

- **Getting Started Guide**: Expanded client coverage
  - Added all 9 MCP clients (was only 4)
  - Organized by category: Developers, Business, Cloud-Specific
  - Added link to Client Comparison Guide

- **Client Comparison**: Enhanced with January 2026 updates
  - Updated recommendation headers to "January 2026"
  - Added Kiro to all relevant sections
  - Updated bottom line with January 2026 reference

### Fixed
- Missing client documentation in getting-started guide (now includes all 9)

---

## [December 2025] - 2025-12-22/23

### Added
- **New Client Documentation**: 5 new comprehensive client guides
  - `clients/5. chatgpt.md` - ChatGPT MCP integration (added March 2025)
  - `clients/6. gemini.md` - Google Gemini MCP integration (added April 2025)
  - `clients/7. copilot.md` - Microsoft Copilot MCP integration (GA 2025)
  - `clients/8. claude-code.md` - Claude Code with remote MCP support (June 2025)
  - `clients/9. kiro.md` - Kiro agentic IDE integration guide (preview)

- **Governance & Security**:
  - `tooling-governance/mcp-security-2025.md` - Comprehensive enterprise security guide
  - `tooling-governance/remote-mcp-servers.md` - Remote MCP deployment architecture guide

- **Documentation**:
  - `UPDATE-SUMMARY-DEC-2025.md` - Comprehensive change log for December updates

### Changed
- **README.md**: Major updates
  - Added MCP donation to Agentic AI Foundation (Linux Foundation)
  - Updated ecosystem stats (10,000+ servers)
  - Added new clients: ChatGPT, Gemini, Copilot, Claude Code, Kiro

- **Foundations/what-is-mcp.md**:
  - Added "MCP's Evolution into Industry Standard" section
  - Documented Linux Foundation donation
  - Updated adoption statistics

- **Foundations/mcp-architecture.md**:
  - Added "MCP Specification 2025-11-25 Updates" section
  - Documented task-based workflows
  - Documented enhanced authorization (OAuth CIMD, client credentials, cross-app access, PKCE)
  - Explained FinOps implications

- **Foundations/getting-started.md**:
  - Added "Discover More MCP Servers" section
  - Documented MCP Registry and ecosystem growth
  - Added community directories (MCPdb, Awesome MCP Servers)

- **clients/Comparison.md**: Complete rewrite
  - Updated for all 9 clients (was 4)
  - Added comparison table with all features
  - Segmented recommendations by team type and cloud provider
  - Added "For teams exploring agentic workflows" section

### Context
This was a comprehensive update bringing the repository current with major MCP developments from November-December 2025:
- **November 25, 2025**: MCP Specification 2025-11-25 released (one-year anniversary)
- **December 9, 2025**: MCP donated to Agentic AI Foundation (Linux Foundation)
- **March-April 2025**: Major client adoptions (ChatGPT, Gemini)
- **June 2025**: Claude Code remote MCP support

---

## [Pre-December 2025]

### Existing Content (Baseline)
- Basic client documentation (VS Code, Claude Desktop, Amazon Q, Cursor)
- AWS Pricing MCP tutorial
- Amazon Q cost analysis tutorial
- FinOps multi-agent with Nova tutorial
- Azure MCP quick start (added via PR #4)
- GCP BigQuery MCP quick start (added via PR #4)
- AWS, Azure, GCP server documentation
- Security and governance documentation
- Basic foundations content

---

## Versioning Notes

**Version Scheme**: We use date-based versioning (YYYY-MM) for major updates.

**Update Frequency**:
- **Major updates**: Quarterly (recommended) to stay current with MCP ecosystem
- **Minor updates**: As needed for corrections, new tutorials, or client updates
- **Next review**: March 2026 (quarterly check)

**What to watch for in future updates**:
- New MCP specification releases
- New client integrations (e.g., additional AI platforms)
- New cloud provider MCP servers
- Enterprise adoption case studies
- Security updates or vulnerabilities
- Breaking changes in MCP protocol

---

## Contributing

When making changes to this repository:
1. Update this CHANGELOG.md with your changes
2. Use semantic categories: Added, Changed, Deprecated, Removed, Fixed, Security
3. Reference PR/issue numbers when applicable
4. Keep entries concise but descriptive
5. Group related changes together

See [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed guidelines.

---

## Maintainers

For questions about changelog entries or repository updates:
- Open an issue on [GitHub](https://github.com/OptimNow/finops-mcp-resources/issues)
- Join the [FinOps Foundation Slack](https://www.finops.org/slack/)
- Follow updates on [LinkedIn](https://linkedin.com/in/jeanlatiere)
