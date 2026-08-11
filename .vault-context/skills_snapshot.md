# Cached Skills Snapshot

## memory_compaction

# SKILL: Memory Compaction
> **Trigger:** User says "compact memory", "compress logs", "clean up context" — or automatically when .system/logs/sessions/ exceeds 30 files
> **Version:** 1.0 | **Last Updated:** 2026-07-30

---

## Description
Prevents the "Dumb Zone" — AI degradation from context overload. Summarizes old session logs into permanent Wiki notes, archives raw logs, and keeps active context lean. See [[Progressive Context Loading]] for theory.

---

## Workflow

### STEP 1 — Assess Current State
```
Count files in .system/logs/sessions/    → if <10, skip compaction
Count files in .system/logs/activity/    → if <14 days, skip compaction
Measure total size of .system/logs/      → flag if >5MB
```

### STEP 2 — Compact Session Logs
```
Read all session logs older than 30 days
Distill into: 06_Wiki/Session Summary YYYY-MM.md
  Include:
    - Key decisions made that month
    - Projects worked on
    - Preferences learned
    - Skills invoked
    - Files created
Move raw logs to 04_Archive/logs/sessions/YYYY-MM/
```

### STEP 3 — Compact Activity Logs
```
Keep last 7 days of activity logs in .system/logs/activity/
Archive everything older to 04_Archive/logs/activity/YYYY-MM/
```

### STEP 4 — Update memory.md
```
Review all session summaries
Extract any preference/correction not yet in memory.md
Add new entries to memory.md
```

### STEP 5 — Update CONTEXT.md
```
Check if CONTEXT.md still reflects current reality
Update any stale information
Bump "Last Updated" date
```

### STEP 6 — Report
```
Tell user:
- X session logs compacted into: 06_Wiki/Session Summary YYYY-MM.md
- X MB freed from .system/logs/
- X new memory entries added
- Context is now lean and ready
```

---

## Rules
- Never delete raw logs — move to 04_Archive/ only
- Always create a Wiki summary BEFORE archiving
- Do not compact logs from the last 7 days
- Run OS Audit after compaction to verify integrity

---

## Output
- `06_Wiki/Session Summary YYYY-MM.md` — permanent distilled summary
- `04_Archive/logs/sessions/YYYY-MM/` — archived raw logs
- `memory.md` — updated with new entries
- User report in chat


## new_project_sop

# 🛠️ SKILL: new_project_sop
> **Trigger:** User says "new project", "start a project", "create project", "I want to build X"
> **Version:** 1.0 | **Last Updated:** 2026-07-30

---

## What This Skill Does
When the user wants to start any new project, this SOP creates:
1. A **vault project folder** in `01_Projects/` with all planning files
2. A **code folder** at `C:\Dev\<ProjectName>\` with proper structure
3. A **bridge link** so Obsidian always knows where the code lives and its status

---

## STEP 1 — Collect Information (Ask User If Missing)

Before doing anything, confirm:
- [ ] `PROJECT_NAME` — short slug (e.g., `MySaaS`, `PortfolioSite`)
- [ ] `PROJECT_DESC` — one sentence description
- [ ] `PROJECT_TYPE` — SaaS / Tool / Script / Content / Research / Other
- [ ] `TECH_STACK` — languages/frameworks (e.g., Next.js, Python, etc.)
- [ ] `DEADLINE` — target date or "none"
- [ ] `GITHUB_REPO` — URL if exists, or "create new"
- [ ] `CODE_PATH` — where code lives (default: `C:\Dev\<ProjectName>\`)

---

## STEP 2 — Create Vault Project Folder

Create the following structure in `01_Projects/<PROJECT_NAME>/`:

```
01_Projects/
└── <PROJECT_NAME>/
    ├── overview.md          ← master project note (frontmatter + description)
    ├── roadmap.md           ← milestones, phases, deadlines
    ├── architecture.md      ← tech decisions, system design
    ├── decisions_log.md     ← why we chose X over Y (ADR-style)
    ├── meeting_notes.md     ← all meetings/calls logged here
    ├── blockers.md          ← current blockers and how to solve them
    └── resources.md         ← links, references, research for this project
