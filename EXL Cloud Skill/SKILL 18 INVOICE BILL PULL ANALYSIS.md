---
name: skill-18-exl-cloud-invoice-bill-pull-analysis
description: Use when user says 'analyse the invoice pull', 'build the bill analysis', 'invoice and credit note analysis', 'what does this Xero pull show', 'build the exec summary for this data', 'analyse the billing data', 'invoice pull dashboard', 'break down the invoices', or invokes /invoice-bill-pull-analysis. Builds a 4-sheet analysis suite (Calculations, Executive Summary, Dashboard, Validation Checks) from any Xero-sourced Invoice & Credit Note data pull workbook containing a defined table. Discovers table structure, aggregates by tracking categories, account codes (Revenue/COS/OpEx/Tax/BS), contacts, and monthly time series using SUMIFS with structured references. Produces narrative insights and 12+ cross-footing validation checks. Do NOT use for bank transaction analysis (use fr-cashflow-bank-recon), trial balance reconciliation (use tb-reconciliation-workbook), or monthly P&L commentary.
---
 
# Invoice & Bill Pull Analysis
 
Builds a comprehensive 4-sheet analysis suite from any Xero-sourced Invoice & Credit Note data pull workbook. Produces a formula-driven Calculations engine, one-page Executive Summary with narrative insights, Dashboard with charts, and Validation Checks with cross-footing — all linked to the source data table via structured references.

**Before outputting:** Verify this sheet against skill-2 and skill-1 foundations.

## When to Use
 
- User has a workbook with a Xero Invoice & Credit Note data pull (EXL Cloud Data Pull or similar)
- The workbook contains a defined Excel table (e.g., `tIBCN`) with line-item-level invoice data
- User asks to analyse, summarise, dashboard, or understand the invoice/bill data
- User wants to identify spend patterns, revenue concentration, data quality issues, or anomalies
## Prerequisites
 
- Workbook must contain a defined table with invoice line items
- Expected columns (names may vary): Item Type, Contact Name, Status, Date, Line Item Line Amount, Line Item Account Code, Line Item Description, Tracking Category columns (Division, Activity, or custom)
- The Master Control or cover sheet should indicate the entity name and date range
## Workflow
 
### Step 1: Investigate Workbook Structure
**Done when:** Table name, column map, row count, date range, and dimension list are known.
 
1. Read Master Control / cover sheet to identify entity name, data pull type, and date range
2. Identify the defined table name and sheet location (check sheet metadata for Tables)
3. Read header row to build a complete column-index map (column letter → field name)
4. Count total rows via `ROWS(tableName)` or sheet dimension
5. Identify which columns serve as:
   - **Item Type** (IN/CR/OP/PP) — invoice vs credit note classification
   - **Contact Name** — customer or supplier
   - **Date** — invoice date for time series
   - **Line Item Line Amount** — the monetary value per line
   - **Line Item Account Code** — for revenue/cost classification
   - **Tracking Category 1 Option** — first dimension (e.g., Division, Region, Project)
   - **Tracking Category 2 Option** — second dimension (e.g., Activity, Service Type)
   - **Status** — PAID, AUTHORISED, DRAFT, etc.
### Step 2: Discover Unique Dimension Values
**Done when:** Exact unique values for every dimension (tracking categories, account codes, item types, statuses) are confirmed.
 
Use `execute_office_js` to scan unique values in each dimension column. This is CRITICAL — never assume dimension values; they may contain:
- Typos and abbreviations (e.g., "Ovehead" for Overhead, "Tree Mngt" for Tree Management)
- Mixed case or trailing whitespace
- Blank/empty values (track as "Untagged")
For large datasets (>5000 rows), read in chunks of 2000 rows via execute_office_js using `getRangeByIndexes()`.
 
Store discovered values in blobs for reference during formula writing.
 
### Step 3: Classify Account Codes
**Done when:** Account code prefixes are mapped to categories and the category gap is quantified.
 
Build a classification scheme based on the account code patterns in the data:
 
| Prefix Pattern | Typical Category | Notes |
|---------------|-----------------|-------|
| Revenue codes (e.g., 200*, 2xx sales) | Revenue | Sales, service income, labour income |
| COS codes (e.g., 219*, 234*) | Cost of Sales | Materials, waste, direct costs |
| Expense codes (3xx, 4xx, 5xx) | Operating Expenses | Fuel, hire, subcontractors, rent |
| Tax/statutory (8xx, 9xx) | Tax / Statutory | PAYG, GST, super, income tax |
| Asset/liability codes (6xx, 7xx) | Balance Sheet | Plant, loans, provisions |
| Blank | Unclassified | Flag for data quality |
 
