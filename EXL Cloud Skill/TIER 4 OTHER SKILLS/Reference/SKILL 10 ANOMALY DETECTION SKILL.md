---
name: skill-10-exl-cloud-anomaly-detection
description: "Add a Validation Checks sheet to any workbook. Use when the user says 'add validation checks', 'build checks', 'add error detection', 'validation sheet', 'PASS FAIL checks', 'model integrity checks', 'add balance sheet check', 'audit checks', or invokes /anomaly-detection. Builds automated PASS/FAIL checks with conditional formatting. Independent of other skills. Do NOT use for model building, commentary, or data cleanup."
---

# Anomaly Detection

This skill adds a dedicated Validation Checks sheet to any workbook. The sheet contains automated checks that flag errors, imbalances, and data quality issues.

**Type:** Encoded Preference · Investigation / Triage

**Before outputting:** Re-check this sheet against skill-2 and skill-1.

## 1. When to Use

- Building a new financial model or report
- Inheriting a workbook that lacks integrity checks
- Month-end close process requiring sign-off validation
- Before distributing a workbook to stakeholders

## 2. Standard Checks
This catalogue is the single source of truth for all validation checks across the EXL Cloud skills library. Model-Specific checks (tagged below) apply only to EXL Cloud financial models — disable them via the Enabled column for other workbook types.
### Structural Checks
- Broken formulas and error values across all sheets (#VALUE!, #REF!, #NAME?, #DIV/0!, #N/A)
- Circular reference detection
- Blank rows or columns within data ranges
### Financial Integrity Checks
- Balance Sheet balances: Total Assets minus Total Liabilities minus Equity must be zero (every period)
- cash reconciliation: CFS closing cash must equal BS cash balance (every period)
- P&L net income matches retained earnings movement on BS
- Revenue recognition: total revenue on IS matches sum of revenue schedules
- Monthly cash balance check — no unintended negative cash
### Data Quality Checks
- period completeness: every period in tbl_Period has data
- Sign convention: expenses negative, revenue positive
- Year-to-date totals match sum of monthly figures
### Model-Specific Checks (EXL Cloud models only — Enabled = Yes)
- Time Series Date validation: chosen date is not in the future
- Historical data refresh status: date changed but data not refreshed
- Unmapped values in revenue, COGS, and expense categories (no orphan accounts)
- Staff setup completeness: termination dates valid, role names unique, setup complete
- Working Capital opening balance splits sum to 100%
- Working Capital profile percentage splits sum to 100%
- P&L pre vs post processing value check
## 3. Check Structure

Each check occupies one row with these columns:
- Check Number: sequential (C1, C2, C3...)
- check Name: descriptive label
on
- Actual: the calculated current state
- Variance: the absolute difference between Expected and Actual
- Status: PASS, FLAG, FAIL, or Not Applicable (set by the pattern shown below)
- Enabled: Yes/No toggle to deactivate checks that do not apply to this workbook type

### Enabled-Aware Formula Pattern
Each check's Status cell must use this pattern so checks can be deactivated without breaking the sheet:
IF(Enabled="No", "NA", IF(absolute_variance < Materiality, "PASS", "FAIL"))
Conditional formatting: PASS green, FAIL red, NA grey (#F2F2F2 fill, #808080 text).
## 4. Status Tiers

- PASS: variance is exactly zero (or within rounding tolerance of $0.01)
- FLAG: variance is non-zero but below materiality threshold (default $1,000)
- FAIL: variance exceeds materiality threshold
- Conditional formatting: PASS is green, FLAG is amber, FAIL is red

## 5. Gotchas

- Materiality threshold should be configurable — put it in a named cell, not hardcoded
- Some checks may legitimately FLAG during mid-month — the Enabled toggle lets users suppress these temporarily
- Error counts should exclude sheets that are intentionally blank or template sheets
- The BS balance check must account for rounding — use ABS(variance) < 1 rather than exact zero

## 6. Constraints

- This skill ADDS a sheet — it does not modify existing data
- Status cells must reference source cells, not hardcoded values
- Do NOT delete or overwrite an existing Validation Checks sheet — ask the user first
- Each check must have a meaningful name that a non-modeller can understand
- Do NOT freeze panes, hide gridlines, or modify sheet view settings
