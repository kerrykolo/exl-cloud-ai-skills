---
name: exl-cloud-tier-4
description: "Router for EXL Cloud Tier 4 governance, analysis, and delivery. Covers: assumption registers, validation/PASS-FAIL checks, model documentation, variance analysis, KPI dashboards, change logs, meeting prep packs. Triggers: 'extract assumptions', 'build assumption register', 'add validation checks', 'PASS FAIL', 'document this model', 'add instructions', 'variance analysis', 'actual vs budget', 'build a dashboard', 'KPI tiles', 'add change log', 'audit trail', 'model governance', 'meeting prep', 'prepare talking points', 'board meeting brief'. Always views the correct Reference/ file before proceeding — never guesses."
---

# EXL Cloud Tier 4 — Governance, Analysis & Delivery Router

Router for governance, analysis, and delivery tasks. Identify the task, load the correct `Reference/` file, follow its instructions. Never guess — always load.

---

## Sub-Skill Directory

| # | Skill | File | Use When |
|---|-------|------|----------|
| 9 | Assumption Register Builder | `Reference/skill-9-exl-cloud-assumption-register-builder.md` | Extracting all model inputs and hardcoded values into a structured register |
| 10 | Anomaly Detection | `Reference/skill-10-exl-cloud-anomaly-detection.md` | Adding a Validation Checks sheet with PASS/FAIL checks and conditional formatting |
| 11 | Model Documentation Generator | `Reference/skill-11-exl-cloud-model-documentation-generator.md` | Creating a structured Instructions sheet for any workbook |
| 12 | Variance Analysis Reporter | `Reference/skill-12-exl-cloud-variance-analysis-reporter.md` | Building actuals vs budget/forecast/prior comparison with $ and % variances |
| 13 | KPI Dashboard Formatter | `Reference/skill-13-exl-cloud-kpi-dashboard-formatter.md` | Building a one-page KPI tile dashboard with traffic lights |
| 14 | Change Log & Audit Trail | `Reference/skill-14-exl-cloud-change-log-audit-trail.md` | Adding Change Log and Claude Log governance sheets |
| 15 | Meeting Prep Pack | `Reference/skill-15-exl-cloud-meeting-prep-pack.md` | Generating a concise brief for board/committee meeting presentation |

---

## Routing Rules

### Single-Skill Tasks

| Task | Load |
|------|------|
| "extract assumptions", "build assumption register", "list all inputs", "find hardcoded values", "assumption audit", "document the inputs" | `Reference/skill-9-exl-cloud-assumption-register-builder.md` |
| "add validation checks", "build checks", "add error detection", "PASS FAIL checks", "model integrity checks", "add balance sheet check", "audit checks" | `Reference/skill-10-exl-cloud-anomaly-detection.md` |
| "document this model", "add instructions", "create documentation", "explain the model structure", "add a readme sheet", "document the workbook" | `Reference/skill-11-exl-cloud-model-documentation-generator.md` |
| "variance analysis", "actual vs budget", "compare to forecast", "what changed vs last year", "variance report", "budget variance", "explain the variances" | `Reference/skill-12-exl-cloud-variance-analysis-reporter.md` |
| "build a dashboard", "KPI tiles", "executive dashboard", "one-page summary", "traffic light dashboard", "key metrics view", "management dashboard" | `Reference/skill-13-exl-cloud-kpi-dashboard-formatter.md` |
| "add change log", "add audit trail", "track changes", "version history", "add governance sheets", "claude log", "AI audit trail", "model governance" | `Reference/skill-14-exl-cloud-change-log-audit-trail.md` |
| "meeting prep", "prepare talking points", "board meeting brief", "committee prep", "what should I present", "meeting pack" | `Reference/skill-15-exl-cloud-meeting-prep-pack.md` |

### Multi-Skill Tasks (Load Order Matters)

| Task | Load Order |
|------|------------|
| Variance analysis + commentary feed | `skill-12` → (hand off to Tier 3 skill-6) |
| KPI dashboard + board paper feed | `skill-13` → (hand off to Tier 3 skill-7) |
| Full governance package | `skill-9` → `skill-10` → `skill-11` → `skill-14` |
| Full delivery package (analysis + dashboard + meeting prep) | `skill-12` → `skill-13` → `skill-15` |

---

## Decision Tree

```
What is the task?
│
├── Find / document model assumptions or hardcodes?
│    └── VIEW: Reference/skill-9-exl-cloud-assumption-register-builder.md
│
├── Add validation checks or PASS/FAIL integrity sheet?
│    └── VIEW: Reference/skill-10-exl-cloud-anomaly-detection.md
│
├── Add an Instructions / documentation sheet to the workbook?
│    └── VIEW: Reference/skill-11-exl-cloud-model-documentation-generator.md
│
├── Compare actuals to budget, forecast, or prior period?
│    └── VIEW: Reference/skill-12-exl-cloud-variance-analysis-reporter.md
│         └── Need commentary too? → hand off to Tier 3 skill-6
│
├── Build a KPI dashboard with traffic lights?
│    └── VIEW: Reference/skill-13-exl-cloud-kpi-dashboard-formatter.md
│         └── Feeding into board paper? → hand off to Tier 3 skill-7
│
├── Add change tracking / audit trail / Claude log?
│    └── VIEW: Reference/skill-14-exl-cloud-change-log-audit-trail.md
│
├── Prepare for a board or committee meeting?
│    └── VIEW: Reference/skill-15-exl-cloud-meeting-prep-pack.md
│         (requires skill-12, skill-13, and Tier 3 skill-6/7 outputs)
│
└── Full governance package?
     └── VIEW skill-9 → skill-10 → skill-11 → skill-14 in sequence
```

---

## Notes

- **Skills 9, 10, 11, 14 are independent** — they can run in any order, but the sequence `9 → 10 → 11 → 14` is the recommended governance workflow
- **Skill 12 feeds Tier 3** — variance analysis output is consumed by skill-6 (commentary) and skill-7 (board paper); always run skill-12 before those
- **Skill 13 feeds Tier 3** — KPI dashboard output is consumed by skill-7 (board paper); run skill-13 before skill-7
- **Skill 15 is a final-stage aggregator** — it consumes outputs from skills 12, 13, and Tier 3 skills 6 and 7; run everything else first
- Tier 2 data prep must be complete before skills 12 or 13 can run against live workbook data
