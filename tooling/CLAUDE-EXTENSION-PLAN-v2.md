# Claude Statistical Research Extension - Comprehensive Plan v2

**Created**: 2025-12-08
**Updated**: 2025-12-12
**Status**: Phase 1-7 COMPLETE ✅ + 23 Bonus Skills
**Supersedes**: CLAUDE-EXTENSION-PLAN.md
**Skills Inventory**: See SKILLS-INVENTORY.md for complete list

---

## Executive Summary

Build a unified **Claude Statistical Research Extension** that integrates:
1. **Interdisciplinary Skills Framework** - 30+ specialized skills for statistical methodology research ✅ IMPLEMENTED
2. **MCP Server** - Live R execution, literature management, proof verification (Desktop + CLI) ✅ OPERATIONAL
3. **Agent Workflows** - Autonomous multi-step research tasks
4. **CLI Enhancements** - Slash commands, hooks ✅ IMPLEMENTED

**ACTUAL STATUS (2025-12-12):**
- 30+ skills implemented at `/Users/dt/.claude/skills/`
- All 7 core research skills from original plan completed
- 23+ additional bonus skills created
- 5 skills currently active in Claude Desktop project
- See SKILLS-INVENTORY.md for complete listing

---

## Architecture Overview

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                    CLAUDE STATISTICAL RESEARCH EXTENSION                     │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ╔══════════════════════════════════════════════════════════════════════╗   │
│  ║                      SKILL HIERARCHY                                  ║   │
│  ║            (15 Skills - Flat directories, semantic naming)            ║   │
│  ╠══════════════════════════════════════════════════════════════════════╣   │
│  ║                                                                       ║   │
│  ║  FOUNDATIONS (4)              IDEATION (3)                           ║   │
│  ║  ├─ mathematical-foundations  ├─ cross-disciplinary-ideation         ║   │
│  ║  ├─ cs-theory-foundations     ├─ literature-gap-finder               ║   │
│  ║  ├─ physics-statistics-bridge └─ method-transfer-engine              ║   │
│  ║  └─ computational-biology                                            ║   │
│  ║                                                                       ║   │
│  ║  MATHEMATICAL-WORK (4)        COMPUTATION (4)                        ║   │
│  ║  ├─ proof-architect           ├─ algorithm-designer                  ║   │
│  ║  ├─ derivation-engine         ├─ complexity-analyzer                 ║   │
│  ║  ├─ identification-theory     ├─ numerical-methods                   ║   │
│  ║  └─ asymptotic-theory         └─ simulation-architect                ║   │
│  ║                                                                       ║   │
│  ║  WRITING (3)                  DEVELOPMENT (2)                        ║   │
│  ║  ├─ methods-paper-writer      ├─ r-package-architect                 ║   │
│  ║  ├─ mathematical-exposition   └─ python-package-architect            ║   │
│  ║  └─ proof-verifier                                                   ║   │
│  ╚══════════════════════════════════════════════════════════════════════╝   │
│                                    │                                         │
│                                    ▼                                         │
│  ╔══════════════════════════════════════════════════════════════════════╗   │
│  ║                        MCP SERVER                                     ║   │
│  ║          (Works with Claude Desktop AND Claude CLI)                   ║   │
│  ╠══════════════════════════════════════════════════════════════════════╣   │
│  ║                                                                       ║   │
│  ║  R CONSOLE              LITERATURE           PROOF/MATH              ║   │
│  ║  ├─ r_execute           ├─ arxiv_search      ├─ latex_compile        ║   │
│  ║  ├─ r_inspect           ├─ crossref_lookup   ├─ notation_check       ║   │
│  ║  ├─ r_test              ├─ bibtex_add        └─ symbolic_compute     ║   │
│  ║  ├─ r_check             ├─ bibtex_search                             ║   │
│  ║  └─ r_document          └─ paper_summarize                           ║   │
│  ║                                                                       ║   │
│  ║  GITHUB                 SIMULATION          PACKAGE                  ║   │
│  ║  ├─ github_pr           ├─ sim_design       ├─ pkg_create            ║   │
│  ║  ├─ github_issues       ├─ sim_run          ├─ pkg_check             ║   │
│  ║  └─ github_search       └─ sim_analyze      └─ pkg_release           ║   │
│  ╚══════════════════════════════════════════════════════════════════════╝   │
│                                    │                                         │
│                                    ▼                                         │
│  ╔══════════════════════════════════════════════════════════════════════╗   │
│  ║                          AGENTS                                       ║   │
│  ║              (Autonomous Multi-Step Research Tasks)                   ║   │
│  ╠══════════════════════════════════════════════════════════════════════╣   │
│  ║                                                                       ║   │
│  ║  RESEARCH                      DEVELOPMENT                           ║   │
│  ║  ├─ Literature-Gap-Finder      ├─ Ecosystem-Health                   ║   │
│  ║  ├─ Method-Transfer-Agent      ├─ Cross-Package-Refactor             ║   │
│  ║  ├─ Cross-Discipline-Connect   ├─ Simulation-Runner                  ║   │
│  ║  └─ Proof-Verification         └─ Release-Coordinator                ║   │
│  ║                                                                       ║   │
│  ║  WRITING                       TEACHING                              ║   │
│  ║  ├─ Manuscript-Section-Writer  ├─ Exercise-Generator                 ║   │
│  ║  ├─ Notation-Synchronizer      ├─ Concept-Explainer                  ║   │
│  ║  └─ Bibliography-Builder       └─ Solution-Checker                   ║   │
│  ╚══════════════════════════════════════════════════════════════════════╝   │
│                                    │                                         │
│                                    ▼                                         │
│  ╔══════════════════════════════════════════════════════════════════════╗   │
│  ║                      SLASH COMMANDS (CLI Only)                        ║   │
│  ╠══════════════════════════════════════════════════════════════════════╣   │
│  ║  /proof-check  /lit-gap    /sim-design   /rpkg-check   /ecosystem    ║   │
│  ║  /derive       /transfer   /notation     /manuscript   /cite         ║   │
│  ╚══════════════════════════════════════════════════════════════════════╝   │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## Directory Structure

