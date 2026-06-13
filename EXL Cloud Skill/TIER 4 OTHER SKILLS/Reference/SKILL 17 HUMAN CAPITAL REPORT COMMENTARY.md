---
name: skill-17-exl-cloud-human-capital-report-commentary
description: Use when user says 'build HC analysis', 'human capital report', 'payroll executive summary', 'workforce commentary', 'HC commentary', 'salary data analysis', 'headcount analysis', 'workforce snapshot', 'staffing cost summary', 'build the HC exec summary', 'analyse the payroll data', 'human capital executive summary', or invokes /human-capital-report-commentary. Builds a 5-section Human Capital Analysis executive summary sheet from EXL Cloud payroll data Workforce Snapshot, Cost Summary, Anomalies & Flags, Tenure Analysis, and CFO Decision Points — with formula-linked charts, validation checks, and change log. Do NOT use for monthly P&L commentary, cash flow analysis, or non-payroll workforce planning.
---
 
# Human Capital Report Commentary
 
Builds a formula-driven Human Capital Analysis executive summary from EXL Cloud Salaries & Wages data. Reads payroll tables, computes workforce metrics, flags data quality anomalies, and produces a CFO-ready insight sheet with charts and validation checks.
 
**Skill type**: Encoded Preference (encodes the HC analysis methodology and layout standards)

**Functional category**: Workflow automation
 
**Before outputting:** Verify this sheet against skill-2 and skill-1 foundations.

## When to Use
 
- User asks for a human capital or workforce analysis on an EXL Cloud model
- User wants a payroll executive summary, headcount snapshot, or salary gap analysis
- User asks to analyse the S&W (Salaries & Wages) component for completeness or anomalies
- The model contains payroll data (tPayroll, tPayrollAdjustment) and needs a diagnostic review

## Context Required
 
Before building, read these sheets to understand the model's payroll setup:
 
1. **Master Control** — Forecast start date, FY end month, number of periods, entity name(s)
2. **2.a. Data - Payroll** (tPayroll table) — Employee records: name, status, job title, start/termination dates, base annual salary, super type/amount/percentage
3. **2.b. Data - PayrollAdjustment** (tPayrollAdjustment) — Adjusted salaries, rate changes
4. **3.f. FTE Restructure** (tRestructure) — Planned hires, departures, role changes
5. **4.a. Summary** or **3.g. SW Calc_Summary** — Monthly cost outputs (base salary, super, STI, bonus, payroll tax)
If any table is missing or empty, note it as a data gap — do not skip the analysis.
 
## Workflow
 
### Step 1: Data Discovery & Extraction
 
1. Read Master Control for entity name, forecast period, and number of periods
2. Read tPayroll — filter to ACTIVE and TERMINATED status counts
3. For active staff, extract: name, job title, start date, base annual salary, super details
4. Count active staff WITH salary (>$0) vs WITHOUT ($0 or blank)
5. Read 4.a Summary row structure to identify cost line references (Base Salary, Super, STI, Bonus, Payroll Tax, Total)
6. Check if tRestructure has any data or is empty
7. Use Python (code_execution) for accurate counting and classification — do not estimate in chat
**Done when**: You have total headcount (active/terminated), salary coverage counts, and cost line cell references.
 
### Step 2: Role Classification
 
Classify active staff into role categories using job title keywords. Use COUNTIFS formulas where possible; use blue hardcoded input cells with classification notes where title parsing is too complex for formulas.
 
Standard category hierarchy (adapt to the business):
- Management / Admin — GM, director, manager, admin, bookkeeper
- Supervisor / Senior — supervisor, senior, principal
- Team Leader — team leader, works leader, coordinator
- Specialist roles — use industry-specific categories (e.g., NRM Team, Civil Team for environmental services)
- Field Worker / Operative — labourer, crew, team member, operative
- Unclassified — blank or missing job titles
**Done when**: Every active employee is assigned to exactly one category, and category counts sum to total active headcount.
 
### Step 3: Tenure Analysis
 
