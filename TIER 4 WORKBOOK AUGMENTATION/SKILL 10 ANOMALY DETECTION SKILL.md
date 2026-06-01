---
name: exl-cloud-anomaly-detection
description: "Add a Validation Checks sheet to any workbook. Use when the user says 'add validation checks', 'build checks', 'add error detection', 'validation sheet', 'PASS FAIL checks', 'model integrity checks', 'add balance sheet check', 'audit checks', or invokes /validation-checks-builder. Builds automated PASS/FAIL checks with conditional formatting. Independent of other skills. Do NOT use for model building, commentary, or data cleanup."
---

# Validation Checks Builder

This skill adds a dedicated Validation Checks sheet to any workbook. The sheet contains automated checks that flag errors, imbalances, and data quality issues.

**Before running:** Read foundation skill validation (exl-cloud-foundations) and confirm all outputs will meet EXL Cloud best practice standards.

## 1. When to Use

- Building a new financial model or report
- Inheriting a workbook that lacks integrity checks
- Month-end close process requiring sign-off validation
- Before distributing a workbook to stakeholders

## 2. Standard Checks

### Structural Checks
- Scan for broken formulas and error values across all sheets
- Circular reference detection
- Blank rows or columns within data ranges

### Financial Integrity Checks
- Balance Sheet balances: Total Assets minus Total Liabilities minus Equity must be zero
- Cash reconciliation: CFS closing cash must match BS cash balance
- P&L net income must match retained earnings movement on BS
- Revenue recognition: total revenue on IS matches sum of revenue schedules

### Data Quality Checks
- Period completeness: every period in tbl_Period has data
- Sign convention: expenses should be negative, revenue positive
- Year-to-date totals match sum of monthly figures

## 3. Check Structure

Each check occupies one row with these columns:
- Check Number: sequential (C1, C2, C3...)
- Check Name: descriptive label
- Expected: the target value (usually zero for balance checks)
- Actual: the calculated current state
- Variance: ABS(Expected minus Actual)
- Status: PASS (variance is 0), FLAG (below materiality), FAIL (above materiality)
- Enabled: Yes/No toggle to deactivate checks that do not apply

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