```
github.com/Data-Wise/claude-statistical-research/
│
├── README.md
├── LICENSE (MIT)
├── install.sh                      # One-command install (Desktop + CLI)
├── uninstall.sh
├── update.sh
│
├── skills/                         # FLAT STRUCTURE (Claude Code requirement)
│   │
│   │── # FOUNDATIONS (4)
│   ├── mathematical-foundations/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       ├── measure-theory.md
│   │       ├── functional-analysis.md
│   │       ├── optimal-transport.md
│   │       └── differential-geometry.md
│   │
│   ├── cs-theory-foundations/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       ├── complexity-theory.md
│   │       ├── optimization.md
│   │       └── online-learning.md
│   │
│   ├── physics-statistics-bridge/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       ├── statistical-mechanics.md
│   │       └── information-thermodynamics.md
│   │
│   ├── computational-biology-methods/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       ├── genomics.md
│   │       └── mendelian-randomization.md
│   │
│   │── # IDEATION (3)
│   ├── cross-disciplinary-ideation/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── transfer-template.md
│   │       └── connection-prompts.md
│   │
│   ├── literature-gap-finder/
│   │   ├── SKILL.md
│   │   └── frameworks/
│   │       ├── method-setting-matrix.md
│   │       └── assumption-relaxation-tree.md
│   │
│   ├── method-transfer-engine/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       └── transfer-protocol.md
│   │
│   │── # MATHEMATICAL WORK (4)
│   ├── proof-architect/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── identification-proof.md
│   │       ├── consistency-proof.md
│   │       ├── asymptotic-normality.md
│   │       └── efficiency-proof.md
│   │
│   ├── derivation-engine/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       └── common-derivations.md
│   │
│   ├── identification-theory/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       ├── dag-based.md
│   │       ├── potential-outcomes.md
│   │       └── partial-identification.md
│   │
│   ├── asymptotic-theory/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       ├── influence-functions.md
│   │       ├── semiparametric-efficiency.md
│   │       └── double-robustness.md
│   │
│   │── # COMPUTATION (4)
│   ├── algorithm-designer/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── algorithm-doc-template.md
│   │       └── pseudocode-template.md
│   │
│   ├── complexity-analyzer/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       └── statistical-method-complexities.md
│   │
│   ├── numerical-methods/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       ├── optimization.md
│   │       ├── integration.md
│   │       └── stability.md
│   │
│   ├── simulation-architect/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── simulation-design.md
│   │       └── results-table.md
│   │
│   │── # WRITING (3)
│   ├── methods-paper-writer/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── jasa-template.md
│   │       ├── biometrika-template.md
│   │       ├── biostatistics-template.md
│   │       └── section-templates/
│   │           ├── introduction.md
│   │           ├── methods.md
│   │           ├── simulation.md
│   │           └── discussion.md
│   │
│   ├── mathematical-exposition/
│   │   ├── SKILL.md
│   │   └── reference/
│   │       └── notation-standards.md
│   │
│   ├── proof-verifier/
│   │   ├── SKILL.md
│   │   └── checklists/
│   │       ├── structure-check.md
│   │       ├── step-validation.md
│   │       └── common-errors.md
│   │
│   │── # DEVELOPMENT (2)
│   ├── r-package-architect/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── s7-class.md
│   │       ├── function-template.md
│   │       └── test-template.md
│   │
│   └── python-package-architect/
│       ├── SKILL.md
│       └── templates/
│           └── package-structure.md
│
├── mcp-server/                     # MCP SERVER (Desktop + CLI)
│   ├── package.json
│   ├── tsconfig.json
│   ├── src/
│   │   ├── index.ts                # Main entry point
│   │   │
│   │   ├── tools/
│   │   │   ├── r-console/
│   │   │   │   ├── execute.ts      # r_execute
│   │   │   │   ├── inspect.ts      # r_inspect
│   │   │   │   ├── test.ts         # r_test
│   │   │   │   ├── check.ts        # r_check
│   │   │   │   └── document.ts     # r_document
│   │   │   │
│   │   │   ├── literature/
│   │   │   │   ├── arxiv.ts        # arxiv_search
│   │   │   │   ├── crossref.ts     # crossref_lookup
│   │   │   │   ├── bibtex.ts       # bibtex_add, bibtex_search
│   │   │   │   └── summarize.ts    # paper_summarize
│   │   │   │
│   │   │   ├── proof/
│   │   │   │   ├── latex.ts        # latex_compile
│   │   │   │   ├── notation.ts     # notation_check
│   │   │   │   └── symbolic.ts     # symbolic_compute (Wolfram?)
│   │   │   │
│   │   │   ├── simulation/
│   │   │   │   ├── design.ts       # sim_design
│   │   │   │   ├── run.ts          # sim_run
│   │   │   │   └── analyze.ts      # sim_analyze
│   │   │   │
│   │   │   ├── github/
│   │   │   │   └── index.ts        # Wraps @modelcontextprotocol/server-github
│   │   │   │
│   │   │   └── package/
│   │   │       ├── create.ts       # pkg_create
│   │   │       ├── check.ts        # pkg_check
│   │   │       └── release.ts      # pkg_release
│   │   │
│   │   ├── resources/
│   │   │   ├── r-session.ts        # Persistent R session management
│   │   │   └── bibtex-db.ts        # BibTeX file management
│   │   │
│   │   └── utils/
│   │       ├── latex-parser.ts
│   │       └── notation-linter.ts
│   │
│   └── dist/                       # Compiled output
│
├── commands/                       # SLASH COMMANDS (CLI Only)
│   ├── proof-check.md              # /proof-check
│   ├── derive.md                   # /derive
│   ├── lit-gap.md                  # /lit-gap
│   ├── transfer.md                 # /transfer
│   ├── sim-design.md               # /sim-design
│   ├── notation.md                 # /notation
│   ├── rpkg-check.md               # /rpkg-check
│   ├── ecosystem.md                # /ecosystem
│   ├── manuscript.md               # /manuscript
│   └── cite.md                     # /cite
│
├── agents/                         # AGENT WORKFLOW TEMPLATES
│   ├── research/
│   │   ├── literature-gap.yml
│   │   ├── method-transfer.yml
│   │   ├── cross-discipline.yml
│   │   └── proof-verification.yml
│   │
│   ├── development/
│   │   ├── ecosystem-health.yml
│   │   ├── cross-package-refactor.yml
│   │   ├── simulation-runner.yml
│   │   └── release-coordinator.yml
│   │
│   ├── writing/
│   │   ├── manuscript-section.yml
│   │   ├── notation-sync.yml
│   │   └── bibliography-builder.yml
│   │
│   └── teaching/
│       ├── exercise-generator.yml
│       ├── concept-explainer.yml
│       └── solution-checker.yml
│
├── configs/                        # CONFIGURATION TEMPLATES
│   ├── claude-desktop.json         # For Claude Desktop
│   ├── claude-cli.json             # For Claude Code CLI
│   └── skill-activation.json       # Skill auto-activation rules
│
└── docs/
    ├── installation.md
    ├── skills-reference.md
    ├── mcp-tools-reference.md
    ├── agents-reference.md
    ├── workflows.md
    └── contributing.md
```