Verify by summing all categories vs grand total — the gap quantifies uncaptured codes.
 
### Step 4: Build "5. Calculations" Sheet
**Done when:** All 6 sections are built with formulas, all totals tie, and column widths are set.
 
Create a new sheet named with a sequential number prefix matching the workbook convention.
 
**Section 1: Key Metrics (rows ~5-18)**
- Total Line Items: `=COUNTA(table[Line Item Line Amount])`
- Total Line Amount: `=SUM(table[Line Item Line Amount])`
- Breakdown by Item Type: `=SUMIFS(table[Amount],table[Item Type],"IN")` etc.
- Unique Contacts: `=SUMPRODUCT(1/COUNTIF(table[Contact Name],table[Contact Name]))`
- Date range: `=MIN(table[Date])` / `=MAX(table[Date])`
- Status breakdown: PAID, AUTHORISED, DRAFT totals
**Section 2: Tracking Category 1 Breakdown (e.g., Division)**
- One row per unique value discovered in Step 2, plus "Untagged (blank)"
- Columns: Label | Line Items (COUNTIFS) | Total Amount (SUMIFS) | % of Total | Avg per Month
- Total row with SUM formulas
- Use exact string values from Step 2 as labels (even if they contain typos)
**Section 3: Tracking Category 2 Breakdown (e.g., Activity)**
- Same structure as Section 2 but for the second tracking dimension
**Section 4: Top 20 Account Codes**
- Sorted by absolute amount descending
- Columns: Account Code | Category (Revenue/COS/OpEx/Tax/BS) | Line Items | Total Amount | % of Total
- Write account code labels as blue-font assumption cells
- Category assigned manually based on Step 3 classification
**Section 5: Monthly Time Series**
- One column per month across the date range
- Rows: Total Amount, Revenue, COS, OpEx, Tax/Statutory, BS, Unclassified, Line Item Count, Invoices (IN), Credits (CR)
- Each cell: `=SUMIFS(table[Amount],table[Date],">="&DATE_START,table[Date],"<"&DATE_END)` pattern
- Date headers formatted as `mmm yy`
- TOTAL column at far right with `=SUM()` across all months
**Section 6: Top 20 Contacts**
- Sorted by absolute amount descending
- Columns: Contact Name | Type (Customer/Supplier/Tax Authority — classified by account code patterns) | Line Items | Total Amount | % of Total
### Step 5: Build Executive Summary Sheet
**Done when:** KPIs, summary tables, account categories, and narrative insights are all present and formula-linked to Calculations.
 
**Layout:**
1. Title bar (entity name + date range + period count)
2. KPI metrics row (Total Invoiced, Line Items, Unique Contacts, Revenue, COS, OpEx, Avg Monthly)
3. Tracking Category 1 table — formula-linked to Calculations section
4. Tracking Category 2 table — formula-linked to Calculations section
5. Top 20 Contacts table — formula-linked with Type classification
6. Account Category summary (Revenue/COS/OpEx/Tax/BS/Unclassified with % and Avg/Month)
7. Key Insights & Observations — 4-6 numbered narrative paragraphs covering:
   - Business profile and primary client/service relationships
   - Revenue concentration risk (single-client dependency, top contact share)
   - Data quality issues (untagged tracking categories as % of total, spelling inconsistencies)
   - Cost structure highlights (top expense categories, key supplier relationships)
   - Key activities/services by value (what work was done)
   - Any anomalies (credit notes, unusual patterns, status distribution)
### Step 6: Build Dashboard Sheet
**Done when:** 4 charts are created, all sourced from formula-linked data tables on the Dashboard sheet.
 
Build local source tables on the Dashboard sheet that reference Calculations, then chart from those tables:
 
1. **Monthly Trend Chart** (clustered column) — Total, Revenue, COS+OpEx by month
2. **Tracking Category 1 Chart** (horizontal bar) — Amount per division/region
3. **Tracking Category 2 Chart** (horizontal bar) — Amount per activity/service type
4. **Top 15 Contacts Chart** (horizontal bar) — Amount per contact
Chart formatting: titles bold, axis format `$#,##0,K`, legend at bottom for multi-series.
 
### Step 7: Build Validation Checks Sheet
**Done when:** All checks have formulas, status is PASS/FLAG/FAIL with colour coding, and notes explain any non-PASS items.
 
Standard check structure:
| # | Check Name | Expected | Actual | Variance | Status |
 
