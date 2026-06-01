---
name: exl-cloud-board-paper-drafter
description: "Assemble a board paper from financial data and commentary inputs. Use when the user says 'draft the board paper', 'board report', 'prepare board pack', 'management report for the board', 'board meeting paper', 'executive summary for board', or invokes /board-paper-drafter. Consumes monthly commentary, cash flow narrative, variance analysis, and KPI data. Do NOT use for monthly commentary alone (use §4), cash flow narrative (use §6), or data cleanup."
---

# Board Paper Drafter

This skill assembles a complete, board-ready management report by consuming outputs from sibling skills. It does not generate raw analysis — it structures and presents the work already done by the Commentary Generator, Cash Flow Narrative, Variance Reporter, and KPI Dashboard into a coherent document.

**Before running:** Read foundation skill validation (exl-cloud-foundations) and confirm all outputs will meet EXL Cloud best practice standards.

## 1. When to Use

- Preparing monthly or quarterly board packs
- Management committee reporting
- Investor update documents
- Any structured financial report that combines narrative with data

## 2. Dependencies

This skill consumes outputs from:
- Monthly Commentary Generator(exl-cloud-monthly-commentary-generator) → variance section narrative
- Cash Flow Narrative(exl-cloud-cash-flow-narrative) → cash position section
- Variance Analysis Reporter(exl-cloud-variance-analysis-reporter) → financial highlights data
- KPI Dashboard Formatter(exl-cloud-kpi-dashboard-formatter) → executive summary KPI tiles

## 3. Document Structure

### Section 1: Executive Summary
- 3-5 bullet points summarising the period
- Key KPI tiles (revenue, EBITDA, cash, headcount)
- Traffic light status indicators (on track / at risk / off track)

### Section 2: Financial Highlights
- P&L summary table (actual vs budget vs prior year)
- Top 5 variance items with dollar and percentage impact
- Year-to-date position vs full-year budget

### Section 3: Operational Commentary
- Revenue commentary (from Monthly Commentary Generator)
- Cost commentary (from Monthly Commentary Generator)
- Bottom line analysis (EBITDA, NPAT)

### Section 4: Cash Position
- Cash flow narrative (from Cash Flow Narrative skill)
- Opening and closing cash positions
- Key cash movements and working capital impact

### Section 5: Outlook & Risks
- Forward-looking commentary (only if user provides guidance)
- Key risks and mitigations
- Decisions required from the board

## 4. Formatting Standards

- Use clear section headings with consistent numbering
- Tables must include column headers and be formatted for readability
- Use colour sparingly: green for favourable, red for unfavourable, amber for watch items
- Keep the total document to 3-5 pages (concise, not exhaustive)

## 5. Gotchas

- Do not duplicate content across sections — the executive summary should summarise, not repeat the detailed sections
- Board members want to know what changed and why, not a recitation of every line item
- Always include a clear "Decisions Required" section even if the answer is "nil for this period"
- Cash position commentary must reconcile to the BS cash balance — if it does not, flag the gap
- Year-to-date figures are more important than single-month variances for board-level reporting

## 6. Constraints

- This skill produces TEXT/DOCUMENT output — it does not modify workbook data
- Do NOT fabricate forward-looking statements — only include if user provides the outlook
- Do NOT include appendices unless explicitly requested
- All numbers must be traceable to workbook cells
