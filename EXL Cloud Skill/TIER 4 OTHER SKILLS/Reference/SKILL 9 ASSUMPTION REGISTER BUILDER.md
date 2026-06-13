---
name: skill-9-exl-cloud-assumption-register-builder
description: "Extract assumptions from a workbook into a structured register. Use when the user says 'extract assumptions', 'build assumption register', 'list all inputs', 'find hardcoded values', 'assumption audit', 'what assumptions does this model use', 'document the inputs', or invokes /assumption-register-builder. Scans all sheets, identifies input cells, and creates an Assumption Register sheet. Independent of other skills. Do NOT use for model building, validation checks, or commentary."
---

# Assumption Register Builder

Every financial model contains assumptions — growth rates, margins, tax rates, discount rates, timing parameters. This skill finds them all and organises them into a single, auditable register. Without this, assumptions are scattered across sheets, buried in formulas, and impossible to scenario-test.

**Type:** Encoded Preference · Investigation / Triage

**Before outputting:** Re-check this sheet against skill-2 and skill-1.

## 1. When to Use

- Inheriting a model built by someone else
- Preparing a model for scenario analysis
- Auditing a model for hidden hardcodes
- Building documentation for a model handover
- Board or investor due diligence preparation

## 2. Detection Method

The skill scans each sheet and identifies:
- Cells with hardcoded numeric values that are referenced by formulas (input cells)
- Named ranges pointing to constant values
- Cells formatted as blue text (hex 0070C0) on light green bg (hex E2E9DA)
- Cells formatted as blue text (hex 0070C0) on medium green bg (hex ABBB93)
- Cells formatted as blue text (hex 0070C0) on no fill
- Cells with labels containing keywords: rate, growth, margin, assumption, factor, multiple, discount, term

## 3. Register Structure

The Assumption Register sheet contains:
- Assumption Name: derived from adjacent labels or named range
- Current Value: the hardcoded value
- Cell Reference: sheet and cell address
- Category: Revenue, Cost, Tax, Financing, Timing, Other
- Source/Basis: user-populated field for documenting the rationale
- Last Reviewed: date field for governance tracking

## 4. Gotchas

- Not every hardcoded number is an assumption — constants like 12 (months per year) or 365 (days) are structural, not assumptions
- Some assumptions are embedded in formulas rather than in dedicated cells — flag these but do not auto-extract (the user needs to refactor)
- Named ranges may point to ranges, not single cells — check for array-based assumptions
- Protected sheets may block reading — ask user to unprotect

## 5. Constraints

- This skill ADDS a sheet — it does not modify existing data
- Do NOT change existing cell references or named ranges
- Do NOT auto-categorise assumptions — suggest categories but let the user confirm
- Do NOT include structural constants (12, 365, 100) unless they are clearly business assumptions
- Do NOT freeze panes, hide gridlines, or modify sheet view settings