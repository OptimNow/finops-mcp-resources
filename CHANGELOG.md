# Changelog

All notable changes to the FinOps MCP Resources repository are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html) where applicable.

---

## [July 2026] - 2026-07-29

### Changed
- **MCP Specification 2026-07-28** — Documented MCP's fifth specification release (the largest architectural change since launch), now rolling out across Claude products. Added a new "MCP Specification 2026-07-28 Updates" section to `foundations/mcp-architecture.md` covering: (1) the **stateless core** — the `initialize`/`initialized` handshake and `Mcp-Session-Id` header are retired in favour of stateless request/response, with protocol version and client identity carried in `_meta`; (2) the **versioned extensions framework** (`io.modelcontextprotocol/*`), into which **MCP Tasks** (now stable: poll-based `tasks/get` + `tasks/update`) and **MCP Apps** (interactive in-conversation UI) graduate; (3) **authorization hardening** — RFC 9207 issuer validation, issuer-bound client credentials, `application_type` in DCR, and the Enterprise Managed Authorization (EMA) extension aligned with OAuth 2.0 / OIDC (Microsoft Entra, Okta). The prior `2025-11-25` section is reframed as the previous release. Sources: [Anthropic](https://claude.com/blog/bringing-mcp-2026-07-28-to-claude), [MCP spec blog](https://blog.modelcontextprotocol.io/posts/2026-07-28/).
- **Stateless hosting-cost angle** — `governance/remote-mcp-servers.md` gains a "Stateless core (MCP 2026-07-28) and hosting cost" section: serverless / scale-to-zero is now the natural fit for remote FinOps servers, sticky-session load balancing is no longer needed, and plain request/response HTTP is favoured over long-lived SSE. Transport-security note and Additional Resources refreshed for the new spec.
- **`foundations/mcp-architecture.md` corrections** — the "dedicated connection" description and the "MCP doesn't standardize authentication" line are updated for the stateless core and standardized OAuth 2.0 / OIDC.
- **Adoption stat refresh** — `README.md` and `foundations/what-is-mcp.md` updated with the figures Anthropic reported at the July 28, 2026 release: **400+ million monthly SDK downloads** (≈4× year-over-year) and **950+ servers** in Claude's connector directory (attributed to the connector directory specifically, not total ecosystem servers). The May 2026 AAIF summit figures are retained as the prior snapshot.
- **Date-stamp refresh (May 2026 → July 2026)** on `foundations/mcp-architecture.md`, `foundations/what-is-mcp.md`, `governance/remote-mcp-servers.md`, and `governance/mcp-authentication-vulnerabilities-2026.md`.

### Added
- **Authorization note in `governance/mcp-authentication-vulnerabilities-2026.md`** — a "What the 2026-07-28 spec changes" subsection (RFC 9207 issuer validation, issuer-bound credentials, `application_type` DCR, EMA), a July 2026 timeline row, and RFC 9207 added to Related Standards — with a caveat that the spec does not fix deployment-hygiene issues (unauthenticated exposure, DNS rebinding).

---

## [July 2026] - 2026-07-13

### Changed
- **Azure FinOps MCP Server (public preview)** — Refreshed all Azure documentation for Microsoft's June 23, 2026 announcement ([From insight to action](https://azure.microsoft.com/en-us/blog/from-insight-to-action-the-next-phase-of-agentic-cloud-operations/)). The claim that "the official Azure MCP server does NOT expose cost management or billing APIs" is retired: (1) the **ARM MCP Server** (public preview since May 7, 2026, branded **Azure FinOps MCP Server** for cost workflows) gives agents natural-language Azure Resource Graph queries with cost and usage intelligence rolling out; (2) `@azure/mcp` gained a **retail pricing tool** (list prices, reservation and savings plan rates — cost estimation, not actual spend). Updated `servers/azure.md`, `tutorials/04-azure-mcp-quickstart.md`, `servers/INDEX.md`, and `servers/configs/registry.yaml`.
- **Recommendation reframed** — julianobarbosa/azure-finops-mcp-server stays the most direct path to actual Cost Management (billing) data today; the official ARM/FinOps MCP Server is the one to watch (currently requires VS Code + GitHub Copilot; Claude support announced as next).
- **Governance note added** — the ARM MCP Server is NOT read-only: it ships ARM template deployment tools alongside its query tools. Docs now recommend Reader + Resource Graph scoping and/or disabling deployment tools for FinOps-only use (Azure Policy can block MCP-initiated deployments).
- **finopshub-mcp guidance** — now points to Microsoft's official [Configure AI agents for FinOps hubs](https://learn.microsoft.com/en-us/cloud-computing/finops/toolkit/hubs/configure-ai) path as the preferred alternative to the proof-of-concept server.
- **Broken link fixed** — `servers/azure.md` and `tutorials/04-azure-mcp-quickstart.md` pointed to a non-existent `governance/security-privileges-azure.md`; both now link to the governance INDEX.