```

### overview.md Frontmatter (REQUIRED — Dataview reads this):
```yaml
---
title: "<PROJECT_NAME>"
status: "active"          # active | paused | completed | archived
type: "<PROJECT_TYPE>"
stack: ["<TECH1>", "<TECH2>"]
started: "<YYYY-MM-DD>"
deadline: "<YYYY-MM-DD or none>"
github: "<GITHUB_URL or TBD>"
code_path: "<C:\\Dev\\PROJECT_NAME>"
priority: "high"          # high | medium | low
tags: [project, <type>]
---
```

---

## STEP 3 — Create Code Folder

Create `C:\Dev\<PROJECT_NAME>\` with:

```
C:\Dev\<PROJECT_NAME>\
├── .git\                  (git init)
├── src\
├── docs\
├── tests\
├── .gitignore
└── README.md              ← links back to vault note
```

**README.md must contain:**
```markdown
# <PROJECT_NAME>
<PROJECT_DESC>

## 📋 Project Brain (Obsidian Vault)
Planning, roadmap, and decisions live in the Obsidian vault:
`G:\My Drive\Main Vault\01_Projects\<PROJECT_NAME>\`

## Status
See [overview.md] for current status and milestones.
```

---

## STEP 3.5 — Setup Vault Bridge (Automatic — Never Skip)

**Every project gets a permanent two-way bridge to the Main Vault.** Run the `vault_bridge` skill:

### A. Create Bridge Rules
```
<CODE_PATH>/
├── .agents/
│   ├── AGENTS.md                         ← Auto-sync rules (copy template, fill project details)
│   └── skills/
│       └── vault_bridge/
│           └── SKILL.md                  ← Portable sync instructions
```

### B. Create Knowledge Cache
```
<CODE_PATH>/
├── .vault-context/
│   ├── vault_manifest.json               ← Registry: vault path, project name, tech stack
│   ├── relevant_knowledge.md             ← Extract from 06_Wiki/ + 03_Resources/ matching tech stack
│   ├── skills_snapshot.md                ← Copy verification_loop + token_optimizer + memory_compaction
│   ├── project_notes.md                  ← Snapshot of 01_Projects/<PROJECT_NAME>/ docs
│   └── memory_snapshot.md                ← Copy of .system/memory.md
```

### C. Extract Relevant Knowledge
```
1. Read project tech stack from overview.md frontmatter
2. Scan 06_Wiki/ — match tags against tech stack → include matching notes
3. Scan 03_Resources/ — include relevant courses, references
4. Scan 01_Projects/ — extract learnings from projects with shared tech stack
5. Copy .system/memory.md → memory_snapshot.md
6. Copy relevant skills → skills_snapshot.md
```

### D. Update .gitignore
Add `.vault-context/` to project `.gitignore` (contains local paths, auto-regenerated).

### E. Full Reference
See `.system/skills/vault_bridge.md` for complete setup protocol.

---

## STEP 3.6 — Assign AI Agent Team (Automatic — Never Skip)

Every new project must have a dedicated virtual AI team assigned to it. This structure allows the main agent to delegate tasks effectively or adopt specific personas when tackling different problems.

### Create Team Roster
Inside the project's `.agents/` folder, create a `team_roster.md` file defining these default roles (customize based on project type):

```markdown
# 🤖 Project AI Team Roster
> **Project:** <PROJECT_NAME>

## 🎬 Director (Main Agent)
- **Role:** Orchestrates the entire project, interfaces with the user, makes high-level architectural decisions, and delegates tasks to sub-agents.
- **Focus:** Strategy, timeline, vault sync, and user alignment.

## 📋 Project Manager
- **Role:** Maintains the roadmap, tracks blockers, and ensures tasks are completed in the correct order.
- **Focus:** `roadmap.md`, `blockers.md`, and task prioritization.

## 💻 Lead Developer (Coder)
- **Role:** Writes the actual source code, implements features, and runs tests.
- **Focus:** Execution, algorithms, debugging, and performance.

## 🎨 UI/UX Designer (if applicable)
- **Role:** Designs the user interface, ensures pixel-perfect CSS, and focuses on accessibility and user flows.
- **Focus:** Aesthetics, styling, and user experience.

## 🧪 QA Tester
- **Role:** Runs the verification loops, checks for edge cases, and tries to break the code.
- **Focus:** Reliability, security, and edge-case handling.
```

### Team Integration
Update the project's `AGENTS.md` to reference the `team_roster.md` so the main agent knows it can invoke these roles or subagents for heavy tasks.

---

## STEP 4 — Update the Projects Dashboard

Append to `01_Projects/README.md` (the dashboard auto-updates via Dataview).
No manual update needed — Dataview queries pick it up automatically.

