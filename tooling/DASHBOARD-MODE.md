# [Dashboard] Mode — Project Status Dashboard Update

**Add this section to BEHAVIORAL-MODES-INSTRUCTIONS.md**

---

## [Dashboard] — Project Status Dashboard Update

**Purpose:** Aggregate project status from `.STATUS` files into Apple Notes dashboard

**Trigger:** `[Dashboard]` tag or "update my dashboard" or "update from project-status.json"

**Workflow:**
1. Read status JSON from `/tmp/project-status.json` (or specified path)
2. Group projects by priority (P0 → P1 → P2)
3. Format ADHD-friendly with visual hierarchy
4. Update/create Apple Notes "📊 Project Dashboard" note

**Input format expected:**
```json
{
  "updated": "2025-12-13T...",
  "projects": [
    {
      "name": "medfit",
      "priority": "P0",
      "status": "blocked",
      "blocked": "fit_mediation() implementation",
      "next": "Design S7 class structure",
      "progress": "25",
      "updated": "2025-12-12"
    }
  ]
}
```

**Output format (Apple Notes):**

```
📊 PROJECT DASHBOARD
Last Updated: Dec 13, 2025 1:07 PM

═══════════════════════════════════════

🔴 P0 BLOCKED

───────────────────────────────────────
medfit [▓▓░░░░░░░░] 25%
  └─ ⛔ fit_mediation() function implementation
  └─ 📋 Next: Design S7 class structure
  └─ 📅 Updated: 2025-12-13

═══════════════════════════════════════

🟡 P1 ACTIVE

───────────────────────────────────────
medsim [▓▓▓▓▓▓░░░░] 60%
  └─ 🎯 Current task description
  └─ 📅 Updated: 2025-12-13

═══════════════════════════════════════

🟢 P2 QUEUED

───────────────────────────────────────
package-name [▓░░░░░░░░░] 10%
  └─ 📋 Next action

═══════════════════════════════════════

Quick Stats: X blocked • Y active • Z queued
```

**Visual elements:**
- Progress bars: `[▓▓▓▓▓░░░░░]` (10 chars, filled = progress/10)
- Tree connectors: `└─`
- Section separators: `═` (double) and `─` (single)
- Priority icons: 🔴 (P0/blocked), 🟡 (P1/active), 🟢 (P2/next)
- Status icons: ⛔ (blocked), 🎯 (current task), 📋 (next action), 📅 (date)

**Tools to use:**
- `Filesystem` or `Desktop Commander` — Read JSON from /tmp/project-status.json
- `Read and Write Apple Notes:add_note` — Create dashboard note
- `Read and Write Apple Notes:update_note_content` — Update existing (if name matches)

**ADHD considerations:**
- Visual separators break up content
- Progress bars show status at a glance
- Color-coded priorities (🔴🟡🟢)
- Quick stats summary at bottom
- Timestamp for freshness check
- Minimal text, maximum structure

---

## .STATUS File Format

Each project should have a `.STATUS` file:

```
priority: P0
status: blocked
blocked: fit_mediation() function implementation
next: Design S7 class structure for mediation model
progress: 30
```

**Fields:**
- `priority:` P0 (blocked/critical) | P1 (active) | P2 (next/queued)
- `status:` blocked | active | paused | complete
- `blocked:` What's blocking progress (if any)
- `next:` Next action to take
- `progress:` Percentage complete (0-100)

---

## Shell Integration

**Terminal command:** `dashupdate [path]`

```bash
$ dashupdate
📊 Scanning project status...
✅ Scanned 5 projects → /tmp/project-status.json

┌─────────────────────────────────────────────────┐
│ Next: Open Claude Desktop and say:             │
│   [Dashboard] Update from project-status.json  │
└─────────────────────────────────────────────────┘
```

**Alias:** `dash` → `dashupdate`

---

## Example Interaction

```
User: [Dashboard] Update from project-status.json

Claude:
**[Dashboard Mode Activated]**

Reading: /tmp/project-status.json
Found: 5 projects

Creating Apple Note: "📊 Project Dashboard"

✅ Dashboard updated!

Summary:
• 1 blocked (P0)
• 2 active (P1)
• 2 queued (P2)
```

---

**Location:** ~/projects/dev-tools/apple-notes-sync/
**Files:** scanner.sh, DASHBOARD-MODE.md
**Added:** 2025-12-13