### Added
- **`arm-mcp-server` registry entry** in `servers/configs/registry.yaml` (maturity: preview, 6 tools: 3 ARG query + 3 ARM deployment) — registry now has 18 entries; documented server count bumped to 19 in README/INDEX.

## [May 2026] - 2026-05-09

### Changed
- **AWS MCP Server GA (May 6, 2026)** — Updated documentation to reflect general availability. The `aws-mcp:InvokeMcp`, `aws-mcp:CallReadOnlyTool`, and `aws-mcp:CallReadWriteTool` permissions are retired and have no effect; access is now controlled through standard IAM policies. The `aws:ViaAWSMCPService` (Boolean) and `aws:CalledViaAWSMCP` (service principal String) global condition context keys are added to every MCP-initiated request, intended for `Deny`-style scoping per the AWS-documented pattern.
- **Skills replace Agent SOPs** — Agent SOPs are renamed to **Skills** and are now contributed and maintained by individual AWS service teams. Updated `servers/aws.md`, `tutorials/07-aws-mcp-remote-server.md`, and `servers/configs/registry.yaml`.
- **Tutorials/Servers refresh** — `tutorials/07-aws-mcp-remote-server.md` and `servers/aws.md` updated to "May 2026" and rewritten around the GA model. The "Unified Architecture (Preview)" block in `servers/aws.md` is removed (superseded by GA).
- **Date stamp refresh (March 2026 → May 2026)** in `README.md` (Industry Adoption header), `foundations/what-is-mcp.md`, `foundations/getting-started.md`, `clients/comparison.md`, `servers/gcp.md`, and `governance/mcp-authentication-vulnerabilities-2026.md`.
- **Date-stamp-only sweep** on 16 otherwise-untouched files (`foundations/INDEX.md`, `foundations/mcp-architecture.md`, `servers/INDEX.md`, `servers/azure.md`, `servers/jira.md`, `servers/slack.md`, `servers/tagging.md`, `clients/INDEX.md`, `governance/INDEX.md`, `governance/remote-mcp-servers.md`, `tutorials/INDEX.md`, `tutorials/01-aws-pricing-quickstart.md`, `tutorials/02-amazon-kiro-cli-cost-analysis.md`, `tutorials/03-finops-multi-agent-nova.md`, `tutorials/04-azure-mcp-quickstart.md`, `tutorials/05-gcp-bigquery-quickstart.md`).
- **Stat reframe** — the unsourced "10,000+ active MCP servers" claim is REPLACED across `README.md`, `foundations/what-is-mcp.md`, and `foundations/getting-started.md` with verified [AAIF stats from MCP Dev Summit NA 2026](https://aaif.io/blog/mcp-is-now-enterprise-infrastructure-everything-that-happened-at-mcp-dev-summit-north-america-2026/): 1,200 attendees (doubled from previous summit), 110+ million monthly SDK downloads, 170 member organizations in under 4 months. The "MCPdb" entry stays as a community directory listing in `foundations/getting-started.md`, correctly attributed to MCPdb itself.
- **`servers/gcp.md`** — refreshed for Google's April 29, 2026 announcement of 50+ managed MCP servers GA / preview. Added a "What's new (May 2026)" section, updated the Official Servers table with Cloud Run, Apigee, AlloyDB / Spanner / Firestore / Cloud SQL rows. The standalone `@google/mcp-server-compute` and `@google-cloud/mcp` npm packages return 404 (verified `npm view`, 2026-05-09); the doc now points to the managed remote server as the recommended path.
- **Update cadence** shifted from quarterly to monthly. Next review: June 2026.

### Added
- **`run_script` tool** documented in `tutorials/07-aws-mcp-remote-server.md` and `servers/aws.md` — server-side Python sandbox that lets the agent chain multiple AWS calls in a single round-trip.
- **`retrieve_skill` tool** documented in `tutorials/07-aws-mcp-remote-server.md` and `servers/aws.md` — loads a specific Skill on demand. Both files also note the `aws___` prefix used by MCP clients when listing tools.
- **Frankfurt (`eu-central-1`) endpoint** documented alongside `us-east-1`: `https://aws-mcp.eu-central-1.api.aws/mcp`.
- **Observability narrative** — CloudWatch metrics under the `AWS-MCP` namespace and CloudTrail audit visibility, with the new context keys used to separate human and agent traffic.
- **Pricing line** in `servers/aws.md`: no charge for the server itself; pay only for AWS resources used and data transfer.
- **GA note** in `governance/remote-mcp-servers.md` (one sentence) noting the May 6, 2026 GA and the unauthenticated documentation tools.
- **Credential-method note** in `tutorials/07-aws-mcp-remote-server.md` — points readers to AWS-recommended `aws login` (auto-rotates credentials every 15 minutes) and `aws configure sso` as alternatives to static IAM access keys, while keeping the IAM-user path for users without SSO.
- **MCP Proxy for AWS explanation** in `tutorials/07-aws-mcp-remote-server.md` and `servers/aws.md` — clarifies what `uvx mcp-proxy-for-aws@latest` actually invokes (the local STDIO-to-HTTPS bridge that handles SigV4 signing) and links to the [aws/mcp-proxy-for-aws](https://github.com/aws/mcp-proxy-for-aws) repository.
- **AWS for SAP MCP Server row** added to `servers/aws.md` (GA May 1, 2026, sourced from the awslabs/mcp release).
- **Recent Updates bullets** in `README.md` for the Google managed MCP servers GA (April 29, 2026) and the May 2026 governance refresh.
- **`clients/cursor.md`** — "What's new (May 2026)" section covering Cursor 3.0 / 3.2 / 3.3 (parallel agents, async subagents, multi-root workspaces, PR review experience) and Security Review beta on Teams / Enterprise.
- **`clients/kiro.md`** — Kiro joining the AWS Agent Toolkit (May 6, 2026) alongside Claude Code, Cursor, Codex, Cline, and Windsurf, plus Kiro IDE 0.12.155 parallel task execution.
- **`clients/kiro-cli.md`** — Kiro CLI 2.2.0 adaptive thinking (April 27, 2026).
- **`clients/claude-code.md`** — MCP Support Timeline updated with April 2026 enhancements (`/mcp` tool counts, retry logic, "needs auth" labels) and Claude Opus 4.7 GA (May 4, 2026) becoming the default model across Claude products.
- **MCP Dev Summit NA 2026 references** added across `README.md` (Industry Adoption paragraph + Recent Updates bullet), `foundations/what-is-mcp.md` (Broad Adoption section), `foundations/getting-started.md` (Discover More intro), and `foundations/mcp-architecture.md` (new "Governance milestones (May 2026)" section covering the TSC three-stage lifecycle policy and Mazin Gilbert as new AAIF Executive Director).
- **The Lethal Trifecta** new framing section in `governance/mcp-authentication-vulnerabilities-2026.md` (sourced from Amazon's James Hood, MCP Dev Summit NA 2026). Includes a FinOps-specific worked example.
- **DNS Rebinding** added as a new attack pattern + timeline row in `governance/mcp-authentication-vulnerabilities-2026.md` (Jonathan Leitschuh / Braise disclosure at MCP Dev Summit NA 2026; 0-day in Google Database Toolbox after 90-day awareness).
- **Enterprise patterns from MCP Dev Summit NA 2026** new section in `governance/remote-mcp-servers.md` covering MCP Gateway pattern (Alex Salazar / Arcade), Uber two-tier trust model, AND-gate authorization, Prime Video progressive tool discovery (100 to 3), and WorkOS context lazy-loading. The last two are flagged with explicit FinOps token-cost angles.

### Fixed
- Stale references to `aws-mcp:InvokeMcp` removed from `tutorials/02-amazon-kiro-cli-cost-analysis.md`.
- Stale "Agent SOPs" reference updated in `servers/configs/registry.yaml`.
- Broken link to non-existent `clients/vscode.md` corrected to `clients/vscode-cline.md` in `foundations/getting-started.md` and `tutorials/06-Tutorial-GCP-Billing-MCP.md`.
- Unverified "407% increase from initial batch" stat removed from `foundations/getting-started.md`.

### Security
- **CVE-2026-0621** (severity High) — MCP TypeScript SDK `UriTemplate` ReDoS, patched in v1.25.2 (2026-04-02). Documented in `governance/mcp-authentication-vulnerabilities-2026.md`.
- **CVE-2026-40933** (severity Critical) — Flowise authenticated RCE via MCP Adapters, patched in 3.1.0 (April 2026). Documented in `governance/mcp-authentication-vulnerabilities-2026.md`.

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
- **Major updates**: Monthly (recommended) to stay current with MCP ecosystem
- **Minor updates**: As needed for corrections, new tutorials, or client updates
- **Next review**: June 2026 (monthly check)

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
