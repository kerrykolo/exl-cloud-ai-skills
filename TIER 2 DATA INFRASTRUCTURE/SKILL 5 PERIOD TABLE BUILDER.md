---
name: exl-cloud-period-table-builder
description: "Build the tbl_Period date structure for time-series workbooks. Use when the user says 'build tbl_Period', 'set up periods', 'create date table', 'build time series', 'set up the date axis', 'period table', 'ACT FCST toggle', 'financial year dates', 'monthly periods', or invokes /period-table-builder. Builds a structured Excel table with period start dates, end dates, days in period, period counters, financial year, and ACT/FCST flags. Independent of Clean Data and TB Mapper. Do NOT use for calendar scheduling, project timelines, or non-financial date structures."
---

# Period Table Builder

The period table (tbl_Period) is the backbone of every EXL Cloud time-series workbook. It defines the date columns that all calculation engines, reports, and dashboards reference. Getting this right means every downstream formula inherits correct dates, financial years, and ACT/FCST classifications automatically.

**Before running:** Read foundation skill validation (exl-cloud-foundations) and confirm all outputs will meet EXL Cloud best practice standards.

## 1. When to Use

- Setting up a new EXL Cloud workbook (report, model, or dashboard)
- Extending an existing workbook's time horizon (adding future periods)
- Changing the financial year end month
- Switching between monthly, quarterly, or annual periodicity

## 2. Required Inputs

- Start Date: first period start (e.g. 1 July 2023)
- Number of Periods: how many periods to generate (e.g. 36 months)
- Periodicity: Monthly (default), Quarterly, or Annual
- Financial Year End Month: month number (e.g. 6 for June, 12 for December)
- ACT/FCST Cutoff Date: the date separating actual from forecast periods

## 3. Table Structure

tbl_Period contains one row per period with these columns:
- Period_Start: first day of the period (EOMONTH-based)
- Period_End: last day of the period
- Days_in_Period: count of days (for daily proration calculations)
- Period_Counter: sequential integer (1, 2, 3...) for OFFSET-based lookups
- Month_Num: calendar month number (1–12)
- Year: calendar year
- FY: financial year (adjusted for FY end month)
- FY_Period: period number within the financial year (1–12 for monthly)
- ACT_FCST: ACT for actual periods, FCST for forecast periods
- Display_Label: formatted label for headers (e.g. Jan 24, Q1 FY24)

## 4. Key Formulas

- Period_Start: use EOMONTH(Start_Date, Period_Counter - 2) + 1
- Period_End: use EOMONTH(Period_Start, 1) - 1 for monthly
- Days_in_Period: use Period_End - Period_Start + 1
- FY: use YEAR + IF logic to adjust for FY end month
- ACT_FCST: compare Period_End to ACT_FCST_Cutoff date

## 5. Gotchas

- Always use EOMONTH for date arithmetic — never add 30 or 31 days. February and month-end boundaries will break.
- The FY calculation must use the Period_Start month, not Period_End — a period starting in June belongs to the FY ending June, not the next FY.
- ACT/FCST flag must compare Period_End (not Period_Start) to the cutoff date — a partially-complete period is still ACT until its end date passes.
- Quarterly periodicity must align to the FY end month (e.g. FY ending June means Q1 is Jul-Sep, not Jan-Mar).
- Display_Label format must match the existing workbook convention. Check the first sheet header before generating.

## 6. Constraints

- Table must be named tbl_Period (exact casing) for structured references to work
- Do NOT hardcode dates — all dates derived from Start_Date and EOMONTH
- Do NOT mix periodicities in a single table (no monthly + quarterly in one tbl_Period)
- ACT/FCST cutoff must reference a named range or Master Control cell, not a hardcoded date
