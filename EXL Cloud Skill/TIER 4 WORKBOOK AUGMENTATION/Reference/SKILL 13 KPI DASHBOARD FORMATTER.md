---
name: skill-13-exl-cloud-kpi-dashboard-formatter
description: "Build a KPI dashboard with tiles and traffic lights. Use when the user says 'build a dashboard', 'KPI tiles', 'executive dashboard', 'one-page summary', 'traffic light dashboard', 'key metrics view', 'management dashboard', or invokes /kpi-dashboard-formatter. Creates a formatted dashboard sheet. Feeds into exl-cloud-board-paper-drafter. Do NOT use for variance analysis, commentary, or chart building."
---

# KPI Dashboard Formatter

This skill builds a one-page executive dashboard that presents key performance indicators as visual tiles. Each tile shows the metric name, current value, trend direction, comparison to target, and a traffic light status. Designed for printing or screen sharing in management meetings.

**Type:** Encoded Preference · Style / Convention Enforcement

**Before outputting:** Re-check this sheet against skill-2 and skill-1.

## 1. When to Use

- Monthly board pack preparation
- Executive team performance reviews
- Weekly/monthly operational check-ins
- Investor or stakeholder update summaries

## 2. Default KPI Tiles

- Revenue (actual vs budget)
- EBITDA (actual vs budget with margin %)
- Net Profit (actual vs budget)
- Cash Position (closing balance vs prior month)
- Headcount (actual vs plan)
- Customer Count or Pipeline (if applicable)

## 3. Tile Layout

Each tile is a 4-row x 3-column merged block containing:
- Row 1: Metric name (bold, 12pt)
- Row 2: Current value (bold, 18pt, formatted with units)
- Row 3: vs Budget/Target with variance (10pt, green/red)
- Row 4: Trend indicator and traffic light dot

## 4. Traffic Light Logic

- Green: within 5% of target or above target
- Amber: 5-15% below target
- Red: more than 15% below target
- Thresholds should be configurable via named cells

## 5. Gotchas

- Dashboard tiles must use formulas linking to source data — never paste static values
- Print area must be set so the dashboard fits on one page
- Conditional formatting colours must be accessible (avoid red/green for colour-blind users — add icons as backup)
- If a KPI has no budget/target, show vs prior period instead rather than leaving the comparison blank

## 6. Constraints

- This skill ADDS a sheet — it does not modify existing data
- Maximum 8 tiles per dashboard (more reduces readability)
- All values must link via formulas to source sheets
- Do NOT include detailed tables on the dashboard — keep it visual and summary-level
