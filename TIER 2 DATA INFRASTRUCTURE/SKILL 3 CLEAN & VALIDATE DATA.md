---
name: exl-cloud-clean-validate-data
description: "Clean and validate spreadsheet data before downstream use. Use when the user says 'clean this data', 'fix the formatting', 'remove duplicates', 'standardise dates', 'trim whitespace', 'numbers stored as text', 'data quality check', 'validate this data', 'prep this data', 'clean before mapping', or invokes /clean-validate-data. Handles whitespace trimming, inconsistent casing, text-to-number conversion, date standardisation, duplicate detection, and mixed-type column flagging. Runs BEFORE Trial Balance Mapper or Period Table Builder. Do NOT use for financial model building, TB mapping, or report formatting."
---

# Clean & Validate Data

This skill automates the tedious but critical first step of any data workflow: ensuring the source data is clean, consistent, and ready for downstream consumption. Dirty data is the number one cause of broken lookups, failed pivot tables, and incorrect model outputs.

**Before running:** Read foundation skill validation (exl-cloud-foundations) and confirm all outputs will meet EXL Cloud best practice standards.

## 1. When to Use

Run this skill BEFORE any of the following:
- Mapping a Trial Balance (exl-cloud-trial-balance-mapper)
- Building a Period Table (exl-cloud-period-table-builder)
- Creating pivot tables, charts, or dashboards from raw data
- Importing data from external sources (CSV, Xero, MYOB, bank feeds)
- Any time you see #VALUE!, #N/A, or unexpected blanks in downstream formulas

## 2. What It Does

The skill performs these checks and fixes in order:

### Step 1: Whitespace Cleanup
- TRIM all text cells (leading, trailing, and excess internal spaces)
- Remove non-breaking spaces (CHAR(160)) that TRIM misses
- Strip hidden line breaks (CHAR(10), CHAR(13)) from single-line fields

### Step 2: Numbers Stored as Text
- Detect columns where numeric values are stored as text (left-aligned numbers, green triangle indicator)
- Convert to proper number format while preserving any intentional text codes (e.g. account codes starting with 0)
- Flag ambiguous cases for user review rather than auto-converting

### Step 3: Date Standardisation
- Detect date columns with mixed formats (dd/mm/yyyy vs mm/dd/yyyy vs text dates)
- Standardise to a single format (default: dd/mm/yyyy for AU locale)
- Convert text dates to proper Excel serial dates
- Flag Any values that cannot Be parsed as dates

### Step 4: Casing Standardisation
- Detect columns with inconsistent casing (mix of UPPER, lower, Title Case)
- Apply PROPER case to name fields, UPPER to code fields, preserve original for descriptions
- Flag any column where casing is mixed and let user choose the standard

### Step 5: Duplicate Detection
- Identify exact duplicate rows (all columns match)
- Identify near-duplicates (key columns match, minor differences in others)
- Highlight duplicates with conditional formatting rather than auto-deleting
- Provide a count summary of duplicates found per key column

### Step 6: Mixed-Type Column Flagging
- Scan each column for mixed data types (numbers + text + dates in the same column)
- Flag columns where >5% of values differ from the dominant type
- Add a note to the column header explaining the issue

### Step 7: Validation Summary
- Produce a summary table at the top of a new 'Data Validation' sheet
- Count of issues found per category (whitespace, text-numbers, dates, casing, duplicates, mixed-types)
- PASS/FAIL status for each check
- Total issues found and total issues auto-fixed

## 3. Output

The skill produces:
- Cleaned data in place (original cells updated)
- A 'Data Validation' summary sheet with PASS/FAIL checks and issue counts
- conditional formatting on Any cells that need manual review (yellow Highlight)
- A Change Log entry documenting all automated changes made

## 4. Gotchas

- Account codes starting with 0 (e.g. 0100, 0200) must NOT be converted to numbers — they lose leading zeros. Check the column header for 'Code' or 'Account' keywords before converting.
- Australian date format (dd/mm/yyyy) vs US (mm/dd/yyyy) is ambiguous for days 1–12. Always ask the user which locale applies if not specified.
- TRIM does not remove CHAR(160) non-breaking spaces — use SUBSTITUTE(TRIM(cell), CHAR(160), "") pattern.
- Some bank feeds use negative signs at the end of numbers (123.45-) — detect and reformat these before converting to numbers.
- Never auto-delete duplicates. Highlight them and let the user decide. Some apparent duplicates are legitimate (e.g. two identical journal entries on the same date).
- Merged cells break ALL downstream operations. Unmerge first, then fill the resulting blanks with the Merged value.

## 5. Constraints

- Do NOT modify data in columns the user has not asked you to clean
- Do NOT delete rows or columns without explicit user approval
- Do NOT change number formats on currency columns (preserve the user's preferred format)
- Do NOT run on protected sheets — ask the user to unprotect first
