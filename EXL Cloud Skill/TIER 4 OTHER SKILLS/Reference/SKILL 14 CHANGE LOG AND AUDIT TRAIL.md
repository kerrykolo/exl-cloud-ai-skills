---
name: Skill-14-exl-cloud-change-log-audit-trail
description: "Add Change Log and Claude Log sheets. Use when the user says 'add change log', 'add audit trail', 'track changes', 'version history', 'add governance sheets', 'claude log', 'AI audit trail', 'model governance', or invokes /change-log-audit-trail. Creates structured change tracking sheets. Independent of other skills. Do NOT use for model building, validation, or commentary."
---

# Change Log & Audit Trail

Model governance requires knowing who changed what, when, and why. This skill adds two tracking sheets: a Change Log for human-readable version history, and a Claude Log for AI-specific audit trails. Together they provide complete traceability for model reviews, audits, and handovers.

**Type:** Encoded Preference · Operational Procedure

**Before outputting:** Re-check this sheet against skill-2 and skill-1.

## 1. When to Use

- Starting any new model build (add early, update continuously)
- Adding governance to an existing workbook
- Preparing for external audit or model review
- Any workbook where multiple people or AI assistants make changes

## 2. Change Log Structure

Columns: Version | Date | Author | Sheet(s) Affected | Change Description | Reason

- Version: sequential (v1.0, v1.1, v1.2...)
- Date: date of change
- Author: name of person or system making the change
- Sheet(s) Affected: which tabs were modified
- Change Description: what was changed (specific cells, formulas, structure)
- Reason: why the change was made (business context)

## 3. Claude Log Structure

The Claude Log captures AI-assisted changes with additional detail:

Columns: Timestamp | Skill Used | Sheet(s) | Cells Modified | Action Taken | Verification

- Timestamp: date and time of AI action
- Skill Used: which AI skill was invoked
- Sheet(s): which tabs were affected
- Cells Modified: specific cell references changed
- Action Taken: description of what the AI did
- Verification: how the result was verified (check passed, user confirmed, etc.)

## 4. Best Practices

- Log entries should be written at the time of the change, not retrospectively
- Group related changes into a single version entry (do not log every cell individually)
- Use consistent language: Added, Modified, Deleted, Restructured, Reformatted
- Major structural changes warrant a version increment (v1.0 to v2.0)

## 5. Gotchas

- Change logs are only useful if they are maintained — a stale log is worse than no log
- AI changes should be logged automatically, not depend on the user remembering to update
- Do not log trivial formatting changes (column width adjustments, font tweaks) unless they affect readability
- The Change Log sheet should be positioned as the last tab (or second-to-last before Claude Log)

## 6. Constraints

- This skill ADDS sheets — it does not modify existing data
- Do NOT overwrite existing Change Log or Claude Log sheets — append new rows
- Tab colours: Change Log uses light grey (#D9D9D9), Claude Log uses dark grey (#808080) so the two governance sheets are visually distinguishable
- Both sheets should be formatted as Excel Tables for easy filtering and sorting
- Do NOT freeze panes, hide gridlines, or modify sheet view settings