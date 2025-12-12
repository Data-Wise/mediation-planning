# Claude Statistical Research Extension - Implementation Plan

**Created**: 2025-12-08
**Status**: Planning Complete - Ready for Implementation
**Priority**: High

---

## Executive Summary

Build a unified **Claude Statistical Research Extension** that works with both Claude Desktop and Claude CLI, providing comprehensive AI assistance for statistical methodology research, R package development, manuscript writing, and teaching.

---

## Architecture Decision

**Chosen Approach**: Hybrid (MCP-First + CLI Enhancements)

```
Extension = MCP Server (core, works with Desktop + CLI)
          + CLI Extensions (skills, commands, hooks for Claude Code)
```

### Rationale
- MCP is the unified plugin system for BOTH Claude Desktop and Claude CLI
- Building MCP-first ensures maximum compatibility
- CLI extensions provide enhanced experience for primary workflow

---

## Repository Structure

```
github.com/Data-Wise/claude-statistical-research/
│
├── README.md
├── LICENSE
├── install.sh                    # One-command install (Desktop + CLI)
├── uninstall.sh
├── update.sh
│
├── mcp-server/                   # CORE: Works with Desktop + CLI
│   ├── package.json
│   ├── tsconfig.json
│   ├── src/
│   │   ├── index.ts              # MCP server entry point
│   │   ├── tools/
│   │   │   ├── r-execute.ts      # Run R code
│   │   │   ├── r-inspect.ts      # Inspect R objects
│   │   │   ├── r-test.ts         # Run package tests
│   │   │   ├── r-check.ts        # R CMD check
│   │   │   ├── r-document.ts     # Generate docs
│   │   │   ├── github-pr.ts      # PR management
│   │   │   ├── github-issues.ts  # Issue management
│   │   │   ├── bibtex-add.ts     # Add citations
│   │   │   ├── bibtex-search.ts  # Search citations
│   │   │   └── simulation.ts     # Simulation helpers
│   │   └── resources/
│   │       └── r-session.ts      # Persistent R session
│   └── dist/                     # Compiled output
│
├── cli-extensions/               # CLI-ONLY enhancements
│   ├── skills/
│   │   ├── statistical-research-workflow.md  (created 2025-12-08)
│   │   ├── manuscript-writing-guide.md       (to create)
│   │   └── statistical-simulation-guide.md   (to create)
│   ├── commands/
│   │   ├── rpkg-check.md         # /rpkg-check
│   │   ├── sim-design.md         # /sim-design
│   │   ├── manuscript-sync.md    # /manuscript-sync
│   │   └── ecosystem-health.md   # /ecosystem-health
│   └── hooks/
│       ├── pre-commit            # R CMD check before commit
│       └── on-project-enter      # Auto-activate skills
│
├── configs/                      # Config templates
│   ├── claude-desktop.json       # For Claude Desktop
│   └── claude-cli.json           # For Claude Code
│
├── workflows/                    # Agent workflow templates
│   ├── rpkg/
│   │   ├── dev.yml
│   │   ├── release.yml
│   │   └── ecosystem-check.yml
│   ├── simulation/
│   │   └── design.yml
│   └── manuscript/
│       └── submit.yml
│
└── docs/
    ├── installation.md
    ├── tools-reference.md
    ├── workflows.md
    └── contributing.md
```

---

## Implementation Phases

### Phase 1: Foundation (Week 1)
**Goal**: Core MCP server with R execution

- [ ] Create repository structure
- [ ] Build MCP server skeleton
- [ ] Implement `r_execute` tool (run R code)
- [ ] Implement `r_inspect` tool (object inspection)
- [ ] Implement `r_test` tool (run tests)
- [ ] Implement `r_check` tool (R CMD check)
- [ ] Test with Claude Desktop
- [ ] Test with Claude CLI

**Deliverable**: Working R Console MCP server

### Phase 2: CLI Extensions (Week 2)
**Goal**: Skills and slash commands

- [ ] Create `manuscript-writing-guide.md` skill
- [ ] Create `statistical-simulation-guide.md` skill
- [ ] Create `/rpkg-check` slash command
- [ ] Create `/sim-design` slash command
- [ ] Create `/ecosystem-health` slash command
- [ ] Write install.sh script
- [ ] Test full installation

**Deliverable**: Complete CLI extension pack

### Phase 3: Enhanced Tools (Week 3)
**Goal**: GitHub and citation integration

- [ ] Configure existing GitHub MCP server
- [ ] Implement `bibtex_add` tool
- [ ] Implement `bibtex_search` tool
- [ ] Add simulation helper tools
- [ ] Create git hooks (pre-commit)
- [ ] Document all tools

**Deliverable**: Full-featured MCP server

### Phase 4: Agent Workflows (Week 4)
**Goal**: Automated multi-step workflows

- [ ] Create ecosystem health check workflow
- [ ] Create simulation design workflow
- [ ] Create manuscript preparation workflow
- [ ] Test agent orchestration
- [ ] Polish documentation
- [ ] Prepare for public release (optional)

**Deliverable**: Production-ready extension

---

## MCP Server Tools Specification

### R Console Tools