---

## Skill Specifications

### Skill Naming Convention (Flat, Semantic)

Since Claude Code requires flat directory structure, use semantic prefixes:

| Category | Prefix | Example |
|----------|--------|---------|
| Foundations | `foundations-` or none | `mathematical-foundations` |
| Ideation | `ideation-` or descriptive | `cross-disciplinary-ideation` |
| Mathematical | `proof-`, `theory-` | `proof-architect`, `asymptotic-theory` |
| Computation | `algo-`, `sim-` | `algorithm-designer`, `simulation-architect` |
| Writing | `paper-`, `writing-` | `methods-paper-writer` |
| Development | `pkg-`, `dev-` | `r-package-architect` |

### Skill YAML Frontmatter Template

```yaml
---
name: skill-name
description: >
  One-sentence description of what this skill does.
  Include trigger phrases: "Use when working on X, Y, or Z."
  Be specific about when Claude should invoke this skill.
allowed-tools:
  - Read
  - Grep
  - Glob
  - WebSearch
  - WebFetch
---
```

### Priority Skills for Immediate Implementation

#### Tier 1: Core Research Skills (Week 1)

1. **proof-architect** - Proof structure and verification
2. **identification-theory** - Causal identification
3. **asymptotic-theory** - Inference and efficiency
4. **methods-paper-writer** - Manuscript structure

