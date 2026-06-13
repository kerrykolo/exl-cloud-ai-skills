---
name: skill-16-exl-cloud-weekly-cf-scenario-analysis
description: "Build and populate scenario overlays for EXL Cloud Weekly Cash Flow models. Use when the user says 'populate the weekly cash flow scenarios', 'build CF scenarios', 'set up the scenario overlay', 'weekly cash flow analysis', 'populate Base/Best/Worst', 'scenario overlay for the weekly model', 'add scenarios to the weekly CF', 'cash flow scenario planning', or invokes /weekly-cf-scenario-analysis. Analyses an EXL Cloud Weekly Cash Flow Scenario Model's actuals, sources P&L from connected peer workbooks, populates 3 scenarios (Base/Best/Worst) in Scenario Overlay using $ Repeat/$ Bullet/$ Spread, builds Executive Summary with scenario comparison and risk flags, adds Validation Checks, Change Log, Claude Log. Do NOT use for monthly P&L commentary, 3-way model building (use exl-cloud-model-builder), or non-EXL-Cloud cash flow models."
---

# Weekly Cash Flow Scenario Analysis

Analyses an EXL Cloud Weekly Cash Flow Scenario Model, populates Base/Best/Worst scenario overlays grounded in actual performance data, and builds companion Executive Summary, Validation Checks, Change Log, and Claude Log sheets. All scenario entries are company-agnostic — calibrated from the entity's own actuals and related workbook data, not hardcoded industry assumptions.

**Type:** Encoded Preference · Workflow Automation

**Before outputting:** Verify this sheet against skill-2 and skill-1 foundations.

## When to Use

- You have an EXL Cloud Weekly Cash Flow Scenario Model open (identifiable by sheets: 1. Master Control, 3.a. Base Forecast, 3.b. Scenario Overlay, 3.c. Weekly Cash Flow Forecast, 4.a. Chart Data, 4.b-4.d Dashboards, 5. Helper)
- You need to populate scenarios with realistic Base, Best, and Worst cases
- You want a scenario-driven executive summary of the cash position
- You're preparing for board or stakeholder presentations requiring scenario analysis

## Prerequisite Context
Before building scenarios, gather data from multiple sources:

1. **This workbook**: Read Master Control for entity name, currency, and date range. Read actuals columns in 3.c to identify revenue and cost patterns.
2. **Connected Performance Reports** (if peer agent available): Request or extract monthly P&L data — revenue breakdown by account, EBITDA margin, cost structure, one-off items, and seasonality patterns. This provides business context to calibrate scenario magnitudes.
3. **Connected Invoices workbook** (if peer agent available):Request accounts receivable and payable (AR/AP)) aging data to calibrate collection and payment delay assumptions. If unavailable, use actuals from the Weekly CF model's historical data.

## Step 1: Discover Workbook Structure

Read and record these locations. They are fixed in the EXL Cloud Weekly CF model template but may vary if the template has been customized:

### Key Named Ranges
- `pModelName` → typically points to '1. Master Control'!B3 (model name for headers)
- Scenario names → typically at '5. Helper'!W11:W13 (the three scenario labels : Base, Best, or Worst)
- `pChosenScenario` → typically points to '5. Helper'!F28 (active scenario from dropdown)


### Scenario Overlay Layout (sheet '3.b. Scenario Overlay')
- **AR/AP Delay cells**: F12/F13 (Base), F41/F42 (Best), F70/F71 (Worst) — integer days adjustment
- **Data rows**: Base 17-36, Best 46-65, Worst 75-94 (20 slots each)
- **Columns**: C=AccountType, D=AccountName, E=Behaviour, F=StartPeriod, G=#ofWeeks, H=Amount
- **Impact area**: Columns BN-DM (52 forecast weeks per scenario row)
- **Behaviour index**: Column BM maps E text → CHOOSE index via `MATCH($E{row}, t_ScenarioOptions, 0)`

### Chart Data Layout (sheet '4.a. Chart Data')
- Columns C-BB = actuals (52 periods), BC-DB = forecast (WK1-WK52)
- **Worst Scenario**: Opening=row 60, CashIn=61, CashOut=62, Net=63, Closing=64
- **Base Scenario**: Opening=row 102, CashIn=103, CashOut=104, Net=105, Closing=106
- **Best Scenario**: Opening=row 144, CashIn=145, CashOut=146, Net=147, Closing=148

### Account Types (Helper Z11:Z20)
Current Liability, Expense, Current Asset, Sale, Direct Costs, Non-current Asset, Fixed Asset, Non-current Liability, Revenue, Other Income

## Step 2: Understand the Overlay Formula Mechanics

**CRITICAL** — misunderstanding the calculation patterns causes all scenario entries to produce zero impact.

The impact area uses a CHOOSE formula:
```
=IFERROR(CHOOSE($BM{row},
$H{row}*M{row}, // 1: % — Amount × base forecast value
IF(AND(col$16>$F,col$16<=($F+$G*7)),$H,0), // 2: $ Repeat
IF(AND(col$16>$F,col$16<=($F+$G*7)),$H/$G,0), // 3: $ Spread
IF(col$15=$F,$H,IF(col$15=($F+$G*7),-$H,0)), // 4: $ Start and End
IF(col$15=($F+$G*7),$H,0) // 5: $ Bullet
),0)
```