**Required checks (minimum 12):**
1. Grand Total: Calculations ties to source `=SUM(table[Amount])`
2. Tracking Category 1 total ties to grand total
3. Tracking Category 2 total ties to grand total
4. Monthly time series total ties to grand total
5. Account category total ties to grand total (will likely FLAG/FAIL — see Gotchas)
6. IN + CR + OP + PP = Grand Total
7. TC1 line item count = total table rows
8. TC2 line item count = total table rows
9. Untagged TC1 % below 50% threshold
10. Untagged TC2 % below 50% threshold
11. Credit notes < 5% of invoiced total
12. No formula errors in Calculations sheet
**Status logic:**
- PASS: Variance < $1 (amounts) or exactly zero (counts)
- FLAG: Variance < $1,000 (amounts) or < 100 (counts) — immaterial
- FAIL: Above thresholds
Apply colour formatting: PASS = green (#C4DDD2 fill, #006100 font), FLAG = amber (#FFEB9C fill, #9C6500 font), FAIL = red (#FFC7CE fill, #9C0006 font).
 
Add summary counts (PASS/FLAG/FAIL) and notes section explaining each non-PASS check.
 
### Step 8: Update Logs and Finalise
**Done when:** Change Log and Claude Log updated, freeze panes applied as LAST step, tab colours set.
 
1. Update Change Log with build summary
2. Update Claude Log with turn details
3. Set tab colours (Calculations=green, Exec Summary=dark green, Dashboard=forest green, Validation=red)
4. Set freeze panes on each new sheet (LAST step — after all data is written)
5. Position admin sheets (Change Log, Claude Log) at end of workbook
## Output Structure Summary
 
```
5. Calculations         — 6 sections: KPIs, TC1, TC2, Account Codes, Monthly Series, Top Contacts
6. Executive Summary    — KPIs + 4 tables + Account Categories + Narrative Insights
7. Dashboard            — 4 charts with source tables
8. Validation Checks    — 12+ checks with PASS/FLAG/FAIL + notes
Change Log              — Build entry
Claude Log              — Turn details
```
 
## Gotchas & Edge Cases
 
1. **copyToRange in set_cell_range copies ALL cells in the call, not just the intended row.** If you write headers AND a data row in the same call and use copyToRange, the entire block (headers + data) tiles across the destination. Fix: write headers in one call, data pattern + copyToRange in a separate call, or use `autoFill()` via execute_office_js.
2. **Column index counting from table start.** When reading via execute_office_js `getRangeByIndexes()`, index 0 is the first column of the range, not column A. Map carefully: if the table starts at column B, then `row[0]` = B, `row[1]` = C, etc. Off-by-one here silently produces wrong aggregations.
3. **Tracking category values may contain typos.** ALWAYS discover exact unique values from the data before writing SUMIFS. Never assume clean spelling — "Ovehead", "Revegatation", "Tree Mngt" are real examples. Use the exact misspelled value in the formula; note the typo in the Executive Summary narrative as a data quality observation.
4. **Account code wildcard classification always leaves a gap.** Patterns like `200*`, `3*`, `4*` won't capture 100% of account codes. Some codes use non-standard prefixes or are blank. Build the validation check (#5) to quantify this gap. A 1-2% gap is expected and should be noted, not forced to zero.
5. **COUNTA vs ROWS for line item count.** `COUNTA(table[column])` excludes blank cells; `ROWS(table)` counts all rows including blanks. Use ROWS for total row count and COUNTA for "items with values." The difference flags data completeness issues.
6. **Large datasets (>5000 rows) must be chunked.** Reading 20K+ rows in a single `execute_office_js` call can timeout. Read in chunks of 2000 rows using `getRangeByIndexes()`. For aggregation, prefer writing SUMIFS formulas (which Excel evaluates natively) over computing in JS.
7. **Date serials in headers and charts.** Xero exports store dates as Excel serial numbers. Always apply `numberFormat: "mmm yy"` to date cells. Never leave raw serials (e.g., 46168) visible.
8. **Unique contact count formula fragility.** `SUMPRODUCT(1/COUNTIF(...))` breaks if any contact name is blank (division by zero). Wrap with IFERROR or filter out blanks: `=SUMPRODUCT((table[Contact]<>"")/COUNTIF(IF(table[Contact]<>"",table[Contact]),IF(table[Contact]<>"",table[Contact])))`.
9. **Status column may contain unexpected values.** Beyond PAID/AUTHORISED/DRAFT, Xero can export VOIDED, DELETED, or blank statuses. Discover all unique statuses in Step 2 and account for them in the KPI section.
10. **Workbook may lose focus during multi-sheet builds.** If multiple workbooks are open, `freezePanes` and `worksheet.activate()` calls fail with `InactiveWorkbook`. Always attempt freeze panes LAST, and if they fail, flag as a pending item for the user to retry when the workbook has focus.