#### Tier 2: Innovation Skills (Week 2)

5. **cross-disciplinary-ideation** - Method innovation
6. **literature-gap-finder** - Research positioning
7. **method-transfer-engine** - Cross-field transfer

#### Tier 3: Computation Skills (Week 3)

8. **simulation-architect** - Simulation design
9. **algorithm-designer** - Algorithm documentation
10. **numerical-methods** - Numerical stability

#### Tier 4: Foundation Skills (Week 4)

11. **mathematical-foundations** - Background reference
12. **cs-theory-foundations** - Computational aspects
13. **physics-statistics-bridge** - Physical intuition
14. **computational-biology-methods** - Applications

---

## MCP Server Specification

### Tool Categories and Priorities

#### Tier 1: R Console (Critical - Week 1-2)

```typescript
// r_execute: Run R code and return output
interface RExecuteInput {
  code: string;
  timeout?: number;  // Default 60s
  capture_plots?: boolean;
}

interface RExecuteOutput {
  stdout: string;
  stderr: string;
  plots?: string[];  // Base64 or temp file paths
  objects?: Record<string, any>;  // If requested
}

// r_inspect: Inspect R object
interface RInspectInput {
  object: string;
  depth?: number;
}

// r_test: Run package tests
interface RTestInput {
  package_path?: string;  // Default: current directory
  filter?: string;        // Test name pattern
  reporter?: "summary" | "progress" | "check";
}

// r_check: R CMD check
interface RCheckInput {
  package_path?: string;
  cran?: boolean;        // --as-cran flag
  vignettes?: boolean;   // Build vignettes
}

// r_document: Generate documentation
interface RDocumentInput {
  package_path?: string;
}
```

#### Tier 2: Literature Management (Week 2-3)

```typescript
// crossref_lookup: Get paper metadata by DOI
interface CrossrefLookupInput {
  doi: string;
}

// arxiv_search: Search arXiv
interface ArxivSearchInput {
  query: string;
  max_results?: number;
  categories?: string[];  // e.g., ["stat.ME", "stat.TH"]
}

// bibtex_add: Add citation to .bib file
interface BibtexAddInput {
  entry: string;         // BibTeX string
  file?: string;         // Target .bib file
}

// bibtex_search: Search local .bib files
interface BibtexSearchInput {
  query: string;
  file?: string;
}
```

#### Tier 3: Proof and Notation (Week 3-4)

```typescript
// latex_compile: Compile LaTeX document
interface LatexCompileInput {
  file: string;
  output_dir?: string;
  bibtex?: boolean;
}

// notation_check: Verify notation consistency
interface NotationCheckInput {
  file: string;
  style?: "vanderweele" | "pearl" | "rubin";
}
```

