# Shell Execution & File Management Protocol

**Version:** 1.0  
**Updated:** December 12, 2025  
**Scope:** MediationVerse multi-package ecosystem

---

## Pre-Action Statement

Before performing any action, clearly state:

| Field | Description |
|-------|-------------|
| **What** | Specific action in plain language |
| **Why** | Purpose and expected benefit |
| **Affected** | Files/folders with paths |
| **Risk** | 🟢 Read | 🟡 Modify | 🔴 Destructive |
| **Est.** | Time estimate (~5s, ~30s, ~2min) |

For ecosystem changes, also note:
- **Packages:** Which R packages affected
- **Downstream:** Any dependent packages impacted

---

## Confirmation Protocol

| Input | Action |
|-------|--------|
| `1` | Proceed with this single action |
| `2` | Proceed and show full results |
| `3` | Accept all similar actions this session |
| `0` | Skip/abort this action |

**Auto-execute (no confirmation):**
- 🟢 Read-only operations (viewing, listing, searching)
- Status checks (`git status`, `medcheck status`)

**Always confirm:**
- 🔴 Destructive operations (delete, overwrite, force)
- Cross-package modifications
- Git pushes to remote

---

## Execution Protocol

For any script or command:

1. **Show** — Display command in code block
2. **Execute** — Run with appropriate timeout
3. **Validate** — Check exit code and output
4. **On failure:**
   - Diagnose root cause
   - Propose fix
   - Re-run automatically
   - Summarize what changed

---

## Progress & Session Tracking

**Multi-step tasks:**
- Show progress: `[Step 2/5]` or `[2/5 ✓]`
- After 3+ completed actions: brief summary
- Mark stopping points: `⏸️ Good place to pause`

**Session state:**
- Track files modified (for end-of-session summary)
- Track decisions made (can reference later)
- Track git state changes across repos

**Ecosystem awareness:**
- Run `medcheck status` before major work sessions
- Note upstream/downstream impacts
- Flag when API-CONTRACTS.md may need updates

---

## Batch Operations

When multiple similar operations detected:
- Offer batch mode: "Apply same operation to all N files?"
- `3` accepts batch for this type
- Group git operations where safe (add + commit)

---

## Rollback & Safety

**Before destructive operations:**
- Note current git SHA or create backup
- State: "To undo: [specific command]"

**After failures:**
- Restore to known good state before continuing
- Summarize what was rolled back

---

## Integration Points

| Tool | When to Use |
|------|-------------|
| `medplan` | Planning doc navigation |
| `medplan status` | Quick project overview |
| `medcheck` | Before/after significant changes |
| `medcheck deps` | Verify dependency consistency |
| `medcheck git` | Cross-repo git status |

---

## Quick Reference

```
Confirm: 1=proceed | 2=show | 3=batch | 0=skip
Risk:    🟢=read  | 🟡=modify | 🔴=destructive
Progress: [Step X/Y] or [X/Y ✓]
```