Calculate tenure from start date to the current date. Use DATEDIF or manual date arithmetic in formulas. Group into bands:
- < 1 year
- 1–2 years
- 2–5 years
- > 5 years
Compute: average tenure, median tenure, min/max. Flag long-service staff (>5 years) who have $0 salary — these represent LSL liability risk.
 
**Done when**: Tenure band counts sum to total active headcount, and stats are formula-linked.
 
### Step 4: Build the Executive Summary Sheet
 
Create sheet named `5. Exec Summary - HC Analysis`. Structure in 5 sections with a blank column A spacer:
 
#### Section 1 — Workforce Snapshot (rows ~5–24)
| Content | Columns |
|---------|---------|
| Headline metrics | B:G — Total staff, Active, Terminated, Total annual salary, Avg salary |
| Turnover ratio | H — commentary flag |
| Data Quality sub-table | Active with salary vs without, as count and % |
| Role Category breakdown | Category, Count, % of Active, Annual Salary, % of Total Salary |
| Cross-check row | Category total = Active headcount (formula, with PASS/FAIL text) |
 
**All counts must use COUNTIFS/SUMIFS referencing tPayroll.** Role category counts may be blue inputs if job title parsing requires manual classification — add cell notes explaining the logic.
 
#### Section 2 — Cost Summary (rows ~26–40)
| Content | Columns |
|---------|---------|
| Cost table | Component (Base Salary, Super, STI, Bonus, Payroll Tax, Total), Monthly, Annual, 5-Year, % of Total |
| Cross-check | Total ties to 4.a Summary (formula with PASS/FAIL) |
| Gap Analysis sub-table | Estimated true payroll (upper + conservative bounds), missing cost, model capture % |
 
**Every cost cell must be a formula referencing 4.a Summary or tPayroll.** Gap analysis uses assumption cells (blue font, yellow fill) for benchmark rates.
 
#### Section 3 — Anomalies & Data Quality Flags (rows ~42–53)
| Content | Columns |
|---------|---------|
| Numbered anomaly rows | #, Severity (CRITICAL/HIGH/MEDIUM/LOW), Category (merged D:G), Description & Impact (H) |
 
Standard anomaly checks (include all that apply):
1. Missing salary data — count and % of active staff at $0
2. Flat forecast — identical cost across all periods (no wage escalation)
3. Missing STI/bonus/commission — zero across forecast
4. Empty FTE Restructure — no workforce planning modelled
5. Missing job titles — staff with no title in payroll
6. No payroll tax — check against state threshold
7. High turnover — terminated > active headcount
8. Long-service at $0 — LSL liability not accruing
9. Salary concentration — top N staff as % of total base
Severity guide: CRITICAL = >50% data gap or material misstatement. HIGH = missing cost component. MEDIUM = data quality. LOW = concentration/structural risk.
 
#### Section 4 — Tenure Analysis (rows ~55–67)
| Content | Columns |
|---------|---------|
| Tenure band table | Band, Count, % of Active, Avg Salary, Commentary |
| Cross-check | Band total = Active headcount (formula with PASS/FAIL) |
| Stats block | Avg tenure, Median, Min, Max, Turnover ratio |
 
#### Section 5 — CFO Decision Points (rows ~69–77)
| Content | Columns |
|---------|---------|
| Prioritised action rows | Priority (P1–P6), Action (merged C:D), Impact, Rationale (H) |
 
Priority order: P1 = highest impact data gap, P2 = wage escalation, P3 = workforce planning, P4 = tax obligation, P5 = incentive design, P6 = data cleanup.
 
**Done when**: All 5 sections built, all cross-checks show PASS, all formulas evaluate without errors.
 
### Step 5: Charts
 
Add two charts below all data content (never overlapping data rows):
1. **Bar chart** — Active headcount by role category
2. **Pie chart** — Tenure distribution by band
Position side-by-side. Source data from the section tables. Standard chart size: ~440×300px each.
 
### Step 6: Formatting
 
