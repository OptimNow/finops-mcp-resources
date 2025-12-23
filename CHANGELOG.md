# Changelog

All notable changes to the FinOps MCP Resources repository are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html) where applicable.

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
