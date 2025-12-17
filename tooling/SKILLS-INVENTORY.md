# Skills Inventory

**Last Updated:** December 12, 2025  
**Total Skills:** 37 (30 directories + 7 standalone files)  
**Location:** `/Users/dt/.claude/skills/`

---

## TL;DR

- ✅ **30+ skills** on Mac at `~/.claude/skills/`
- ✅ **5 skills** currently loaded in Claude Desktop
- 📋 **ALL 7** skills from original plan implemented
- 🎉 **23+ bonus** skills beyond original scope

---

## Skills Currently Active in Claude

These 5 skills are loaded in `/mnt/skills/user/` and available in current project:

| Skill | Size | Purpose |
|-------|------|---------|
| **causal-inference-research** | 133K | Research lifecycle for causal inference methodology papers |
| **statistical-pedagogy-framework** | 96K | Evidence-based teaching framework with cognitive principles |
| **r-simulation-config** | 16K | Preferences for R simulation studies configuration |
| **statistical-simulation-guide** | 9.5K | Best practices for statistical simulations |
| **manuscript-writing-guide** | 13K | Standards for product-of-three distributions manuscript |

---

## Complete Skills on Mac

### ✅ Core Research Skills (From Original Plan)

These 7 skills were specified in CLAUDE-EXTENSION-PLAN-v2.md:

| # | Skill | Status | Description |
|---|-------|--------|-------------|
| 1 | **proof-architect** | ✅ Complete | Structured methodology for mathematical proofs |
| 2 | **asymptotic-theory** | ✅ Complete | M-estimation, influence functions, semiparametric efficiency |
| 3 | **identification-theory** | ✅ Complete | DAG/potential outcomes, mediation identification |
| 4 | **methods-paper-writer** | ✅ Complete | JASA/Biometrika structure, notation standards |
| 5 | **cross-disciplinary-ideation** | ✅ Complete | Field connection maps, ideation prompts |
| 6 | **literature-gap-finder** | ✅ Complete | Method×Setting matrices, gap verification |
| 7 | **method-transfer-engine** | ✅ Complete | 6-phase transfer protocol |

### 🆕 Extended Research Skills (Beyond Plan)

Additional skills developed after initial planning:

| # | Skill | Description |
|---|-------|-------------|
| 8 | **algorithm-designer** | Algorithm development and optimization |
| 9 | **computational-inference** | Computational methods for statistical inference |
| 10 | **mathematical-foundations** | Core mathematical concepts and frameworks |
| 11 | **mediation-meta-analyst** | Meta-analysis for mediation studies |
| 12 | **methods-communicator** | Communicating statistical methods effectively |
| 13 | **numerical-methods** | Numerical algorithms and computation |
| 14 | **publication-strategist** | Publication planning and venue selection |
| 15 | **sensitivity-analyst** | Sensitivity analysis frameworks and methods |
| 16 | **simulation-architect** | Simulation study design and implementation |
| 17 | **statistical-software-qa** | Quality assurance for statistical software |

### 📊 Teaching & Pedagogy Skills

| # | Skill | Description |
|---|-------|-------------|
| 18 | **teaching-workflow** | ADHD-optimized teaching material creation |
| 19 | **statistics-exam-generator** | Generate statistics exams with LaTeX/Examark |

### 📝 Documentation & Writing Skills

| # | Skill | Description |
|---|-------|-------------|
| 20 | **manuscript-writing-guide** | Manuscript writing standards and conventions |
| 21 | **research-writing-meta** | Meta-skill for research writing workflows |

### 🔧 Development & Tools Skills

| # | Skill | Description |
|---|-------|-------------|
| 22 | **r-simulation-config** | R simulation study configuration patterns |
| 23 | **statistical-simulation-guide** | Statistical simulation best practices |
| 24 | **mac-webapp-cleanup** | Utility for cleaning Mac web app residuals |

### 📄 Standalone Guide Files (7 files)

These are markdown reference guides, not full skill packages:

