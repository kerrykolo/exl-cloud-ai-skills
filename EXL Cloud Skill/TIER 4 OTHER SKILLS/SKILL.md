---
name: exl-cloud-tier-4
description: "Router for EXL Cloud Tier 4 governance, analysis, delivery, and specialist workbook applications. Two sub-categories: (9–15) Core Governance & Analysis; (16–19) Other Skills / Specialist Workbook Applications. Reference by number e.g. 'use skill 9', 'run skill 16', 'skill 19'. Trigger phrases cover all 19 skills. Always load the correct Reference file before proceeding — never guess."
---

# EXL Cloud Tier 4 — Other Skills Router

**Router for all Tier 4 tasks.** Identify the task, load the correct SKILL.md file, follow its instructions. Never guess — always load.

---

## Skill Directory

| # | Skill | File | Use When |
|---|-------|------|----------|
| 9 | Assumption Register Builder | `SKILL_9_ASSUMPTION_REGISTER_BUILDER.md` | Extracting all model inputs and hardcoded values into a structured register |
| 10 | Anomaly Detection | `SKILL_10_ANOMALY_DETECTION_SKILL.md` | Adding a Validation Checks sheet with PASS/FAIL checks and conditional formatting |
| 11 | Model Documentation Generator | `SKILL_11_MODEL_DOCUMENTATION_GENERATOR.md` | Creating a structured Instructions sheet for any workbook |
| 12 | Variance Analysis Reporter | `SKILL_12_VARIANCE_ANALYSIS_REPORTER.md` | Building actuals vs budget/forecast/prior comparison with $ and % variances |
| 13 | KPI Dashboard Formatter | `SKILL_13_KPI_DASHBOARD_FORMATTER.md` | Building a one-page KPI tile dashboard with traffic lights |
| 14 | Change Log & Audit Trail | `SKILL_14_CHANGE_LOG___AUDIT_TRAIL.md` | Adding Change Log and Claude Log governance sheets |
| 15 | Meeting Prep Pack | `SKILL_15_MEETING_PREP_PACK.md` | Generating a concise brief for board/committee meeting presentation |
| 16 | Weekly Cash Flow Scenario Builder | `SKILL_16_WEEKLY_CASH_FLOW_SCENARIO_BUILDER.md` | Building and populating Base/Best/Worst scenarios in an EXL Cloud Weekly CF model |
| 17 | Human Capital Report Commentary | `SKILL_17_HUMAN_CAPITAL_REPORT_COMMENTARY.md` | Building an HC executive summary from payroll data in an EXL Cloud model |
| 18 | Invoice & Bill Pull Analysis | `SKILL_18_INVOICE_BILL_PULL_ANALYSIS.md` | Analysing Xero invoice/credit note data pull workbooks (4-sheet suite) |
| 19 | Performance Report Dashboard Commentary | `SKILL_19_PERFORMANCE_REPORT_COMMENTARY.md` | Writing dashboard commentary for EXL Cloud Performance Reports workbooks |

---

## Routing Rules

| Task | Load |
|------|------|
| "extract assumptions", "build assumption register", "list all inputs", "find hardcoded values", "assumption audit", "document the inputs" | `SKILL_9_ASSUMPTION_REGISTER_BUILDER.md` |
| "add validation checks", "build checks", "add error detection", "PASS FAIL checks", "model integrity checks", "add balance sheet check", "audit checks" | `SKILL_10_ANOMALY_DETECTION_SKILL.md` |
| "document this model", "add instructions", "create documentation", "explain the model structure", "add a readme sheet", "document the workbook" | `SKILL_11_MODEL_DOCUMENTATION_GENERATOR.md` |
| "variance analysis", "actual vs budget", "compare to forecast", "what changed vs last year", "variance report", "budget variance", "explain the variances" | `SKILL_12_VARIANCE_ANALYSIS_REPORTER.md` |
| "build a dashboard", "KPI tiles", "executive dashboard", "one-page summary", "traffic light dashboard", "key metrics view", "management dashboard" | `SKILL_13_KPI_DASHBOARD_FORMATTER.md` |
| "add change log", "add audit trail", "track changes", "version history", "add governance sheets", "claude log", "AI audit trail", "model governance" | `SKILL_14_CHANGE_LOG___AUDIT_TRAIL.md` |
| "meeting prep", "prepare talking points", "board meeting brief", "committee prep", "what should I present", "meeting pack" | `SKILL_15_MEETING_PREP_PACK.md` |
| "populate the weekly cash flow scenarios", "build CF scenarios", "set up the scenario overlay", "weekly cash flow analysis", "populate Base/Best/Worst", "scenario overlay for the weekly model", "add scenarios to the weekly CF" | `SKILL_16_WEEKLY_CASH_FLOW_SCENARIO_BUILDER.md` |
| "build HC analysis", "human capital report", "payroll executive summary", "workforce commentary", "HC commentary", "salary data analysis", "headcount analysis", "workforce snapshot", "staffing cost summary", "build the HC exec summary", "analyse the payroll data" | `SKILL_17_HUMAN_CAPITAL_REPORT_COMMENTARY.md` |
| "analyse the invoice pull", "build the bill analysis", "invoice and credit note analysis", "what does this Xero pull show", "build the exec summary for this data", "analyse the billing data", "invoice pull dashboard", "break down the invoices" | `SKILL_18_INVOICE_BILL_PULL_ANALYSIS.md` |
| "write dashboard commentary", "populate the commentary text boxes", "fill in the dashboard text boxes", "write CF4 commentary for the dashboards", "update the actuals/revenue/expense/EBITDA/combo dashboard commentary", "analyse the P&L and write commentary" | `SKILL_19_PERFORMANCE_REPORT_COMMENTARY.md` |

