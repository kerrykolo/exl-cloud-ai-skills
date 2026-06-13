---
name: skill-8-exl-cloud-cash-flow-narrative
description: "Generate cash flow narrative from financial data. Use when the user says 'explain cash flow', 'cash flow commentary', 'cash narrative', 'what happened to cash', 'cash position summary', 'explain the cash movements', 'why did cash change', or invokes /cash-flow-narrative. Reads CFS data and produces structured paragraphs covering operating, investing, and financing cash flows. Feeds into exl-cloud-board-paper-drafter. Do NOT use for P&L commentary, board paper assembly, or cash flow model building."
---

# Cash Flow Narrative

This skill reads cash flow data from the workbook and produces structured, plain-English narrative explaining where cash came from, where it went, and what the net position means.

**Type:** Encoded Preference · Workflow Automation

**Before outputting:** Re-check this sheet against skill-2 and skill-1.

## 1. When to Use

- Monthly or quarterly reporting where cash commentary is required
- Board papers that need a cash position section
- When stakeholders ask why the cash balance changed
- Covenant compliance reporting that requires cash narrative

## 2. Prerequisites

- Workbook must contain a Cash Flow Statement or cash waterfall data
- Opening and closing cash balances must be identifiable
- Comparatives (budget or prior period) are desirable but not mandatory

## 3. Narrative Structure

### Opening Position
- State the opening cash balance and the period covered
- Compare to prior period opening if available

### Operating Cash Flow
- Net cash from operations with the key drivers
- Working capital movements (receivables, payables, inventory)
- Non-cash adjustments that explain the gap between profit and cash

### Investing Activities
- Capital expenditure (maintenance vs growth capex if distinguishable)
- Asset disposals or acquisitions
- Investment movements

### Financing Activities
- Debt drawdowns and repayments
- Equity raisings or distributions
- Dividend payments

### Closing Position
- State the closing cash balance
- Compare to budget and prior period
- Highlight available headroom or liquidity concerns

## 4. Writing Style

- Lead with the net cash movement, then decompose into components
- Use specific dollar amounts, not vague qualifiers
- Explain the operational meaning, not just the accounting classification
- Example: Cash increased $120K driven by strong collections ($85K above budget) partially offset by an unbudgeted equipment purchase ($35K)

## 5. Gotchas

- The non-direct method CFS starts with net profit then adds back non-cash items — do not confuse depreciation add-backs with actual cash movements
- Working capital movements have counter-intuitive signs: an increase in receivables is a cash OUTFLOW (you sold but did not collect)
- Ensure the narrative ties to the balance sheet cash figure — if it does not, there is a reconciliation issue to resolve first
- Restricted cash or security deposits should be called out separately from available cash

## 6. Constraints

- This skill produces TEXT output only — it does not modify workbook data
- Do NOT fabricate cash movement explanations — if the driver is unclear, say so
- Do NOT include forecast cash commentary unless the user explicitly provides forward estimates
- All amounts must be traceable to workbook cells
- Do NOT freeze panes, hide gridlines, or modify sheet view settings
