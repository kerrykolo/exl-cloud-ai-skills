---
name: skill-19-exl-cloud-performance-report-commentary
description: Use when the user says 'write dashboard commentary', 'populate the commentary text boxes', 'fill in the dashboard text boxes', 'write CF4 commentary for the dashboards', 'update the actuals/revenue/expense/EBITDA/combo dashboard commentary', 'analyse the P&L and write commentary', or invokes /performance-report-commentary. Reads P&L and charts data from an EXL Cloud Performance Reports workbook, synthesises CF4-level analysis commentary, and writes it directly into named commentary text boxes (tbComment*) on each dashboard sheet. Covers Actuals, Combo, Revenue, Expense, and EBITDA dashboards. Do NOT use for P&L pivot analysis, creating new dashboards, or building new charts.
---
 
# Performance Report Dashboard Commentary
 
Reads P&L and summary data from an EXL Cloud Performance Reports workbook and writes CF4-level business analysis commentary into the named text boxes on each dashboard sheet. Commentary is company-agnostic in structure but specific in content — driven entirely by the actual data in the workbook rather than any pre-loaded assumptions.

**Before outputting:** Verify this sheet against skill-2 and skill-1 foundations.

---
 
## Context Required
 
Before writing any commentary, read:
1. **1. Master Control** — entity name, period end date (month/year), financial year start month, selected organisation
2. **2.c. Data - Charts** — all KPI totals: YTD, PYTD, monthly and quarterly values by account type (Revenue, Sales, Other Income, Direct Costs, Indirect Costs, EBITDA)
3. **3.a. Profit and Loss Pivot** — line-by-line monthly account data for the last 12–24 months; primary source for understanding individual revenue streams and expense categories
4. **3.b. Actual v Budget Report** — monthly summary table for the reporting period
Read all four in a single pass before drafting any commentary. Do not draft from partial data.
 
---
 
## Steps
 
### Step 1 — Identify the Commentary Text Boxes
 
Use `execute_office_js` to enumerate shapes on each dashboard sheet and locate the text boxes. Standard naming pattern:
 
| Dashboard Sheet | Text Box Name |
|----------------|--------------|
| 4.a. Actuals Dashboard | tbCommentADB |
| 4.b. Combo Dashboard | tbCommentCDB |
| 4.c. Revenue Dashboard | tbCommentRDB |
| 4.d. Expense Dashboard | tbCommentEDB |
| 4.e. EBITDA Dashboard | tbCommentEBDB |
 
If the workbook is from a new client or has been customised, verify box names via shape enumeration before writing. Do not assume names match the standard pattern.
 
```javascript
// Enumerate shapes to verify names — run on each dashboard sheet
const sheet = context.workbook.worksheets.getItem("4.a. Actuals Dashboard");
const shapes = sheet.shapes;
shapes.load("items/name,items/type");
await context.sync();
return { shapes: shapes.items.map(s => ({ name: s.name, type: s.type })) };
```
 
### Step 2 — Analyse the Data
 
For each dashboard, extract specific data points before writing:
 
- **Actuals:** YTD Revenue, Expenses, EBITDA (actual and PY). YTD margin. EBITDA variance %. Any non-recurring items in the period.
- **Combo:** Full P&L waterfall YTD. Quarterly EBITDA (Q1–Q4). Three-month revenue/expense/EBITDA trend. Direct vs indirect cost split.
- **Revenue:** Revenue broken down by account type (Sales, Revenue, Other Income). Individual stream contribution from the P&L Pivot. Monthly trend. Best/worst months.
- **Expense:** Top 5–8 expense categories by YTD value. Monthly run-rate. Non-recurring or lump-sum items. Direct vs indirect split.
- **EBITDA:** Monthly EBITDA profile. Best/worst months and quarters. Margin trend. Statutory vs underlying EBITDA when non-recurring items exist.
### Step 3 — Write Commentary
 
Write each commentary block using `execute_office_js`:
 
```javascript
const sheet = context.workbook.worksheets.getItem("4.a. Actuals Dashboard");
const shape = sheet.shapes.getItem("tbCommentADB");
shape.textFrame.textRange.text = "Your commentary text here...";
await context.sync();
```
 
Write all five in sequence. Do not batch into a single JS call — write one shape per `execute_office_js` call so each can be verified independently.
 
### Step 4 — Verify
 
After writing, read back the first 80 characters of each text box to confirm the text landed:
 
```javascript
const shape = sheet.shapes.getItem("tbCommentADB");
shape.textFrame.textRange.load("text");
await context.sync();
return { preview: shape.textFrame.textRange.text.substring(0, 80) };
```
 
Fix any text boxes that still show placeholder content.
 
### Step 5 — Update Logs
 
Add a row to the Claude Log and Change Log: dashboard sheets updated, period covered, entity, key metrics used.
 
---
 
## CF4 Commentary Standards
 