### Key Rules
- **StartPeriod (column F) is a DATE SERIAL**, not a week counter. Use the first forecast week start date from the model (typically found at the forecast start column row 9 of 3.c).
- **$ Repeat**: Active when `week_end > StartPeriod AND week_end <= StartPeriod + weeks × 7`. For full-year: StartPeriod = WK1 date, weeks = 52.
- **$ Bullet**: Fires once when `week_start = StartPeriod + weeks × 7`. To fire at WK4: set StartPeriod = WK1 date, weeks = 3 (since WK1_date + 21 = WK4_start).
- **% behaviour requires 3.a. Base Forecast data** — if Base Forecast rows are empty, % multiplies by zero. Use $ Repeat instead.
- **Sign convention**: Positive amount = cash inflow. Negative = cash outflow. Revenue uplift = positive. Expense increase = negative. Expense savings = positive.

## Step 3: Analyse Actual Performance

From actuals in 3.c and peer workbook data, determine:

1. **Revenue composition**: Which accounts dominate total cash in? Flag concentration risk if one line is 70%+.
2. **Cost base structure**: Average weekly costs by category, trend direction (increasing/stable/decreasing).
3. **Seasonality**: Weak months (holidays, industry-specific slowdowns) and strong months.
4. **One-off items**: Prior year EOFY payments (tax, dividends, insurance, levies) — these calibrate bullet amounts.
5. **AR/AP patterns**: Average collection and payment delays from actuals or invoices data.
6. **Cash range**: 52-week high, low, average, current closing position.
Use this analysis to set scenario amounts as % adjustments to observed averages (e.g., Best = +15% revenue uplift on average weekly figure).

## Step 4: Design & Populate Scenarios

Write entries to '3.b. Scenario Overlay' starting at the first data row of each scenario block.

### Base Case — Expected outcome, minimal adjustments

- **Entry type**: EOFY obligations only, using $ Bullet behaviour
- **Timing**: Calculate which week contains the entity's financial year end (requires reading FY_End month from Master Control)
- **Accounts**: Income tax, dividends, insurance premiums (one-time annual costs)
- **AR/AP delays**: Leave at 0 (no change from actuals)
- **Typical count**: 3-5 entries

### Best Case — Upside scenario
- **Revenue uplift**: Increase primary revenue lines by +10% to +20% relative to 52-week average (use $ Repeat)
- **New revenue sources**: Add entries for growth channels or market expansion (if applicable) using $ Repeat
- **Cost savings**: Reduce variable expense lines (use $ Repeat with positive amounts, since negative expenses mean less outflow)
- **AR improvement**: Reduce collection delays by 5-10 days (lower AP delay cell value)
- **EOFY adjustments**: Reduce tax or dividend provisions if best-case earnings are lower
- **Typical count**: 6-10 entries

### Worst Case — Downside stress test
- **Revenue decline**: Reduce primary lines by -15% to -25% relative to 52-week average (use $ Repeat)
- **Secondary revenue decline**: Apply smaller declines to secondary lines
- **Cost increases**: Increase variable expenses (use $ Repeat with negative amounts, since higher expenses = more outflow)
- **Seasonal shutdown**: Model a 2-4 week window (e.g., Christmas/New Year, industry-specific closure) with additional revenue reduction using $ Repeat
- **AR/AP deterioration**: Increase collection delays by 10-15 days (higher AR delay cell value), decrease payment terms (lower AP delay cell value)
- **EOFY adjustments**: Increase tax provisions as a liquidity buffer; optionally reduce dividends
- **Typical count**: 8-12 entries

### After Writing Each Scenario Block
Verify impact area by reading columns BN-BQ for the first few data rows:
- $ Repeat entries show the H amount in all active-week columns and 0 outside
- $ Bullet entries show 0 everywhere except the target week

## Step 5: Build Executive Summary

Create new sheet "0. Executive Summary":

### Title Block (rows 2-4)
- Entity name: `=INDEX('1. Master Control'!I14:I39,MATCH("x",'1. Master Control'!J14:J39,0))`
- Model name + date range (link to pModelName)
- Prepared date + active scenario: `="Prepared: "&TEXT(TODAY(),"d mmm yyyy")&" | Active Scenario: "&pChosenScenario`
### Model Overview (rows 6-12)
Key/value pairs: Entity, Reporting Basis, Currency, Actuals Period, Forecast Period

### Scenario Comparison Table (rows 14-23)
Headers: Metric | Base | Best | Worst | Best-Worst Spread

