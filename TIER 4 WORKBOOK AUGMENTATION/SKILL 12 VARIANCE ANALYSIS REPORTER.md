---
name: exl-cloud-variance-analysis-reporter
description: "Build variance analysis comparing actuals to budget/forecast/prior. Use when the user says 'variance analysis', 'actual vs budget', 'compare to forecast', 'what changed vs last year', 'variance report', 'budget variance', 'explain the variances', or invokes /variance-analysis-reporter. Builds structured comparison sheet with dollar and percentage variances. Feeds Commentary and Board Paper. Do NOT use for commentary text, board paper assembly, or data cleanup."
---

# Variance Analysis Reporter

This skill builds a structured variance analysis sheet that compares actual financial results against budget, forecast, or prior period data. It calculates dollar and percentage variances, flags material items, and provides the data foundation for management commentary.

**Before running:** Read foundation skill validation (exl-cloud-foundations) and confirm all outputs will meet EXL Cloud best practice standards.

## 1. When to Use

- Monthly or quarterly close process
- Budget performance reviews
- Forecast accuracy assessment
- Year-on-year trend analysis

## 2. Report Layout

Columns: Line Item | Actual | Budget | Variance ($) | Variance (%) | Fav/Unfav | Prior Year | YoY Variance ($) | YoY Variance (%)

### P&L Section
- Revenue lines (by stream or total)
- Cost of Goods Sold
- Gross Profit + Gross Margin %
- Operating expenses (by category)
- EBITDA + EBITDA Margin %
- Net Profit After Tax

### YTD Section
- Same structure as monthly, but cumulative year-to-date
- Full-year budget remaining (Budget minus YTD Actual)

## 3. Materiality Flagging

- Flag variances exceeding $10K or 5% (whichever is lower)
- Use conditional formatting: green for favourable, red for unfavourable
- Top 5 variances should be highlighted for commentary focus

## 4. Sign Convention

- Revenue variance: Actual minus Budget (positive means above budget)
- Expense variance: Budget minus Actual (positive means under budget, which is favourable)
- This ensures positive variances are always favourable across both revenue and expenses

## 5. Gotchas

- Percentage variance on a near-zero base produces misleading results — suppress % when base is below $1K
- YTD variances can mask monthly swings — always show both monthly and YTD
- One-off items should be flagged separately so they do not distort trend analysis
- Ensure the comparative period is the correct version (original budget vs reforecast)

## 6. Constraints

- This skill ADDS a sheet with formulas referencing source data — it does not modify source sheets
- All variances must be formulas, not hardcoded calculations
- Materiality thresholds should be in named cells for easy adjustment
- Do NOT include commentary text on this sheet — that belongs in the Commentary Generator output
