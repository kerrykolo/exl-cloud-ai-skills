---
name: exl-cloud-best-practice-modelling
description: "Best practice financial modelling standards for the EXL Cloud platform. Use when the user says 'format my model', 'colour code my inputs', 'apply modelling best practice', 'fix my number formats', 'check my formula discipline', 'audit model structure', 'apply EXL Cloud standards', 'blue inputs black formulas', 'financial model formatting', 'build a validation sheet', or invokes /exl-cloud-best-practice-modelling. Covers colour coding, number formats, font standards, formula rules, tab structure, validation checks, and output statement layouts. Do NOT use for building calculation engines or proprietary modelling logic."
---

# EXL Cloud Best Practice Financial Modelling Standards

These standards define the formatting, structure, and convention rules for financial models built in the EXL Cloud platform. Follow these when building, modifying, or auditing any Excel model to ensure professional, consistent, auditable output.

**Before running:** Read foundation skill validation (exl-cloud-foundations) and confirm all outputs will meet EXL Cloud best practice standards.

## 1. Colour Coding Standards

Every cell must be coloured according to its function. This is non-negotiable.

### Cell Legend
- Blue text (hex 0070C0) on light green bg (hex E2E9DA): Input — assumption input cells users will change
- Blue text (hex 0070C0) on medium green bg (hex ABBB93): Input with calc — linked cells (e.g. from Scenario Manager)
- Blue text (hex 0070C0) on no fill: Blank input — input cell with no current impact
- Black text, no special fill: Output — calculated values and workings
- Dark green bg (hex 375623), white bold text: Section headers

### Colour Palette (Wisp Theme)
- Dark green (primary brand): hex 375623 — section headers, dark header backgrounds
- Medium green: hex ABBB93 — secondary headers, alternating row fills
- Light green: hex E2E9DA — Light background fills, input area highlights
- Blue (inputs): hex 0070C0 — all hardcoded input cells and assumption values
- Red (alerts/links): hex FF0000 — external links, error flags, warning values
- Black: ALL calculation/output cells
- White: header text on dark backgrounds

## 2. Font Standards

- Primary font: Arial, 9pt for all body content
- Display font: Century Gothic, 9-11pt (sparingly, for titles and branding only)
- VBA/button font: Tahoma, 9pt bold

## 3. Number Format Standards

Apply these formats consistently across all models:

- Currency (whole): use custom format with thousands separator, parentheses for negatives, dash for zeros
- Dollar sign: prefix with $ symbol using custom format
- Percentage (1dp): 0.0% format
- Multiples: display with 'x' suffix (e.g. 9.0x) using custom format
- Dates (headers): mmm yy — displays: Jan 23
- Dates (full): d mmmm yyyy — displays: 1 January 2023
- Zeros: Always display as "-" using custom format (including for percentages)
- Negative numbers: Use parentheses (1,234) not minus sign -1,234
- Sign convention: Revenue positive, Expenses negative (natural sign)

## 4. Tab Structure & Naming

Tabs follow a strict numbered hierarchy. Every tab begins with a section number and descriptive name:

- Instructions: User guide, cell legends, step-by-step instructions
- Master Control: Central parameters, date settings, entity selection
- 1.x: Outputs & Dashboards — Executive Summary, Scenario Dashboard, Annual Financials, Management Report
- 2.x: Data Input & Processing — Date setup, raw data, processed data
- 3.x: Mapping & Checks — COA mapping, mapped outputs, value checks, logic selectors
- 4.x-12.x: Calculation Engines — Revenue, COGS, Expenses, Salaries, Working Capital, Assets, Capital, Tax, Other Items
- 13: Output — Consolidated monthly 3-way financial statements (IS, BS, CFS)
- 14-17: Dashboard Data Feeds — Monthly, annual, scenario, combo data for dashboards
- 18: Chart of Accounts — Master COA for P&L and Balance Sheet
- 19: Helper — Lookups, named ranges, dropdown lists, parameters
- 20: Validation Checks — All model error and integrity checks
- Change Log: Audit trail of all changes made to the workbook

## 5. Time Series Layout

All calculation sheets with time series data follow this layout:

- Columns A-G: Reserved for labels, categories, and helper columns
- Column H onwards: Time series data (one column per period)
- Row 1: Reserved (hidden helper row)
- Rows 2-8: Sheet header block (model name, sheet title, brand, active scenario, error messages)
- Rows 9-18: Time series header block (start dates, end dates, days in period, counters, year, FY, ACT/FCST flag)
- Row 18: Forecast Flag — marks each column as ACT (actual/historical) or FCST (forecast)
- Data rows: Begin at row 19 or below, depending on the sheet

## 6. Workings and Calculation Rules

These rules ensure every model is auditable and trustworthy:

- never hardcode values in calculation cells — ALL assumptions must live in dedicated input cells coloured Blue
- All input cells must be clearly visible: blue text on light green or medium green background
- Assumptions must NOT be comingled within workings — keep them in a separate, clearly labelled section
- Use IFERROR() and IFNA() wrapping for all lookup-based workings
- Use IF(denominator=0, 0, numerator/denominator) to prevent DIV/0 errors
- Cross-sheet references must use sheet name in single quotes: 'Sheet Name'!CellRef
- Use structured table references where tables exist: tbl_Period[Column], not fixed A1 ranges
- Circular references are not permitted — use iterative workarounds if needed
- Keep workings simple and auditable — prefer helper rows over deeply nested expressions
- Avoid volatile or complex functions (e.g. those that build references from text strings) unless specifically required
- Break complex logic into intermediate steps so each step is traceable
- Use EOMONTH for all date calculations — never add or subtract fixed day counts

