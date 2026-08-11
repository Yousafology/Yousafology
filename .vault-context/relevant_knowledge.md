# Relevant Knowledge Cache

## Agent_Skills_Index

# Agent Skills Index

> [!NOTE] **MASSIVE SKILLS UPGRADE** (2026-08-11)
> This static index only lists legacy manual skills. The system is now permanently connected to the `antigravity-skills` and `agentic-awesome-skills` repositories via `.gemini/config/community-skills`. Over 2,000+ dynamic skills are now available and autonomously active in the background! Use your IDE extensions to browse them.

- [[3d-web-experience]]
- [[accessibility-compliance-accessibility-audit]]
- [[ad-creative]]
- [[agent-memory-mcp]]
- [[agent-memory-systems]]
- [[agent-orchestration-multi-agent-optimize]]
- [[ai-agents-architect]]
- [[ai-ml-developer]]
- [[ai-product]]
- [[api-builder]]
- [[appdeploy]]
- [[browser-automation]]
- [[browser-extension-builder]]
- [[code-reviewer]]
- [[data-analyst]]
- [[defuddle]]
- [[docs-generator]]
- [[excel-master]]
- [[fix-review]]
- [[frontend-expert]]
- [[humanizer]]
- [[json-canvas]]
- [[mobile-developer]]
- [[obsidian-bases]]
- [[obsidian-cli]]
- [[obsidian-markdown]]
- [[obsidian_vault_context]]
- [[project_bootstrapper]]
- [[shadcn]]
- [[sql-optimization]]
- [[uiux-designer]]
- [[vault_sync_macro]]



## Boot Camp on Intel Macs (2012 MBP + Windows 10)

---
title: "Boot Camp on Intel Macs (2012 MBP + Windows 10)"
created: 2026-08-05
updated: 2026-08-05
tags: [windows, mac, bootcamp, drivers, troubleshooting]
sources: ["[[issue.txt]]", "https://github.com/timsutton/brigadier/releases"]
linked_to: ["[[Claude Code Best Practices]]"]
---

# Boot Camp on Intel Macs — Missing Icon / Missing Control Panel

## Problem
Boot Camp process runs in Task Manager but **no tray icon or window appears**.

Root cause: incomplete/wrong Boot Camp support package. `BootCamp.exe` starts but has no
`BootCamp.cpl` (Control Panel) to display, so it just sits idle.

## Diagnosis
- Check `C:\Windows\System32\BootCamp.cpl` — **must exist** after a good install.
- Check `C:\Program Files\Boot Camp\` — should contain `BootCamp.exe`, `AppleControlPanel.exe`, etc.
- Check service `Apple OS Switch Manager` — running = install partially succeeded.
- Older Macs (e.g. Mid-2012 MacBook Pro 9,2) need the **Windows 10-capable** package, NOT the
  Windows 7/8 package (e.g. 5.1.5621) from 3rd-party repack sites.

## Fix (no macOS needed)
1. Uninstall existing Boot Camp entries (Control Panel → Apps), delete leftover folders/files.
2. Download **Brigadier** → `https://github.com/timsutton/brigadier/releases`
3. Run in admin Command Prompt: `brigadier.exe -m MacBookPro9,2`
   (replace model id with your own, e.g. `-m MacBookPro11,1`). Downloads the correct package
   straight from Apple's servers.
4. Run the downloaded `Setup.exe` as Administrator → Install (ignore Windows logo warnings).
5. Verify `C:\Windows\System32\BootCamp.cpl` exists, then reboot → tray icon appears.

## Notes
- Boot Camp Assistant is a macOS app — it cannot be installed inside Windows.
- Windows 10 on 4GB RAM (2012 Macs) is slow regardless of drivers.
- 3rd-party driver repack sites (e.g. 123myIT) are unreliable for newer Windows versions.


## Claude Code Best Practices

---
title: "Claude Code Best Practices"
created: 2026-07-31
updated: 2026-07-31
tags: [agentic-coding, claude-code, workflow, best-practices]
sources: ["[[How to Use Claude Code Better Than 98% of People]]"]
linked_to: ["[[Skill Anatomy]]", "[[Progressive Context Loading]]"]
---

# Claude Code Best Practices

These best practices represent the shift from "vibe coding" (prompting and praying) to formally directing AI coding agents.

## 1. Director Mindset & Planning
You spend more time planning than you actually do building. The success of the AI is entirely dependent on the quality of the upfront plan.
- **Manage Context Early:** Attention is scarce. Do not overload the agent with irrelevant context upfront.
- **Grill Me:** Force the agent to ask you clarifying questions before it writes a single line of code so it does not make assumptions.

