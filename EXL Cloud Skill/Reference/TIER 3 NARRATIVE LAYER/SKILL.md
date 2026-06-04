---
name: exl-cloud-tier-3
description: "Router for EXL Cloud Tier 3 narrative and reporting outputs. Sub-skills: (6) Monthly Commentary Generator — monthly P&L management commentary; (7) Board Paper Drafter — full board paper assembly; (8) Cash Flow Narrative — cash flow commentary. Reference by number e.g. 'use skill 6', 'run skill 7', 'skill 8'. Trigger phrases — skill 6: 'write commentary', 'explain the P&L', 'management narrative'; skill 7: 'draft the board paper', 'board pack', 'board report'; skill 8: 'explain cash flow', 'cash narrative', 'cash movements'. Always views the correct Reference/ file before proceeding — never guesses."
---

# EXL Cloud Tier 3 — Narrative & Reporting Router

Router for all narrative and reporting output tasks. Identify the task, load the correct `Reference/` file, follow its instructions. Never guess — always load.

---

## Sub-Skill Directory

| # | Skill | File | Use When |
|---|-------|------|----------|
| 6 | Monthly Commentary Generator | `Reference/skill-6-exl-cloud-monthly-commentary-generator.md` | Writing P&L management narrative from actuals vs budget |
| 7 | Board Paper Drafter | `Reference/skill-7-exl-cloud-board-paper-drafter.md` | Assembling a complete board/management report from sibling skill outputs |
| 8 | Cash Flow Narrative | `Reference/skill-8-exl-cloud-cash-flow-narrative.md` | Writing narrative for operating, investing, and financing cash flows |

---

## Routing Rules

### Single-Skill Tasks

| Task | Load |
|------|------|
| "write commentary", "monthly commentary", "explain the P&L", "management narrative", "write up the month", "commentary for the board", "explain the variances", "financial narrative" | `Reference/skill-6-exl-cloud-monthly-commentary-generator.md` |
| "draft the board paper", "board report", "prepare board pack", "management report for the board", "board meeting paper", "executive summary for board" | `Reference/skill-7-exl-cloud-board-paper-drafter.md` |
| "explain cash flow", "cash flow commentary", "cash narrative", "what happened to cash", "cash position summary", "explain the cash movements", "why did cash change" | `Reference/skill-8-exl-cloud-cash-flow-narrative.md` |

### Multi-Skill Tasks (Load Order Matters)

| Task | Load Order |
|------|------------|
| Full monthly narrative (P&L + cash flow) | `skill-6` → `skill-8` |
| Complete board paper | `skill-6` → `skill-8` → `skill-7` |

---

## Decision Tree

```
What is the task?
│
├── Writing P&L / management commentary?
│    └── VIEW: Reference/skill-6-exl-cloud-monthly-commentary-generator.md
│
├── Writing cash flow commentary?
│    └── VIEW: Reference/skill-8-exl-cloud-cash-flow-narrative.md
│
├── Assembling a board or management report?
│    └── VIEW: Reference/skill-7-exl-cloud-board-paper-drafter.md
│         (requires skill-6 and skill-8 outputs to be ready first)
│
└── Full narrative package (P&L + cash + board paper)?
     └── VIEW skill-6 → skill-8 → skill-7 in sequence
```

---

## Notes

- **Skill 7 is an assembler, not a generator** — it structures outputs from skill-6, skill-8, and Tier 4 skills (skill-12 variance, skill-13 KPI); run those first
- Skill 6 is stronger when Tier 4 variance analysis (skill-12) has already been run — it uses the variance data to identify key drivers
- Skill 8 covers cash flow narrative only; for P&L commentary use skill-6; do not mix them
- Tier 2 data prep (clean data, TB mapping) must be complete before any Tier 3 narrative skill runs