---

## STEP 5 — Log in memory.md

Append to `.system/memory.md`:
```
| <DATE> | New Project | Created <PROJECT_NAME> — vault: 01_Projects/<PROJECT_NAME>/ | code: C:\Dev\<PROJECT_NAME>\ |
```

---

## STEP 6 — Report to User

Tell the user:
1. Exact vault path of the project folder
2. Exact code path
3. GitHub URL (or instruction to create repo)
4. Next immediate action to take

---

## Project Lifecycle States

```
active    → currently being worked on
paused    → on hold temporarily
completed → shipped / done
archived  → abandoned or obsolete
```

To change state: update `status:` in `overview.md` frontmatter.
Dataview dashboard updates automatically.

To archive: move `01_Projects/<NAME>/` → `04_Archive/<NAME>/`


## os_audit

# SKILL: OS Audit
> **Trigger:** User says "run audit", "health check", "check the vault", "OS audit"
> **Version:** 1.0 | **Last Updated:** 2026-07-30

---

## Description
Performs a full health check of the Second Brain vault. Checks for routing integrity, bloat, context rot, broken links, and stale priorities. Reports findings and fixes what it can automatically.

---

## Workflow

### STEP 1 — Routing Integrity Check
```
Scan 00_Inbox/       → flag items older than 7 days (should have been routed)
Scan 01_Projects/    → check each project has: overview.md, roadmap.md, status field
Scan 06_Wiki/        → check each note has: frontmatter, sources, linked_to fields
Scan 05_Raw/         → flag any files NOT in /processed (unprocessed sources)
```

### STEP 2 — Bloat Check
```
Find notes with no outgoing links    → flag as orphaned
Find notes with no incoming links    → flag as orphaned
Find duplicate topics in 06_Wiki/    → suggest merging
Find files in wrong folders          → suggest moving
Check .system/logs/ size             → flag if >50 files or >5MB
```

### STEP 3 — Context Rot Check
```
Read priorities.md   → are goals older than 90 days? Flag for update
Read memory.md       → are any entries older than 6 months still relevant?
Read about_me.md     → is it still empty? (flag: onboarding incomplete)
Check agents.md      → is version current? Any outdated rules?
Check CONTEXT.md     → does it reflect current reality?
```

### STEP 4 — Project Health Check
```
For each project in 01_Projects/:
  → Does overview.md have valid status field?
  → Is there a roadmap.md?
  → Any task overdue? (check deadline field)
  → Any blocker older than 14 days?
  → Is the code_path still valid?
```

### STEP 5 — Generate Report
Write audit report to `.system/logs/audit_YYYY-MM-DD.md`:
```markdown
# OS Audit — YYYY-MM-DD
## 🔴 Critical Issues (fix now)
## 🟡 Warnings (fix soon)
## 🟢 Healthy
## 📋 Recommended Actions
```

### STEP 6 — Auto-Fix What's Safe
```
✅ Add missing frontmatter to Wiki notes
✅ Move misrouted files to correct folder
✅ Flag stale items in inbox
❌ Do NOT delete anything
❌ Do NOT move project folders without confirmation
```

---

## Rules
- Never delete anything — only flag and archive
- Report EVERYTHING found — no hiding issues
- Auto-fix only low-risk items (frontmatter, tags)
- High-risk changes require user confirmation

---

## Output
- `/.system/logs/audit_YYYY-MM-DD.md` — full audit report
- Summary reported to user in chat
- memory.md updated with audit date


## token_optimizer

# 🛠️ SKILL: token_optimizer
> **Trigger:** Active on EVERY task. User can explicitly trigger via `/tokens` or "optimize tokens"
> **Version:** 1.0 | **Last Updated:** 2026-07-30

---

## 🎯 Purpose
Protects the user's AI token budget by preventing redundant context loading, avoiding unnecessary AI generation for mechanical tasks, and using local scripts/CLI tools whenever possible.

---

## 🧭 Token Optimization Routing Matrix

Before executing ANY request, the AI must evaluate the task type:

```
                  ┌───────────────────────────────┐
                  │      Incoming User Request    │
                  └──────────────┬────────────────┘
                                 │
                 Is it Mechanical / Repetitive / File Ops?
                                 │
                   ┌─────────────┴─────────────┐
                   │                           │
                YES (Fast Route)            NO (Deep Thinking)
                   │                           │
        • Use local PowerShell/CLI   • Use full reasoning
        • Read index files only      • Progressive context load
        • Concise output (<150 words)• Generate detailed solution
        • Execute scripts directly   • Update memory & context
```