| Tool | Purpose | Parameters | Returns |
|------|---------|------------|---------|
| `r_execute` | Run R code | `code: string` | stdout, stderr, plots |
| `r_inspect` | Inspect object | `object: string` | structure, summary |
| `r_test` | Run tests | `package?: string, filter?: string` | test results |
| `r_check` | R CMD check | `package?: string` | check output |
| `r_document` | Generate docs | `package?: string` | success/errors |
| `r_install` | Install package | `package: string` | success/errors |
| `r_load` | Load package | `package: string` | success/errors |

### GitHub Tools (via existing MCP)

| Tool | Purpose | Parameters |
|------|---------|------------|
| `github_create_pr` | Create PR | repo, branch, title, body |
| `github_list_issues` | List issues | repo, state |
| `github_search_code` | Search code | repo, query |

### Citation Tools

| Tool | Purpose | Parameters | Returns |
|------|---------|------------|---------|
| `bibtex_add` | Add citation | doi or bibtex | success |
| `bibtex_search` | Search local bib | query | matching entries |
| `bibtex_format` | Format citation | key, style | formatted text |

---

## Skills to Create

### 1. manuscript-writing-guide.md

**Purpose**: JASA/Biostatistics manuscript standards

**Contents**:
- Journal-specific requirements (abstract length, structure)
- VanderWeele notation standards
- Proof formatting guidelines
- Simulation study reporting standards
- Real data application structure
- Reference formatting

### 2. statistical-simulation-guide.md

**Purpose**: Simulation study best practices

**Contents**:
- Parameter grid design
- Performance metrics (bias, coverage, MSE, power)
- Morris et al. (2019) guidelines
- Reproducibility requirements
- Reporting templates
- Common pitfalls

---

## Slash Commands to Create

### /rpkg-check
Quick R package health check for current directory.

### /sim-design
Interactive simulation study design wizard.

### /ecosystem-health
Check all 6 mediationverse packages.

### /manuscript-sync
Verify manuscript matches package implementation.

---

## Agent Workflows

### Ecosystem Health Check
```yaml
name: Ecosystem Health Check
agents:
  - type: explore
    packages: [medfit, probmed, rmediation, medrobust, medsim, mediationverse]
    tasks:
      - Check R CMD check status
      - Verify test coverage >80%
      - Check for outdated dependencies
      - Identify breaking change risks
output: Health report with recommendations
```

### Simulation Design
```yaml
name: Simulation Study Design
agents:
  - type: explore
    task: Find similar simulations in medsim
  - type: plan
    task: Design parameter grid following Morris et al.
output: Simulation code scaffold + validation plan
```

---

## Configuration Files

### Claude Desktop (~/Library/Application Support/Claude/claude_desktop_config.json)
```json
{
  "mcpServers": {
    "statistical-research": {
      "command": "node",
      "args": ["~/.claude/extensions/statistical-research/mcp-server/dist/index.js"],
      "env": {
        "R_HOME": "/usr/local/bin/R",
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

### Claude CLI (~/.claude/settings.json)
```json
{
  "mcpServers": {
    "statistical-research": {
      "command": "node",
      "args": ["~/.claude/extensions/statistical-research/mcp-server/dist/index.js"]
    }
  }
}
```

---

## Existing Resources

### Already Created
- `~/.claude/skills/statistical-research-workflow.md` - Master orchestration skill
- `~/.claude/skills/r-package-development.md` - R workflow
- `~/.claude/skills/emacs-r-devkit.md` - Emacs integration
- `~/.claude/skills/doc-management.md` - Documentation workflow
- `~/.claude/skills/data-wise-documentation.md` - Documentation standards

### Already Exists (Use)
- `@modelcontextprotocol/server-github` - GitHub MCP server
- `~/projects/code/zsh-claude-workflow/` - Workflow automation

### To Create
- MCP server for R Console
- manuscript-writing-guide skill
- statistical-simulation-guide skill
- Slash commands
- Git hooks

---

## Success Metrics

1. **R Code Execution**: Claude can run R code and see results
2. **Package Management**: Claude can run rdev, rtest, rcheck via tools
3. **Cross-Platform**: Works with both Claude Desktop and CLI
4. **One-Command Install**: `./install.sh` sets up everything
5. **Ecosystem Awareness**: Claude understands 6-package dependencies

---

## Dependencies

### Required
- Node.js >= 18.0.0
- R >= 4.1.0
- Claude CLI (for full features)
- Git

### R Packages
- languageserver
- devtools
- testthat
- roxygen2
- pkgdown

### npm Packages
- @modelcontextprotocol/sdk
- typescript

---

## Open Questions

1. **Public vs Private**: Release as open source for R/stats community?
2. **R Session Management**: Persistent session vs spawn per call?
3. **Plot Handling**: Return as base64, temp file, or viewer?
4. **BibTeX Location**: Single global file or per-project?

---

## References

- [MCP Specification](https://modelcontextprotocol.io/)
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [Existing MCP Servers](https://github.com/modelcontextprotocol/servers)
- VanderWeele (2015) - Notation standards
- Morris et al. (2019) - Simulation guidelines

---

## Notes

This plan was developed through brainstorming session on 2025-12-08.
Key insight: MCP is the unified plugin system for both Claude Desktop and CLI.

**Next Action**: When ready to implement, start with Phase 1 (MCP server core).

---

**Last Updated**: 2025-12-08
**Author**: Claude + DT