| Metric | Formula Pattern (Base example) |
|--------|-------------------------------|
| Current Cash (Last Actual) | `='4.a. Chart Data'!BB106` |
| EOFY Cash Trough | Value at the bullet week column (typically BF) |
| Minimum Cash (Forecast) | `=MIN('4.a. Chart Data'!BC106:DB106)` |
| Closing Cash (WK52) | `='4.a. Chart Data'!DB106` |
| Net Cash Movement | `=SUM('4.a. Chart Data'!BC105:DB105)` |
| Total Cash Inflows | `=SUM('4.a. Chart Data'!BC103:DB103)` |
| Total Cash Outflows | `=SUM('4.a. Chart Data'!BC104:DB104)` |
| Avg Weekly Cash Balance | `=AVERAGE('4.a. Chart Data'!BC106:DB106)` |

Spread = Best minus Worst. Substitute row numbers for Best (148/147/146/145/144) and Worst (64/63/62/61/60).

### Key Assumptions (rows 25-33)
Table linking each overlay entry back to 3.b cells with a Rationale column explaining business basis.

### Risk Flags (rows 35-42)
Colour-coded rows: ⚠️ HIGH (#FFC7CE/#9C0006), ℹ️ MEDIUM (#FFEB9C/#9C6500), ✅ POSITIVE (#C4DDD2/#006100).

### Formatting
Green palette headers (#2D6A4F), sub-headers (#D8F3DC), tab colour #1A3C2E. Arial throughout. Currency format: $#,##0;($#,##0);"-".

## Step 6: Build Validation Checks

**First:** Check if a Validation Checks sheet already exists. If it does, append new checks to the existing sheet rather than deleting and rebuilding. If it does not, create "Validation Checks" sheet 

the model checks to add (10+):

| # | Check | Category | Expected | Actual | Status |
|---|-------|----------|----------|--------|--------|
| 1-3 | Overlay entry counts per scenario | Overlay | Expected count | COUNTA | PASS/FAIL |
| 4-5 | Bullet fires at correct week | Bullet | Expected amount | Impact cell | PASS/FAIL |
| 6 | Scenarios produce different WK52 closing | Divergence | TRUE | IF comparison | PASS/FAIL |
| 7 | Best > Base > Worst ordering | Order | TRUE | AND comparison | PASS/FAIL |
| 8 | Worst trough above minimum floor | Liquidity | Above floor | MIN formula | PASS/FAIL |
| 9 | Exec Summary cross-check | Cross-check | Match | ABS difference | PASS/FAIL |
| 10 | No formula errors in overlay | Integrity | 0 errors | SUMPRODUCT(ISNA) | PASS/FAIL |

Apply conditional formatting: PASS = #C4DDD2/#006100, FAIL = #FFC7CE/#9C0006. Summary: "X of N checks passed".

**Colour formatting:**
- Sheet tab colour: #384329 (dark green)

## Step 7: Change Log & Claude Log
- **Change Log**: Numbered entries (Date, Author, Description, Sheets Affected)
- **Claude Log**: Standard header (Turn #, Date, User Request, Action Taken, Details, Outcome)
## Gotchas & Edge Cases

### Overlay Mechanics
- **StartPeriod is a date serial, NOT a week number** — entering 3 instead of 46188 (or whatever the date serial is) causes all entries to produce zero. Always read the first forecast week start date from 3.c and use that as the anchor.
- **% behaviour is dead if 3.a is empty** — the intermediate calc multiplies Amount × base value. If base = 0, result = 0. Always use $ Repeat for explicit amounts.
- **$ Bullet fires at StartPeriod + weeks × 7** — matches against week START dates, not end dates. Off-by-one on weeks shifts the bullet by 7 days.
- **Two entries for the same account stack** — the model sums all overlay rows per account. Use deliberately for layered effects (e.g., annual decline + Christmas shutdown).
### Multi-Workbook Environment
- **execute_office_js may fail with InactiveWorkbook** when peer workbooks are connected. Use set_cell_range as the primary write method — it's more reliable in multi-workbook sessions.

### Sign Convention
- Revenue/Sale: positive = more cash IN, negative = less
- Expense: negative = more cash OUT (cost increase), positive = less cash OUT (savings)
- AR delay: negative days = faster collection (Best), positive = slower (Worst)
- AP delay: negative days = faster payment (Worst), positive = more time (Best)
### EOFY Timing
- For Australian entities, EOFY = 30 June. Tax/dividend bullets land at WK3-5 if WK1 starts early June.
- Use Expense account type for tax, dividends, and insurance (direct cash impact). Current Liability also works but is less intuitive.
- Cross-check prior year one-off amounts from Performance Reports P&L or actuals.
### Chart Data & Exec Summary
- BB (last actual column) is the same across all scenarios — use for "Current Cash".
- Entity name: Use INDEX/MATCH on I14:I39/J14:J39 from Master Control. Do NOT use B5 (platform name, not entity).
- Scenario Comparisons (4.c) and Dashboards (4.d) auto-update from Chart Data — no manual wiring needed.
## Done When
1. All three scenario blocks in 3.b have populated entries with non-zero impact values
2. Chart Data (4.a) shows different closing cash per scenario at WK52
3. Executive Summary exists with overview, comparison table, assumptions, and risk flags
4. Validation Checks: 10+ checks, all PASS
5. Change Log and Claude Log document all work

## Constraints

- Do NOT freeze panes, hide gridlines, or modify sheet view settings