---

## 📋 Optimization Rules

### 1. Mechanical Tasks → Script/CLI First (Zero AI Generation Overhead)
For bulk file renames, folder checks, sorting, cleaning, or running tests:
- **Rule:** Do NOT read full files into AI context or write long code explanations.
- **Action:** Run a targeted PowerShell command or script locally. Report only the result summary.

### 2. Context Minimization (Progressive Loading)
- **Rule:** Never load entire folders or historical transcripts unless explicitly requested.
- **Action:** Read `CONTEXT.md` (500 tokens) or `01_Projects/README.md` index first. Only read individual files if needed for that specific task.

### 3. Response Length Calibration
- **Short Status Updates / Checks:** Keep responses under 100–150 words.
- **Complex Architecture / Strategy:** Use full structured breakdown.

### 4. Memory Compaction Trigger
- **Rule:** If the conversation history exceeds ~30 tool calls or context gets heavy, trigger `memory_compaction` skill automatically to compress logs and flush bloat.

---

## 🚨 Hard Restrictions
1. **No Output Bloat:** Do NOT re-state whole files in chat responses when editing files. Use tool diffs.
2. **No Redundant Reading:** Do NOT view the same file twice in one session unless modified.
3. **No Unused Skill Loading:** Only load skills relevant to the active prompt.


## vault_bridge

# 🌉 SKILL: vault_bridge
> **Trigger:** Runs automatically on every project session open. Also runs when user says "sync project", "bridge vault", "connect project". Runs as sub-step of `new_project_sop` skill.
> **Version:** 1.0 | **Last Updated:** 2026-07-31

---

## 🎯 Purpose

Maintains a **permanent two-way connection** between any dev project workspace and the Main Vault (Obsidian Second Brain). Ensures:
1. The agent always has access to vault knowledge (courses, wiki, skills, memory) even from a project workspace
2. Every coding session automatically feeds data back to the vault (session logs, decisions, learnings)
3. Works whether Main Vault is the active workspace OR the project is the active workspace

---

## 🔍 STEP 1 — Auto-Detect Vault Path

The Main Vault may be on different drive letters depending on which PC the user is working from. Scan in this order:

```
SCAN ORDER:
  1. E:\My Drive\Main Vault
  2. G:\My Drive\Main Vault
  3. D:\My Drive\Main Vault
  4. C:\My Drive\Main Vault
  5. F:\My Drive\Main Vault
  6. H:\My Drive\Main Vault
  7. %USERPROFILE%\My Drive\Main Vault
  8. %USERPROFILE%\Google Drive\Main Vault

TEST: Check if CONTEXT.md exists at the candidate path.
RESULT: First match = VAULT_PATH
FAIL: If no path found, warn user: "Main Vault not detected. Is Google Drive mounted?"
```

Store the detected path in `.vault-context/vault_manifest.json`.

---

## 📦 STEP 2 — Setup Bridge in Project (First-Time Only)

If `.agents/AGENTS.md` does NOT exist in the project root, create the full bridge structure:

```
<PROJECT_ROOT>/
├── .agents/
│   ├── AGENTS.md                         ← Auto-sync rules for this workspace
│   └── skills/
│       └── vault_bridge/
│           └── SKILL.md                  ← Portable sync instructions
│
├── .vault-context/                       ← Local knowledge cache
│   ├── vault_manifest.json               ← Registry: vault path, project name, sync timestamps
│   ├── relevant_knowledge.md             ← Extracted vault knowledge for THIS project
│   ├── skills_snapshot.md                ← Copy of vault skills (verification, token optimizer, etc.)
│   ├── project_notes.md                  ← Snapshot of vault project planning docs
│   └── memory_snapshot.md                ← Copy of user preferences & corrections
```

Add `.vault-context/` to `.gitignore` (contains local paths and personal data).

---

## 🔄 STEP 3 — Automated Vault Sync (Token Optimizer)

DO NOT waste tokens manually reading and writing these files. Execute the automated sync script:

```powershell
& "E:\My Drive\Main Vault\.system\scripts\sync_vault_bridge.ps1" -ProjectDir "C:\Dev\<ProjectName>"
```

This script will automatically:
1. Pull the latest `memory.md`, `skills/`, and `01_Projects/` docs into `.vault-context/`
2. Extract relevant Wiki/Resource knowledge based on tech stack
3. Update the `vault_manifest.json` timestamp