1. **claude-desktop-project-setup-guide.md** - Claude Desktop project organization
2. **claude-desktop-exam-generation.md** - Exam generation workflows
3. **claude-desktop-examark-formatting.md** - Examark format specifications
4. **data-wise-documentation.md** - Data-Wise organization documentation standards
5. **doc-management.md** - Document management workflows
6. **emacs-r-devkit.md** - Emacs configuration for R development
7. **examark-example-exam.md** - Example exam in Examark format
8. **examark-quick-reference.md** - Quick reference for Examark syntax
9. **quarto-examark-generator.md** - Quarto integration with Examark
10. **r-package-development.md** - R package development guide
11. **research-writing-workflow.md.backup** - Backup of research writing workflow
12. **statistical-research-workflow.md** - Statistical research workflow guide
13. **zsh-environment.md** - ZSH shell environment documentation

---

## How Skills Work

### Architecture

```
Mac: ~/.claude/skills/ (30+ skills)
     ↓ (manual upload to project)
Claude: /mnt/skills/user/ (5 skills active)
```

### Adding Skills to Claude

**Method 1: Through Claude Desktop UI**
1. Open Claude Desktop
2. Navigate to Project Settings → Skills
3. Click "Add Skill" or "Upload Skill"
4. Browse to `~/.claude/skills/[skill-name]/`
5. Skill becomes available in `/mnt/skills/user/`

**Method 2: Skill Package Files**
- Some skills have `.skill` packaged files
- Upload .skill file directly through UI
- Skill auto-extracts and loads

**Method 3: Via CLI** (if available)
```bash
claude skill add ~/.claude/skills/proof-architect
claude skill list
claude skill enable proof-architect
```

### Skill Structure

Each skill directory contains:
```
skill-name/
├── SKILL.md          # Main skill definition
├── templates/        # Optional templates
├── references/       # Optional reference docs
├── scripts/          # Optional utility scripts
└── .backups/         # Optional backup directory
```

---

## Recommended Next Steps

### Option A: Load All Core Research Skills [High Priority]
**Action:** Upload all 7 original plan skills to Claude  
**Time:** 15 min  
**Skills to add:**
- proof-architect
- asymptotic-theory
- identification-theory
- methods-paper-writer
- cross-disciplinary-ideation
- literature-gap-finder
- method-transfer-engine

### Option B: Load Extended Research Skills [Medium Priority]
**Action:** Add bonus research skills  
**Time:** 20 min  
**Skills to add:**
- algorithm-designer
- computational-inference
- mathematical-foundations
- mediation-meta-analyst
- sensitivity-analyst
- simulation-architect

### Option C: Load All Skills [Comprehensive Mode]
**Action:** Upload all 30+ skills  
**Time:** 30-45 min  
**Result:** Complete capabilities available

---

## Maintenance

### Adding New Skills
1. Create skill directory: `mkdir ~/.claude/skills/new-skill`
2. Create SKILL.md with skill definition
3. Add templates/references as needed
4. Upload to Claude Desktop project

### Updating Existing Skills
1. Edit skill files on Mac at `~/.claude/skills/`
2. Re-upload to Claude if needed (check if auto-sync)
3. Test skill in Claude chat

### Removing Skills
1. Delete from `~/.claude/skills/` (Mac)
2. Remove from Claude project settings (if loaded)
3. Skill no longer available in new chats

---

## File Locations Reference

| What | Where |
|------|-------|
| **Skills source** | `/Users/dt/.claude/skills/` (Mac) |
| **Active skills** | `/mnt/skills/user/` (Claude) |
| **Public skills** | `/mnt/skills/public/` (Claude built-in) |
| **Example skills** | `/mnt/skills/examples/` (Claude examples) |
| **This inventory** | `/Users/dt/projects/research/mediation-planning/SKILLS-INVENTORY.md` |

---

**Questions? Check:**
- CLAUDE-EXTENSION-PLAN-v2.md for original plan
- CLAUDE-SKILLS-GUIDE.md in project knowledge
- Claude Desktop Settings → Skills tab
