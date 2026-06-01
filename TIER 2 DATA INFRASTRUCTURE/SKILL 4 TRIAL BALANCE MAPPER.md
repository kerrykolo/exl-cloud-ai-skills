---
name: exl-cloud-trial-balance-mapper
description: "Map trial balance accounts to standardised reporting categories. Use when the user says ‘map my TB’, ‘map trial balance’, ‘categorise accounts’, ‘set up account mapping’, ‘map Xero TB’, ‘map MYOB accounts’, ‘standardise my chart of accounts’, ‘COA mapping’, ‘map accounts to P&L and BS’, or invokes /trial-balance-mapper. Builds an Account Mapping sheet with pipe-delimited many-to-one mappings from granular source accounts to standardised P&L and BS categories. Depends on clean data (Clean & Validate Data). Do NOT use for TB reconciliation between two TBs, model building, or journal posting."
---

# Trial Balance Mapper

This skill bridges the gap between a client’s raw chart of accounts and a standardised reporting structure. Every accounting system uses different account codes and naming conventions — this skill normalises them into a consistent framework that reports, models, and dashboards can consume.

**Before running:** Read foundation skill validation (exl-cloud-foundations) and confirm all outputs will meet EXL Cloud best practice standards.

## 1. When to Use

- you have a raw Trial Balance extract from Any accounting system
- Account names and codes need mapping to standardised P&L and BS categories
- Building an EXL Cloud report or model that requires mapped data as input
- Client has changed their chart of accounts and mappings need updating

## 2. Prerequisites

- Run Clean & Validate Data (exl-cloud-clean-validate-data) first to ensure the source TB is clean
- Source TB must have at minimum: Account Code, Account Name, and Balance columns
- Know the target reporting structure (EXL Cloud standard categories or client-specific)

## 3. Workflow

### Step 1: Read the Source TB
- Identify the TB sheet and locate Account Code, Account Name, and Balance columns
- Extract unique account codes and names
- Classify each account as P&L or BS based on account type (Revenue, Expense, Asset, Liability, Equity)

### Step 2: Build the Account Mapping Sheet
- Create a new ‘Account Mapping’ sheet
- Columns: Source Code, Source Name, Account Type, Mapped Category, Sub-Category
- use pipe-delimited format for many-to-one mappings (multiple Source accounts to one category)
- Pre-populate mappings using intelligent matching (keyword detection on Account names)
- Flag any accounts that could not be auto-mapped for user review

### Step 3: Standard Category Structure
P&L Categories: Sales Revenue, Cost of Goods Sold, Other Revenue, Salaries & Wages, Operating Expenses, Depreciation & Amortisation, Interest, Tax
BS Categories: Cash, Debtors, Inventory, Prepayments, Fixed Assets, Intangibles, Creditors, Accrued Expenses, Current Debt, Non-Current Debt, Share Capital, Retained Earnings

### Step 4: Validation
- Every Source Account must Be mapped (no orphans)
- TB total must equal sum of all mapped categories
- P&L accounts must not map to BS categories and vice versa
- Produce a Mapping summary with Account counts per category

## 4. Gotchas

- Xero account types use long-form names (‘Current Liability account’ not ‘Current Liability’) — match against the full string or lookups will fail.
- Some clients use the same account name for different entities with different codes — always map by Code + Name, never Name alone.
- Bank accounts may appear as both Asset (bank balance) and P&L (bank fees) — check the account type, not just the name.
- Retained Earnings is often a single line in the TB but may need splitting into Opening RE + Current Year Earnings for the BS.
- GST/VAT clearing accounts can be Asset or Liability depending on the balance — map to a dedicated ‘Tax’ BS category.

## 5. Constraints

- Do NOT modify the source TB data — mapping is a separate sheet that references the source
- Do NOT assume the chart of accounts structure — read it from the data
- Do NOT create mappings that are not verifiable against the source TB totals