#### Tier 4: Simulation (Week 4)

```typescript
// sim_design: Generate simulation design
interface SimDesignInput {
  estimand: string;
  parameters: {
    sample_sizes: number[];
    effect_sizes: number[];
    replications: number;
  };
  template?: "morris" | "custom";
}

// sim_run: Execute simulation (via R)
interface SimRunInput {
  script: string;
  parallel?: boolean;
  cores?: number;
}
```

---

## Agent Workflow Specifications

### Agent: Literature-Gap-Finder

```yaml
name: Literature Gap Finder
description: Systematically identify research gaps in a field

triggers:
  - "Find gaps in [field]"
  - "What's missing in [area] research"
  - "Position my contribution"

workflow:
  steps:
    - name: Gather context
      agent: Explore
      prompt: |
        Search for recent papers on {topic}.
        Identify: key methods, main authors, recent developments.

    - name: Build method-setting matrix
      skills:
        - literature-gap-finder
      prompt: |
        Create matrix of methods × settings.
        Mark: ✓ (well-developed), ? (partial), ✗ (gap)

    - name: Analyze assumption relaxations
      skills:
        - identification-theory
        - asymptotic-theory
      prompt: |
        For key methods, trace which assumptions have been relaxed.
        Identify unexplored relaxation combinations.

    - name: Synthesize gaps
      prompt: |
        Combine analyses to identify:
        1. Clear gaps (method ✗ setting)
        2. Partial gaps (need extension)
        3. Novel connections (cross-field)

output:
  format: markdown
  sections:
    - Summary of field state
    - Gap matrix
    - Prioritized research opportunities
    - Suggested first steps
```

### Agent: Cross-Discipline-Connect

```yaml
name: Cross-Disciplinary Connector
description: Find methods from other fields that could apply to statistical problem

triggers:
  - "What methods from [field] could help with [problem]"
  - "Cross-disciplinary ideas for [topic]"

workflow:
  steps:
    - name: Analyze target problem
      skills:
        - identification-theory
        - asymptotic-theory
      prompt: |
        Characterize the statistical problem:
        - Mathematical structure
        - Key challenges
        - Current limitations

    - name: Search related fields
      agent: Explore
      parallel: true
      prompts:
        - "How does optimal transport relate to {problem}?"
        - "What physics concepts connect to {problem}?"
        - "How do CS algorithms address similar structures?"
        - "What biology methods handle analogous challenges?"

    - name: Evaluate transfers
      skills:
        - method-transfer-engine
        - cross-disciplinary-ideation
      prompt: |
        For each potential connection:
        1. Assess structural alignment
        2. Translate assumptions
        3. Identify gaps in transfer
        4. Rate feasibility (high/medium/low)

output:
  format: markdown
  sections:
    - Problem characterization
    - Potential cross-field methods (ranked)
    - Transfer feasibility analysis
    - Recommended first exploration
```

### Agent: Proof-Verification

```yaml
name: Proof Verifier
description: Systematically verify mathematical proof correctness

triggers:
  - "Verify this proof"
  - "Check my derivation"
  - "Find errors in this argument"

workflow:
  steps:
    - name: Structure check
      skills:
        - proof-verifier
      prompt: |
        Level 1 check:
        - Is claim clearly stated?
        - All notation defined?
        - Logical flow apparent?

    - name: Step validation
      skills:
        - proof-architect
        - asymptotic-theory
      prompt: |
        Level 2 check:
        For each step:
        - What justifies this step?
        - Are external results correctly applied?
        - Do assumptions actually hold?

    - name: Edge case analysis
      prompt: |
        Level 3 check:
        - Boundary cases (n=1, p→∞)
        - Degenerate cases
        - What if assumptions barely hold?

    - name: Consistency check
      skills:
        - identification-theory
      prompt: |
        Level 4 check:
        - Does result match intuition?
        - Special cases recover known results?
        - Numerical verification possible?

output:
  format: markdown
  sections:
    - Verification summary (PASS/FAIL/NEEDS ATTENTION)
    - Issues found (by severity)
    - Suggestions for improvement
    - Confidence assessment
```

### Agent: Manuscript-Section-Writer