### During Session (automatic, no asking):
```
• Code change made        → note in session tracking
• Decision made           → write directly to vault OR let sync script handle it at the end
```

### On Session End:
```
1. Write session log to vault: .system/logs/sessions/YYYY-MM-DD_session-N.md
2. Run `sync_vault_bridge.ps1` to flush caches and update the vault.
3. Report: "Automated sync complete."
```

---

## 🧠 STEP 5 — Knowledge Lookup Order (During Coding)

When the agent needs knowledge to solve a coding problem:

```
PRIORITY 1: Check Main Vault LIVE (if accessible)
  → 06_Wiki/ for concepts
  → 03_Resources/ for courses and references
  → 01_Projects/ for related project learnings
  → .system/memory.md for user preferences

PRIORITY 2: Check .vault-context/ CACHE (always available)
  → relevant_knowledge.md
  → project_notes.md
  → memory_snapshot.md
  → skills_snapshot.md

PRIORITY 3: Proceed with agent's own knowledge
```

---

## Rules

- **NEVER ask user** where the vault is — auto-detect it
- **NEVER ask user** what knowledge is relevant — scan and extract automatically
- **NEVER skip sync** — even if user doesn't say "done", write session data on every significant action
- **ALWAYS update .vault-context/ cache** at session start if vault is accessible
- **ALWAYS write back to vault** — decisions, learnings, session logs go to vault path, not just local
- Add `.vault-context/` to `.gitignore` — it contains local paths
- Do NOT add `.agents/` to `.gitignore` — it should be committed (portable rules)

---

## Output

After running this skill:
- `.agents/AGENTS.md` exists in project root with full sync rules
- `.agents/skills/vault_bridge/SKILL.md` exists with portable sync instructions
- `.vault-context/` populated with relevant vault knowledge
- `.gitignore` updated
- Vault path auto-detected and stored
- Agent ready to code with full vault context


## verification_loop

# 🛠️ SKILL: verification_loop
> **Trigger:** Runs automatically after EVERY file edit, code generation, ingestion, or system update.
> **Version:** 1.0 | **Last Updated:** 2026-07-30

---

## 🎯 Purpose
Ensures zero mistakes or halluncinations. After taking any action, the AI must run a self-verification loop to confirm correctness before declaring success to the user.

---

## 🔄 The Self-Verification & Correction Loop

```
               ┌───────────────────────────────┐
               │    STEP 1: Execute Action     │
               └──────────────┬────────────────┘
                              │
               ┌──────────────▼────────────────┐
               │   STEP 2: Verify & Inspect    │
               │  • File exists on disk?       │
               │  • Links & frontmatter valid? │
               │  • Tests/syntax error-free?   │
               └──────────────┬────────────────┘
                              │
                     Is it 100% Correct?
                              │
               ┌──────────────┴──────────────┐
               │                             │
           YES (Verified)               NO (Error Found)
               │                             │
     ┌─────────▼────────┐          ┌─────────▼────────┐
     │  Report Success  │          │ Backtrack & Fix  │
     └──────────────────┘          └─────────┬────────┘
                                             │
                                     (Loop Back to Step 2)
```

---

## 📋 Inspection Checklist (What to Verify)

### A. Markdown & Vault Files
- [ ] File exists at the absolute path specified.
- [ ] Required frontmatter YAML is syntactically valid.
- [ ] `[[Double Brackets]]` links point to valid notes or folders.
- [ ] Sources section is present (if Wiki note).
- [ ] No placeholder text like `[TO BE FILLED]` left unflagged.

### B. Code & Scripts
- [ ] Script runs without exit code errors.
- [ ] No missing imports or broken path references.
- [ ] Separated code lives in `C:\Dev\`, NOT inside the vault.

### C. System Logs & Memory
- [ ] User preferences recorded during session are logged in `.system/memory.md`.
- [ ] Log entry added to `.system/logs/ingestion_log.md` if source was ingested.

---

## 🚨 Backtracking & Self-Correction Protocol

If an error or failure occurs during verification:
1. **Do NOT pretend it succeeded.**
2. **Backtrack:** Read the exact error log or inspect the faulty file.
3. **Fix:** Make the required correction immediately.
4. **Re-Verify:** Re-run verification until 100% clean.
5. **Log Fix:** Append root cause to `.system/decision_records/` if architectural.



