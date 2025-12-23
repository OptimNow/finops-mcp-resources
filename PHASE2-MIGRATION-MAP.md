# Phase 2 Migration Map

**Date**: January 2026
**Purpose**: Track all file and directory renames for Phase 2 reorganization

---

## Directory Renames

```
OLD PATH                    → NEW PATH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Foundations/                → foundations/
finops-use-cases/           → use-cases/
tooling-governance/         → governance/
```

---

## Client File Renames

```
OLD PATH                    → NEW PATH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
clients/1. vscode.md        → clients/vscode.md
clients/2. claude.md        → clients/claude-desktop.md
clients/3. amazonQ.md       → clients/amazon-q.md
clients/4. cursor.md        → clients/cursor.md
clients/5. chatgpt.md       → clients/chatgpt.md
clients/6. gemini.md        → clients/gemini.md
clients/7. copilot.md       → clients/copilot.md
clients/8. claude-code.md   → clients/claude-code.md
clients/9. kiro.md          → clients/kiro.md
clients/Comparison.md       → clients/comparison.md
clients/INDEX.md            → clients/INDEX.md
```

---

## Tutorial File Renames

```
OLD PATH                                            → NEW PATH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
tutorials/01-aws-pricing-quickstart.md         → tutorials/01-aws-pricing-quickstart.md
tutorials/02. cost-analysis-with-AmazonQ.md         → tutorials/02-amazon-q-cost-analysis.md
tutorials/03.finops_multi_agent_with_nova/          → tutorials/03-finops-multi-agent-nova/ (flatten)
tutorials/04. Azure MCP quick Start.md              → tutorials/04-azure-mcp-quickstart.md
tutorials/05. GCP BigQuery MCP Quick Start.md       → tutorials/05-gcp-bigquery-quickstart.md
tutorials/INDEX.md                                  → tutorials/INDEX.md
```

---

## Other File Renames

```
OLD PATH                                → NEW PATH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
tooling-governance/Anonymisation.md    → governance/anonymisation.md
tooling-governance/MCPServerPortal.md   → governance/mcp-server-portal.md
tooling-governance/Vantage.md           → governance/vantage-integration.md
tooling-governance/security-privileges-aws.md → governance/security-aws-iam-policies.md
tooling-governance/mcp-security-2025.md       → governance/security-best-practices-2025.md
tooling-governance/remote-mcp-servers.md      → governance/remote-mcp-servers.md
```

---

## Files That Reference Paths (Need Updates)

### High Priority (Many references)
- README.md
- Foundations/getting-started.md → foundations/getting-started.md
- clients/INDEX.md
- clients/Comparison.md → clients/comparison.md
- tutorials/INDEX.md

### Medium Priority
- All client files (*.md in clients/)
- All tutorial files (*.md in tutorials/)
- Foundations/what-is-mcp.md → foundations/what-is-mcp.md
- Foundations/mcp-architecture.md → foundations/mcp-architecture.md

### Low Priority
- CONTRIBUTING.md
- UPDATE-SUMMARY-DEC-2025.md
- REPO-STRUCTURE-RECOMMENDATIONS.md

---

## Git Commands for Renaming

Git preserves history when using `git mv`:

```bash
# Directories
git mv Foundations foundations
git mv finops-use-cases use-cases
git mv tooling-governance governance

# Clients
git mv "clients/1. vscode.md" clients/vscode.md
git mv "clients/2. claude.md" clients/claude-desktop.md
git mv "clients/3. amazonQ.md" clients/amazon-q.md
git mv clients/Comparison.md clients/comparison.md

# Tutorials
git mv tutorials/01-aws-pricing-quickstart.md tutorials/01-aws-pricing-quickstart.md
git mv "tutorials/02. cost-analysis-with-AmazonQ.md" tutorials/02-amazon-q-cost-analysis.md
git mv "tutorials/04. Azure MCP quick Start.md" tutorials/04-azure-mcp-quickstart.md
git mv "tutorials/05. GCP BigQuery MCP Quick Start.md" tutorials/05-gcp-bigquery-quickstart.md

# Flatten tutorial 03
git mv tutorials/03-finops-multi-agent-nova.md tutorials/03-finops-multi-agent-nova.md

# Governance files
git mv tooling-governance/Anonymisation.md governance/anonymisation.md
git mv tooling-governance/MCPServerPortal.md governance/mcp-server-portal.md
git mv tooling-governance/Vantage.md governance/vantage-integration.md
git mv tooling-governance/security-privileges-aws.md governance/security-aws-iam-policies.md
git mv tooling-governance/mcp-security-2025.md governance/security-best-practices-2025.md
git mv tooling-governance/remote-mcp-servers.md governance/remote-mcp-servers.md
```

---

## Link Update Patterns

### Pattern 1: Foundation directory references
```
FIND: /foundations/
REPLACE: /foundations/

FIND: (./foundations/)
REPLACE: (./foundations/)
```

### Pattern 2: Client file references
```
FIND: /clients/vscode.md
REPLACE: /clients/vscode.md

FIND: /clients/claude-desktop.md
REPLACE: /clients/claude-desktop.md

... (continue for all numbered clients)
```

### Pattern 3: Tutorial file references
```
FIND: 01-aws-pricing-quickstart.md
REPLACE: 01-aws-pricing-quickstart.md

... (continue for all tutorials)
```

---

## Testing Checklist

After all renames and link updates:
- [ ] README.md loads correctly
- [ ] All links in README.md work
- [ ] foundations/getting-started.md loads and all links work
- [ ] clients/INDEX.md loads and all links work
- [ ] tutorials/INDEX.md loads and all links work
- [ ] All client files load
- [ ] All tutorial files load
- [ ] All governance files load
- [ ] No broken internal links (check with link checker)

---

## Rollback Plan

If issues arise:
1. Checkout previous commit: `git checkout c3db0a8`
2. Create new branch: `git checkout -b phase2-rollback`
3. Cherry-pick specific fixes if needed
4. Or start Phase 2 again with corrections

---

**Status**: Ready to execute
**Estimated Time**: 30-45 minutes
**Risk Level**: Medium (many file moves, link updates required)