## 2. The Verification Harness
Never assume the agent is done just because it says it is. You must engineer a "harness" that forces the agent to prove its work.
- **Validation:** Use unit tests, Playwright for UI testing, or image-rendering (for visual diagrams) so the agent can iterate and fix its own mistakes before handing it back to you.

## 3. The Dumb Zone
Large Language Models have a finite window of sharp reasoning. As context grows, they enter the "dumb zone" and start missing obvious details (e.g., the needle in the haystack problem).
- **Opus 4.8:** ~250,000 tokens.
- **Sonnet 4.6:** ~125,000 tokens.
- *Solution:* Clear context frequently, use memory compaction, and rely on Harness Engineering.

## 4. Harness Engineering (The Ralph Loop)
For complex workflows, do not rely on a single agent session. String multiple sessions together.
- Agent A: Researches and Plans.
- Agent B: Executes the implementation based on the handoff document.
- Agent C: Validates and tests the code.

## 5. Security & System Evolution
Always assume that if an agent *can* touch something, it eventually *will* (e.g., accidentally deleting a database or sending a mass email). 
- Every bug or mistake should be treated as an opportunity for **System Evolution**. Do not just fix the code—update your system prompts, skills, and constraints so the agent never makes that mistake again.


## Claude Code Mastery (Nate Herk & Cole Medin)

---
title: "Claude Code Mastery (Nate Herk & Cole Medin)"
created: 2026-07-30
updated: 2026-07-30
tags: [claude-code, agentic-coding, prompt-engineering, dumb-zone]
sources: ["[[How to Use Claude Code Better Than 98% of People]]"]
linked_to: ["[[Progressive Context Loading]]", "[[The Four C's Framework]]", "[[Skill Anatomy]]"]
---

# Claude Code Mastery (Nate Herk & Cole Medin)

> Insights from Nate Herk & Cole Medin on directing AI coding agents, managing context limits, and preventing vibe coding errors.

---

## 🔑 Key Principles

### 1. Directing vs. "Vibe Coding"
- **Vibe Coding:** Prompting without a plan and hoping the AI gets it right.
- **Directing:** Writing explicit plans, enforcing verification steps, and breaking big tasks into modular agent sessions.

### 2. The "Dumb Zone" (~250k Tokens)
Even though models support up to 1M tokens, accuracy degrades sharply past 250k tokens (especially in Opus).
- **Solution:** Use progressive context loading and periodic context resets/compaction.

### 3. Make the Agent Prove Its Work (Verification Loop)
Never assume the agent's code works because it compiled. Enforce verification scripts, test execution, and visual checks before declaring success.

### 4. "Every Bug Is a Permanent Upgrade"
When an agent makes a mistake:
1. Identify the root cause.
2. Update the system constitution/SOP file (`agents.md` or `.system/skills/`).
3. The bug never happens again.

---

## 📚 Sources
- YouTube video: *How to Use Claude Code Better Than 98% of People* (Nate Herk & Cole Medin)
- Original clip: [[How to Use Claude Code Better Than 98% of People]]


## Life OS Sub-Systems (Health, Finance, Goals)

---
title: "Life OS Sub-Systems (Health, Finance, Goals)"
created: 2026-07-30
updated: 2026-07-30
tags: [life-os, health, finance, goals, para]
sources: ["[[obsidian-second-brain-workflows-2026-07-30]]"]
linked_to: ["[[Strategic Journaling and Chess Moves]]", "[[Personal CRM in Obsidian]]"]
---

# Life OS Sub-Systems (Health, Finance, Goals)

> Specialized areas of responsibility living in `02_Areas/` to manage non-deadline, ongoing life standards.

---

## 🏛️ The Sub-System Architecture

```
02_Areas/
├── CRM/            ← Person notes, meeting logs, networking
├── Health/         ← Blood work, macros, workout logs, energy tracking
├── Finance/        ← Bank statements, budget reports, investment notes
└── Goals/          ← 90-day sprints, quarterly reviews, vision
```

---

## 📊 Sub-System Breakdown

### 1. Health Area (`02_Areas/Health/`)
- **Blood Work & Biomarkers:** Track lab results over time.
- **Nutrition & Macros:** Protein, calories, supplementation protocols.
- **Fitness Logs:** Workout routines and recovery metrics.

### 2. Finance Area (`02_Areas/Finance/`)
- **Budget Reports:** Monthly income/expense summaries.
- **Asset Allocation:** Investment thesis and business revenue tracking.

### 3. Goals Area (`02_Areas/Goals/`)
- **90-Day Sprints:** 1-3 primary quarterly objectives.
- **Cadence Reviews:** Weekly, monthly, and annual retrospectives.

---

## 📚 Sources
- [[obsidian-second-brain-workflows-2026-07-30]]