---

## Decision Tree

```
What is the task?
│
├── Extract / document model assumptions or hardcodes?
│    └── LOAD: SKILL_9_ASSUMPTION_REGISTER_BUILDER.md
│
├── Add validation checks or PASS/FAIL integrity sheet?
│    └── LOAD: SKILL_10_ANOMALY_DETECTION_SKILL.md
│
├── Add an Instructions / documentation sheet to the workbook?
│    └── LOAD: SKILL_11_MODEL_DOCUMENTATION_GENERATOR.md
│
├── Compare actuals to budget, forecast, or prior period?
│    └── LOAD: SKILL_12_VARIANCE_ANALYSIS_REPORTER.md
│         └── Need commentary too? → hand off to Tier 3 skill-6
│
├── Build a KPI dashboard with traffic lights?
│    └── LOAD: SKILL_13_KPI_DASHBOARD_FORMATTER.md
│         └── Feeding into board paper? → hand off to Tier 3 skill-7
│
├── Add change tracking / audit trail / Claude log?
│    └── LOAD: SKILL_14_CHANGE_LOG___AUDIT_TRAIL.md
│
├── Prepare for a board or committee meeting?
│    └── LOAD: SKILL_15_MEETING_PREP_PACK.md
│
├── Weekly Cash Flow Scenario Model needing Base/Best/Worst scenarios?
│    └── LOAD: SKILL_16_WEEKLY_CASH_FLOW_SCENARIO_BUILDER.md
│
├── EXL Cloud financial model needing Human Capital / Payroll analysis?
│    └── LOAD: SKILL_17_HUMAN_CAPITAL_REPORT_COMMENTARY.md
│
├── Xero invoice/credit note data pull needing analysis & dashboarding?
│    └── LOAD: SKILL_18_INVOICE_BILL_PULL_ANALYSIS.md
│
└── EXL Cloud Performance Reports workbook needing dashboard commentary?
     └── LOAD: SKILL_19_PERFORMANCE_REPORT_COMMENTARY.md
```

---

## Multi-Skill Tasks & Load Order

| Task | Load Order |
|------|------------|
| Variance analysis + commentary feed | `skill-12` → (hand off to Tier 3 skill-6) |
| KPI dashboard + board paper feed | `skill-13` → (hand off to Tier 3 skill-7) |
| Full governance package | `skill-9` → `skill-10` → `skill-11` → `skill-14` |
| Full delivery package (analysis + dashboard + meeting prep) | `skill-12` → `skill-13` → `skill-15` |
| Weekly CF scenarios + executive summary | `skill-16` (auto-builds Exec Summary) → Feeds into Tier 3 for commentary if needed |
| HC analysis standalone | `skill-17` (standalone) → Feeds into Tier 3 skill-7 (board paper) if needed |
| Invoice analysis standalone | `skill-18` (4-sheet suite) → Can feed into Tier 3 for narrative if needed |
| Dashboard commentary only | `skill-19` (text boxes only) → Works within Performance Reports workbook |

---

## Notes

- **Skills 9–15**: Generic governance, analysis, and delivery tasks — applicable to any workbook type
- **Skills 16–19**: Domain and workbook-specific — each tied to a particular workbook structure or data source
- **Recommended governance sequence**: 9 → 10 → 11 → 14 for a full governance package
- **Skills 16–19 execution**: Each skill runs independently and is workbook-specific
- **Cross-tier flow**: Tier 4 outputs (especially skills 12, 13, 16) feed into Tier 3 for narrative assembly
- **Skill Independence**: Skills 9, 10, 11, 14 can run in any order; Skills 16–19 are independent of each other