Each commentary block follows this structure:
 
1. **Opening line** — Entity name · Dashboard type · Period (e.g. "YTD to May 2026, 11 months, FY2026")
2. **Paragraph 1** — Top-line performance: absolute $ and % vs prior year, primary driver summary
3. **Paragraph 2** — Key sub-drivers: revenue stream breakdown or expense category analysis; isolate non-recurring items explicitly (name them and state $)
4. **Paragraph 3** — Trend and context: recent months, quarterly pattern, seasonality, forward-looking observation
5. **Budget note** — If budget is all zeros, close with: *"No budget has been loaded; all variances shown reflect actual versus prior year performance only."*
**Length:** 3–4 paragraphs, ~200–280 words per text box.
 
**Tone:** Precise, objective, analytical. Dollar figures in $Xk or $X.XM as appropriate. Percentages to one decimal.
 
**Format conventions:**
- Dollar values: $X.XXM or $XXXk (match magnitude to context)
- Percentages: 0.0% format
- Month references: Mon-YY (e.g. May-26)
- Quarters: Q1 (Jul–Sep), Q2 (Oct–Dec), Q3 (Jan–Mar), Q4 (Apr–Jun) for July FY-start — adjust for other FY starts per Master Control
---
 
## Dashboard-Specific Guidance
 
**Actuals Dashboard** — High-level executive summary. Cover all three KPIs. Lead with the dominant story — revenue problem or cost problem? State EBITDA margin. Call out non-recurring items by name and amount.
 
**Combo Dashboard** — Broadest view. Cover P&L waterfall dynamics, quarterly performance, three-month trend. Note whether the business is accelerating or decelerating into year-end.
 
**Revenue Dashboard** — Identify the dominant revenue stream and its share of total. Note any streams down materially vs PY. Call out inactive streams (e.g. contracts that have wound down). Flag any lumpy or irregular items (grants, rebates, large project crystallisations).
 
**Expense Dashboard** — Lead with the largest cost category and its % of total expenses. Separate non-recurring items. Note any trending costs. If the monthly run-rate is stable, state the range.
 
**EBITDA Dashboard** — Calculate underlying EBITDA (excluding non-recurring items) alongside statutory EBITDA when they differ materially. State best and worst months. Comment on margin trend. Close with a year-end trajectory observation.
 
---
 
## Gotchas
 
- **`textFrame` cannot be bulk-loaded on shape collections** — load the shapes collection first (`items/name,items/type`), sync, then load `textFrame.textRange.text` per shape individually. Attempting `textFrame/textRange/text` in the initial `shapes.load()` call throws `InvalidOperation`.
- **Zero budget ≠ budget miss** — if all budget cells show 0, no budget has been loaded. Never describe zero-budget variances as performance indicators. Always state "No budget has been loaded" explicitly.
- **Revenue vs Sales are distinct account types** — EXL Cloud uses "Sales" for primary trading revenue and "Revenue" for other income streams (interest, rebates, non-operating). These map to separate rows in the Charts data. Do not aggregate them without distinguishing which is which in the commentary.
- **Non-recurring items distort monthly and YTD figures** — annual charges (Income Tax, WorkCover, Dividends, large insurance premiums, annual leave adjustments) typically hit specific months and create outlier results. Identify these in the P&L Pivot before writing any month-level commentary, then isolate them explicitly.
- **Shape names may differ in customised workbooks** — the `tbComment[XXX]` pattern is standard but may vary. Always enumerate shapes first if the workbook is a new engagement.
- **Prior year column is PYTD, not Budget** — in the Charts data Live Data section, the comparison column is prior year actuals labelled PYTD. Budget is a separate column and is often zero.
- **Project-based or equipment revenue streams are often lumpy** — streams like plant hire, materials recovery, or project-specific work create large month-to-month swings. Describe these as irregular rather than inferring a trend from a single month movement.
- **EBITDA may equal Net Profit** — if depreciation is minimal or not separately tracked in the workbook, EBITDA and Net Profit will be the same figure. Confirm from the P&L Pivot before claiming a depreciation-adjusted result.
- **FY start month drives quarter definitions** — Q1 starts in the FY start month (from Master Control). Do not assume July unless confirmed.
---
 
## Output Template
 
```
[Entity Name] — [Dashboard Title]: YTD to [Month Year] ([N] months, FY[XX])
 
[Paragraph 1 — revenue/expense/EBITDA KPIs with $ and % vs PY, dominant story in 3–4 sentences]
 
[Paragraph 2 — sub-drivers: revenue streams or expense categories, non-recurring items named and quantified]
 
[Paragraph 3 — trend context: recent months, quarterly profile, seasonality, forward-looking note]
 
No budget has been loaded; all variances shown reflect actual versus prior year performance only.
```

## Constraints

- Do NOT freeze panes, hide gridlines, or modify sheet view settings