```yaml
name: Manuscript Section Writer
description: Draft manuscript sections with proper notation and structure

triggers:
  - "Write methods section for [paper]"
  - "Draft simulation study section"
  - "Help with introduction for [topic]"

workflow:
  steps:
    - name: Gather context
      agent: Explore
      prompt: |
        Read relevant code/implementation for this paper.
        Identify: key algorithms, main results, notation used.

    - name: Check existing conventions
      tools:
        - Read
      prompt: |
        Read any existing manuscript sections.
        Extract: notation conventions, citation style, section structure.

    - name: Draft section
      skills:
        - methods-paper-writer
        - mathematical-exposition
      prompt: |
        Write the {section} section following:
        - Journal conventions ({journal})
        - VanderWeele notation for causal/mediation
        - Proper LaTeX formatting

    - name: Verify notation
      skills:
        - proof-verifier
      tools:
        - notation_check (MCP)
      prompt: |
        Check notation consistency with:
        - Rest of manuscript
        - Standard conventions
        - Previous sections

output:
  format: latex
  includes:
    - Section text
    - Required packages
    - BibTeX keys used
    - Notation glossary (if new symbols)
```

---

## Integration with Existing Setup

### Your Current Skills (Keep)

| Skill | Status | Action |
|-------|--------|--------|
| `r-package-development.md` | Active | Keep, enhance with r-package-architect |
| `emacs-r-devkit.md` | Active | Keep |
| `statistical-research-workflow.md` | Active | Keep as orchestrator |
| `manuscript-writing-guide` (symlink) | Active | Enhance with methods-paper-writer |
| `statistical-simulation-guide` (symlink) | Active | Enhance with simulation-architect |
| `r-simulation-config` (symlink) | Active | Merge into simulation-architect |

### Migration Path

```bash
# Phase 1: Add new skills alongside existing
~/.claude/skills/
├── r-package-development.md          # Existing
├── statistical-research-workflow.md  # Existing
├── proof-architect/                   # NEW (directory format)
│   └── SKILL.md
├── identification-theory/             # NEW
│   └── SKILL.md
└── ...

# Phase 2: Gradually convert .md files to directory format
# Phase 3: Update symlinks to point to extension repo
```

---

## Implementation Phases

### Phase 1: Core Skills + MCP Foundation ✅ COMPLETE

**Skills created:**
1. `~/.claude/skills/proof-architect/SKILL.md` ✅
2. `~/.claude/skills/identification-theory/SKILL.md` ✅
3. `~/.claude/skills/asymptotic-theory/SKILL.md` ✅
4. `~/.claude/skills/methods-paper-writer/SKILL.md` ✅

**MCP development:**
1. R Console tools (r_execute, r_inspect, r_test, r_check) ✅
2. Bun-based MCP server at `~/projects/code/claude-statistical-research/mcp-server/` ✅

**Deliverable:** Working R Console MCP + 4 core skills ✅

### Phase 2: Innovation Skills + Literature MCP ✅ COMPLETE

**Skills created:**
5. `~/.claude/skills/cross-disciplinary-ideation/SKILL.md` ✅
6. `~/.claude/skills/literature-gap-finder/SKILL.md` ✅
7. `~/.claude/skills/method-transfer-engine/SKILL.md` ✅

**MCP development:**
1. Literature tools ✅
   - `arxiv_search` - Search arXiv for papers
   - `crossref_lookup` - DOI metadata lookup
   - `bibtex_search` - Search local .bib files
   - `bibtex_add` - Add entries to .bib
   - `lit_note_create` - Create Obsidian literature notes

**Integrations configured:**
- Zotero Better BibTeX at `~/Zotero/`
- BibTeX at `~/Library/CloudStorage/GoogleDrive-dtofighi@gmail.com/My Drive/research/product of three/references.bib`
- Obsidian vault at `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/`

**Deliverable:** Literature management + innovation skills ✅

### Phase 3: MCP Server Finalization ✅ COMPLETE

**MCP Server:** `~/projects/code/claude-statistical-research/mcp-server/`
- Runtime: Bun (user preference over Node.js)
- Total tools: 9 (4 R Console + 5 Literature)
- Configured in `~/.claude/settings.json`

**Deliverable:** Complete MCP server with R and literature tools ✅

### Phase 4: Slash Commands + Agent Workflows ✅ COMPLETE

