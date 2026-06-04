---
name: exl-cloud-tier-2
description: "Router for EXL Cloud Tier 2 data preparation. Sub-skills: (3) Clean & Validate Data — cleaning/validating raw spreadsheet data; (4) Trial Balance Mapper — mapping TB accounts to P&L/BS categories; (5) Period Table Builder — building tbl_Period date axis with ACT/FCST flags. Reference by number e.g. 'use skill 3', 'run skill 4', 'skill 5'. Trigger phrases — skill 3: 'clean this data', 'fix formatting', 'remove duplicates'; skill 4: 'map my TB', 'COA mapping', 'map Xero TB'; skill 5: 'build tbl_Period', 'create date table', 'ACT FCST toggle'. Always views the correct Reference/ file before proceeding — never guesses."
---

# EXL Cloud Tier 2 — Data Preparation Router

Router for all data preparation tasks. Identify the task, load the correct `Reference/` file, follow its instructions. Never guess — always load.

---

## Sub-Skill Directory

| # | Skill | File | Use When |
|---|-------|------|----------|
| 3 | Clean & Validate Data | `Reference/skill-3-exl-cloud-clean-validate-data.md` | Cleaning raw data: whitespace, duplicates, date formats, text-to-number |
| 4 | Trial Balance Mapper | `Reference/skill-4-exl-cloud-trial-balance-mapper.md` | Mapping TB accounts to standardised P&L and BS categories |
| 5 | Period Table Builder | `Reference/skill-5-exl-cloud-period-table-builder.md` | Building tbl_Period date axis with ACT/FCST flags for time-series workbooks |

---

## Routing Rules

### Single-Skill Tasks

| Task | Load |
|------|------|
| "clean this data", "fix the formatting", "remove duplicates", "standardise dates", "trim whitespace", "numbers stored as text", "data quality check", "prep this data" | `Reference/skill-3-exl-cloud-clean-validate-data.md` |
| "map my TB", "map trial balance", "categorise accounts", "map Xero TB", "map MYOB accounts", "COA mapping", "map accounts to P&L and BS" | `Reference/skill-4-exl-cloud-trial-balance-mapper.md` |
| "build tbl_Period", "set up periods", "create date table", "build time series", "ACT FCST toggle", "financial year dates", "monthly periods" | `Reference/skill-5-exl-cloud-period-table-builder.md` |

### Multi-Skill Tasks (Load Order Matters)

| Task | Load Order |
|------|------------|
| Clean raw data then map to P&L/BS | `skill-3` → `skill-4` |
| Full data prep for a new workbook | `skill-3` → `skill-4` → `skill-5` |

---

## Decision Tree

```
What is the task?
│
├── Raw data needs cleaning / standardising?
│    └── VIEW: Reference/skill-3-exl-cloud-clean-validate-data.md
│         └── Also needs TB mapping after? → VIEW: Reference/skill-4-exl-cloud-trial-balance-mapper.md
│
├── Mapping TB accounts to P&L / BS categories?
│    └── VIEW: Reference/skill-4-exl-cloud-trial-balance-mapper.md
│         (if data is not yet clean, run skill-3 first)
│
├── Building the period/date axis for a time-series model?
│    └── VIEW: Reference/skill-5-exl-cloud-period-table-builder.md
│
└── Full data prep from scratch?
     └── VIEW skill-3 → skill-4 → skill-5 in sequence
```

---

## Notes

- **Skill 3 must run before Skill 4** — TB Mapper assumes clean, consistently formatted source data; dirty data causes broken mappings
- Skill 5 (Period Table) is independent of Skills 3 and 4 — it can be built at any point, but is typically set up after data is clean
- Skill 4 produces an Account Mapping sheet; that sheet feeds Tier 3 reporting skills (commentary, variance analysis)
