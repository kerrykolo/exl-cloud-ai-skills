---
name: skill-6-exl-cloud-monthly-commentary-generator
description: "Generate monthly management commentary from financial data. Use when the user says 'write commentary', 'monthly commentary', 'explain the P&L', 'management narrative', 'write up the month', 'commentary for the board', 'explain the variances', 'financial narrative', or invokes /monthly-commentary-generator. Reads P&L actual vs budget, identifies key drivers, and produces structured paragraphs. Feeds into exl-cloud-board-paper-drafter. Do NOT use for board paper assembly (use exl-cloud-board-paper-drafter), cash flow commentary (use exl-cloud-cash-flow-narrative), or data cleanup."
---

# Monthly Commentary Generator

This skill transforms raw financial data into professional management commentary. It reads actual results alongside budget or prior period comparatives, identifies the key drivers of variance, and produces structured narrative paragraphs ready for management packs or board papers.

**Type:** Encoded Preference · Workflow Automation

**Before outputting:** Re-check this sheet against skill-2 and skill-1.

## 1. When to Use

- Month-end reporting cycle when management commentary is required
- Preparing variance explanations for P&L line items
- Drafting narrative sections for management packs
- When the Board Paper Drafter needs variance commentary as input

## 2. Prerequisites

- Workbook must contain at least one period of actual P&L data
- Comparatives required: budget, prior period, or prior year (at least one)
- Data must be mapped to standard P&L categories (run TB Mapper first if needed)

## 3. Commentary Structure

The skill produces commentary in this order:

### Revenue Commentary
- Total revenue vs budget/prior with dollar and percentage variance
- Top 3 revenue drivers (largest positive and negative movements)
- Revenue mix changes if multiple revenue streams exist

### Cost Commentary
- Gross margin analysis (COGS as percentage of revenue vs budget)
- Operating expense movements (fixed vs variable cost behaviour)
- Salary and wages variance with headcount context if available
- One-off or non-recurring items called out separately

### Bottom Line
- EBITDA variance with margin percentage comparison
- Net profit after tax commentary
- Year-to-date cumulative position vs budget

## 4. Writing Style

- Use active voice and specific numbers (not "revenue increased" but "revenue increased $45K (8%) driven by...")
- Lead each paragraph with the variance, then explain the drivers
- Use parentheses for unfavourable variances: ($12K) unfavourable
- Keep each section to 2-3 sentences maximum
- Use Australian English spelling conventions

## 5. Gotchas

- Do not describe every line item — focus on material variances only (>5% or >$10K, whichever is lower)
- Sign convention matters: expenses are negative, so a negative variance on expenses is favourable (costs lower than budget)
- Year-to-date commentary must not just repeat the monthly narrative — identify trend differences
- If actuals are significantly different from budget, check whether the budget itself was reasonable before attributing the variance to operations
- Never say "the numbers speak for themselves" — the whole point of commentary is to explain what the numbers mean

## 6. Constraints

- This skill produces TEXT output only — it does not modify workbook data
- Commentary must reference specific cells/ranges so the reader can trace back to source
- Do NOT fabricate explanations — if the variance driver is unclear from the data, say so
- Do NOT include forward-looking statements unless explicitly asked