Apply EXL Cloud house style:
- **Section headers**: Dark green (#1A3C2E) fill, white bold 12pt text
- **Sub-headers**: Medium green (#2D6A4F) fill, white bold 10pt
- **Table headers**: Light green (#4A7C59) fill, white bold 10pt, centre-aligned (except B and H left-aligned)
- **Total rows**: Light green (#D8F3DC) fill, bold, top border thin, bottom border double
- **Critical flag rows**: Red fill (#FFC7CE), dark red font (#9C0006) on commentary
- **Alternating row banding**: #F5F5F5 on even data rows
- **Borders**: Thin #D0D0D0 inside, #808080 outside on all data tables
- **Font**: Arial throughout
- **Alignment**: Left-align labels (B) and commentary (H), right-align numbers (C–G)
- **Number formats**: $#,##0 for currency, 0.0% for percentages
- **Column widths**: A=8 (spacer), B≥200 (labels), C–G=80–100 (data), H≥240 (commentary), I=8 (spacer)
- **Row heights**: Standard 16pt for data, 24pt for section headers, 20pt for table headers, 8pt for spacer rows, 32pt for wrapped text rows (CFO actions)
### Step 7: Commentary Writing Standard
 
Column H commentary must be **concise single-line flags** (max ~50 characters). Never write multi-sentence paragraphs in commentary cells.
 
Good: `"⚠ 89% at $0 — model severely understated"`
Bad: `"58 of 65 active staff (89%) show $0 salary. The model captures only 7 salaried roles totalling $822k pa. Entire field workforce..."`
 
Use emoji severity prefixes: ⚠ for warnings, 🔴 for critical, 🟡 for high, 🟠 for medium, 🟢 for low.
 
### Step 8: Validation Checks
 
Add to the model's existing Validation Checks sheet:
1. Role category total = Active headcount
2. Tenure band total = Active headcount
3. Cost total ties to 4.a Summary
4. Flag if >50% active staff at $0 salary
5. Flag if forecast is flat (period 1 = period 60)
6. Flag if STI + Bonus + Payroll Tax all zero
7. Flag if FTE Restructure is empty
Use the existing check format (description in B, formula returning 1=FAIL/0=PASS in the check column).
 
### Step 9: Logs
 
Update Change Log and Claude Log with details of the build.
 
## Gotchas & Edge Cases
 
- **Commentary length is the #1 formatting killer.** Multi-paragraph text in column H makes rows 100+ pixels tall and the sheet unreadable. Always use concise single-line flags. This was the primary failure in the initial build.
- **Column widths must be set explicitly.** Default widths cause ## on numbers and truncation on labels. Set B≥200pt, data columns 80-100pt, H≥240pt.
- **Charts MUST be positioned below all data rows.** If placed mid-sheet they overlap content and hide data. Calculate chart top position from the last data row.
- **Role classification can't always be formula-driven.** Job titles are messy (inconsistent naming, missing titles). Use blue hardcoded input cells with cell notes explaining classification logic. Always verify category total = active headcount.
- **Gap analysis assumptions need source citations.** When estimating missing salary costs using industry benchmarks, document the benchmark rate and basis in cell notes. Use blue font + yellow fill for assumption cells.

- **Merge cells for long text in Sections 3 and 5.** Anomaly categories (D:G) and CFO actions (C:D) need merged cells to display full text. Don't rely on column width alone.
- **Cross-check formulas use IF().** Pattern: `=IF(C24=C9,"PASS: [description]","FAIL: [detail]")` — returns readable PASS/FAIL text, not just 0/1.
- **tPayroll may include more than 2 statuses.** Check for CASUAL, CONTRACTOR, or other status values beyond ACTIVE/TERMINATED.
- **Super type varies.** STATUTORY = percentage-based (usually 12%), FIXEDAMOUNT = dollar top-up. Some staff have both. Handle each in the analysis.
- **Date serials in payroll.** Start and termination dates are Excel serials. Convert using `datetime(1899,12,30) + timedelta(days=serial)` in Python for accurate tenure calculations.
- **The 4.a Summary sheet structure varies by model.** Don't hardcode row references — read the row labels first to find Base Salary, Super, STI, Bonus, Payroll Tax, and Total rows dynamically.

## Constraints

- Do NOT freeze panes, hide gridlines, or modify sheet view settings