## 7. Hardcoded Values — What's Allowed vs Prohibited

### Prohibited (will fail an audit)
- Business assumptions embedded directly in workings (tax rates, growth rates, margins as magic numbers)
- Computed values typed directly into cells instead of being derived by a calculation
- Data from another sheet copy-pasted as values instead of linked with a cell reference
- Overwriting a live calculation cell with a hardcoded value to force a specific output

### Allowed (no issue)
- Designated input/assumption cells clearly labelled and coloured blue
- True constants: multiply by 12 (months per year), divide by 100 (percentage conversion)
- Initial seed values (first value in a series where no prior cell exists)
- Small lookup tables in a clearly labelled reference range

## 8. Validation Checks System

Every model must include a Validation Checks sheet with a comprehensive error-checking engine:

- Total error count displayed prominently
- Error message string displayed on every sheet header (so users see issues immediately)
- Individual checks returning PASS (0) or FAIL (1)

### Minimum Required Checks
1. Time Series Date validation (is the chosen date in the future?)
2. Historical data refresh status (has date been changed but data not refreshed?)
3. Unmapped values in revenue, COGS, and expense categories
4. Staff setup completeness (termination dates, role names unique, setup complete)
5. Working Capital opening balance splits sum to 100%
6. Working Capital profile percentage splits sum to 100%
7. Balance Sheet balance check — Assets = Liabilities + Equity (every period)
8. Cash Flow reconciliation — CFS closing cash = BS cash (every period)
9. P&L pre vs post processing value check
10. Monthly cash balance check (no unintended negative cash)

## 9. Output Statement Structure

The model's output tab (13. Output) produces three linked financial statements:

### Income Statement (P&L)
Sales Revenue (detail lines) > Total Sales Revenue
Cost of Goods Sold (detail lines) > Total COGS
Gross Margin / Gross Margin (%)
Other Revenue (detail lines) > Total Other Revenue
Salaries and Wages (base + payroll tax + leave) > Total S&W Expense
Operating Expenses (Advertising, Insurance, Licenses, Marketing, OpEx, Prof Fees, Rent, SG&A, Travel, Utilities) > Total Expenses
Other Expenses > Total Other Expenses
EBITDA / EBITDA Margin (%)
Depreciation / Amortisation > EBIT
Interest Expense / Income > EBT
Tax Expense > NPAT / NPAT Margin (%)

### Balance Sheet
Current Assets: Cash, Debtors, Inventory, Prepayments, Other Current Assets
Non-Current Assets: Fixed Assets (net of Acc Depn), Intangible Assets (net of Acc Amort), Other Non-Current Assets
Total Assets
Current Liabilities: Creditors, Accrued Expenses, Current Debt, Other Current Liabilities
Non-Current Liabilities: Non-Current Debt, Other Non-Current Liabilities
Total Liabilities
Equity: Share Capital, Retained Earnings, Current Year Earnings
Total Equity > Total Liabilities + Equity
Balance Check (must = 0 every period)

### Cash Flow Statement
Operating Activities: NPAT + Depreciation + Amortisation + Working Capital movements
Investing Activities: Asset purchases/disposals
Financing Activities: Debt drawdowns/repayments + Equity injections + Dividends
Net Cash Movement
Opening Cash Balance > Closing Cash Balance
Cash Balance Check (Closing Cash = BS Cash, must = 0)

## 10. Table & Structured Reference Standards

- ALL Excel tables use the prefix tbl_ followed by a descriptive name (e.g. tbl_Period, tbl_PL_COA, tbl_SW_Role)
- Tables use the model's green colour scheme (dark green headers, alternating light green rows)
- Header row: Bold white text on dark green (hex 375623) background
- Data validation dropdowns reference Helper sheet lists
- Use structured references (tbl_Period[Column]) instead of fixed ranges wherever a table exists

## 11. Key Modelling Reminders

1. Always start time series data from column H
2. Row 1 is reserved for hidden helper data
3. Never delete or rename core named ranges
4. Sign convention: Revenue positive, Expenses negative
5. All growth rate drivers must reference the Scenario Manager — never hardcode growth rates
6. Balance sheet must balance every period — include balance check rows
7. Cash flow must reconcile — CFS closing cash must equal BS cash
8. All sheets must display the error message bar linked to the global error string
9. Financial Year adjustment: Year + IF(MONTH(date) > FY_MonthEnd, 1, 0)
10. Use EOMONTH for all date calculations
11. Default currency: AUD (configurable in Master Control)
12. Default accounting basis: Accrual (configurable)

## 12. Gotchas

- Forgetting to colour input cells blue is the most common audit failure — check every assumption cell
- Using fixed A1 ranges instead of structured table references means your workings break when rows are added
- Zeros displayed as '0' instead of '-' look unprofessional — always apply the dash-for-zero custom format
- Negative numbers shown with minus sign instead of parentheses breaks financial convention
- Missing Balance Check rows mean imbalances go undetected for months
- Comingling assumptions inside workings makes it impossible to run scenario analysis
- Complex nested functions look impressive but erode confidence — simpler helper rows are always preferred
