---
name: exl-cloud-master
description: "Master router for all EXL Cloud tasks. Routes to 4 tier routers: Tier 1 (quality audit, skill check, model formatting, EXL Cloud standards), Tier 2 (clean data, map trial balance, build period table, date axis), Tier 3 (monthly commentary, board paper, cash flow narrative, management report), Tier 4 (variance analysis, KPI dashboard, assumption register, validation checks, model documentation, change log, meeting prep). Triggers any EXL Cloud task. Always views the correct Reference/ file before proceeding — never guesses."
---

# EXL Cloud Master Router

Top-level entry point for all EXL Cloud tasks. Identify the tier, load the correct `Reference/` file, then follow its routing logic to reach the right sub-skill.

---

## Tier Directory

| Tier | Router | File | Covers |
|------|--------|------|--------|
| Tier 1 | Foundation | `Reference/exl-cloud-tier-1.md` | Quality audits, skill checks, model formatting, EXL Cloud conventions |
| Tier 2 | Data Infrastructure | `Reference/exl-cloud-tier-2.md` | Clean data, trial balance mapping, period table / date axis |
| Tier 3 | Narrative Layer | `Reference/exl-cloud-tier-3.md` | Monthly commentary, cash flow narrative, board paper assembly |
| Tier 4 | Workbook Augmentation | `Reference/exl-cloud-tier-4.md` | Variance analysis, KPI dashboard, assumption register, validation checks, model docs, change log, meeting prep |

---

## Routing Rules

### Single-Tier Tasks

| Task | Load |
|------|------|
| "check my skill", "audit this skill", "quality check", "foundations check", "format my model", "colour code inputs", "apply EXL Cloud standards", "blue inputs black formulas" | `Reference/exl-cloud-tier-1.md` |
| "clean this data", "map my TB", "map trial balance", "COA mapping", "build tbl_Period", "set up periods", "ACT FCST toggle" | `Reference/exl-cloud-tier-2.md` |
| "write commentary", "monthly commentary", "explain the P&L", "cash flow narrative", "draft the board paper", "board pack", "board meeting paper" | `Reference/exl-cloud-tier-3.md` |
| "variance analysis", "actual vs budget", "build a dashboard", "KPI tiles", "assumption register", "validation checks", "add change log", "document this model", "meeting prep" | `Reference/exl-cloud-tier-4.md` |

### Multi-Tier Tasks (Load Order Matters)

| Task | Load Order |
|------|------------|
| New workbook from raw data | Tier 2 → Tier 1 |
| Monthly management pack | Tier 4 (skill-12) → Tier 3 |
| Full board paper | Tier 4 (skill-12, skill-13) → Tier 3 (skill-6, skill-8, skill-7) |
| Full model governance | Tier 1 → Tier 4 (skill-9, skill-10, skill-11, skill-14) |
| End-to-end new engagement | Tier 1 → Tier 2 → Tier 4 → Tier 3 |

---

## Decision Tree

```
What is the task?
│
├── Quality audit / skill review / model formatting / EXL Cloud standards?
│    └── VIEW: Reference/exl-cloud-tier-1.md
│
├── Data cleaning / TB mapping / period table setup?
│    └── VIEW: Reference/exl-cloud-tier-2.md
│
├── Writing commentary / cash flow narrative / assembling board paper?
│    └── VIEW: Reference/exl-cloud-tier-3.md
│
├── Variance analysis / KPI dashboard / assumption register /
│   validation checks / model docs / change log / meeting prep?
│    └── VIEW: Reference/exl-cloud-tier-4.md
│
└── Unsure / spans multiple tiers?
     └── VIEW tiers in order: Tier 1 → Tier 2 → Tier 4 → Tier 3
```

---

## Notes

- **Tier 2 before Tier 3** — data must be clean and mapped before narrative skills can run
- **Tier 4 analysis before Tier 3 narrative** — variance analysis (skill-12) and KPI dashboard (skill-13) feed the board paper (skill-7) and commentary (skill-6)
- **Tier 1 can run any time** — quality and formatting standards are independent of data state
- Each `Reference/` file is itself a router — it will direct to the correct sub-skill within that tier