**Slash commands created in `~/.claude/commands/`:**
1. `/lit-gap` - Literature gap finder workflow
2. `/proof-check` - Proof verification workflow
3. `/ecosystem-health` - MediationVerse health check
4. `/manuscript-section` - Manuscript section writer
5. `/sim-design` - Simulation study design
6. `/cite` - Citation lookup
7. `/rpkg-check` - R package quick check

**Deliverable:** 7 agent workflow slash commands ✅

### Phase 5: Foundation Skills + Enhancements (Future)

**Skills to create:**
- `simulation-architect/SKILL.md`
- `algorithm-designer/SKILL.md`
- `numerical-methods/SKILL.md`
- `mathematical-foundations/SKILL.md`
- `cs-theory-foundations/SKILL.md`
- `physics-statistics-bridge/SKILL.md`
- `computational-biology-methods/SKILL.md`

**Future MCP tools:**
- `latex_compile` - LaTeX compilation
- `notation_check` - Notation consistency
- `sim_design` / `sim_run` / `sim_analyze` - Simulation tools

### Phase 6: Polish and Documentation (Future)

1. Comprehensive testing
2. Documentation
3. Install script refinement
4. Public release preparation (optional)

---

## Success Metrics

### Skill Effectiveness
- [ ] Skills auto-activate appropriately (based on task)
- [ ] Notation consistency across manuscripts
- [ ] Proof verification catches known error patterns

### MCP Functionality
- [ ] R code executes and returns results
- [ ] Citations can be looked up and added
- [ ] LaTeX compiles correctly

### Agent Performance
- [ ] Literature gaps identified match expert assessment
- [ ] Cross-discipline suggestions are relevant
- [ ] Manuscript sections follow journal conventions

### Overall Impact
- [ ] Reduced time from idea to implementation
- [ ] Fewer notation inconsistencies in manuscripts
- [ ] Better identification of research opportunities
- [ ] Smoother package ecosystem management

---

## Open Questions

1. **Skill organization**: Use categorical prefixes or purely semantic names?
2. **MCP session management**: Persistent R session vs. spawn per call?
3. **Agent orchestration**: How to chain agents for complex workflows?
4. **Symbolic computation**: Integrate Wolfram Alpha/Mathematica?
5. **Collaboration**: Multi-user skill sharing?

---

## References

- [MCP Specification](https://modelcontextprotocol.io/)
- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- VanderWeele (2015) - Notation standards
- Morris et al. (2019) - Simulation guidelines
- Your interdisciplinary skills framework (provided 2025-12-08)

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v1 | 2025-12-08 | Initial extension plan |
| v2 | 2025-12-08 | Added interdisciplinary skills framework, expanded agents |
| v2.1 | 2025-12-08 | Phase 1-4 implementation complete |

---

## Implementation Summary

### Completed Components

**7 Skills (in ~/.claude/skills/):**
- `proof-architect/` - Proof structure, verification checklists, templates
- `identification-theory/` - DAG/potential outcomes, mediation identification
- `asymptotic-theory/` - M-estimation, influence functions, semiparametric efficiency
- `methods-paper-writer/` - JASA/Biometrika structure, notation standards
- `cross-disciplinary-ideation/` - Field connection maps, ideation prompts
- `literature-gap-finder/` - Method×Setting matrices, gap verification
- `method-transfer-engine/` - 6-phase transfer protocol

**9 MCP Tools (in ~/projects/code/claude-statistical-research/mcp-server/):**
- R Console: `r_execute`, `r_inspect`, `r_test`, `r_check`
- Literature: `arxiv_search`, `crossref_lookup`, `bibtex_search`, `bibtex_add`, `lit_note_create`

**7 Slash Commands (in ~/.claude/commands/):**
- `/lit-gap`, `/proof-check`, `/ecosystem-health`
- `/manuscript-section`, `/sim-design`, `/cite`, `/rpkg-check`

### Testing

To test the MCP server in a new Claude session:
```bash
# Verify MCP tools are available
# In Claude CLI, the mcp tools should appear as mcp__statistical-research__*

# Test R execution
mcp__statistical-research__r_execute code="1+1"

# Test literature search
mcp__statistical-research__arxiv_search query="causal mediation" max_results=5
```

---

**Status**: Phase 1-4 Complete ✅
**Next Action**: Test MCP tools, then proceed to Phase 5 (foundation skills)
**Author**: Claude + DT
