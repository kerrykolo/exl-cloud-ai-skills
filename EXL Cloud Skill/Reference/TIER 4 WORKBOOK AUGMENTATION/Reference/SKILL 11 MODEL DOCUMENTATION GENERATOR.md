---
name: skill-11-exl-cloud-model-documentation-generator
description: "Generate model documentation and Instructions sheet. Use when the user says 'document this model', 'add instructions', 'create documentation', 'explain the model structure', 'model overview', 'add a readme sheet', 'document the workbook', or invokes /model-documentation-generator. Reads all sheets and produces structured documentation. Do NOT use for model building, validation checks, or commentary."
---

# Model Documentation Generator

Good models are self-documenting. This skill generates a comprehensive Instructions sheet that any new user can read to understand the workbook's purpose, structure, inputs, outputs, and operating procedures. Without documentation, models become black boxes that only the original builder can use.

**Type:** Encoded Preference · Workflow Automation

**Before outputting:** Re-check this sheet against skill-2 and skill-1.

## 1. When to Use

- Completing a new model build (documentation is the final step)
- Handing over a model to a new user or team
- Preparing a model for external review or audit
- Onboarding new team members to an existing workbook

## 2. Documentation Structure

### Model Overview
- Purpose and scope of the workbook
- Date created, author, version number
- Key assumptions and limitations

### Sheet Map
- Table listing every sheet with: Tab Name, Purpose, Tab Colour, Key Contents
- Grouped by function (Inputs, Calculations, Outputs, Admin)

### Data Flow
- How data flows from inputs through calculations to outputs
- Key cross-sheet references and dependencies
- Which sheets are source data vs derived

### Operating Procedures
- Data refresh steps (what to update each period)
- Scenario switching instructions
- Print/export guidance

### Colour Code Legend
- Blue text: hardcoded inputs
- Black text: formulas and calculations
- Green text: cross-sheet links

## 3. Gotchas

- Auto-generated documentation is a starting point — the user should review and add context-specific notes
- Sheet names may not be self-explanatory — the documentation should clarify purpose, not just list names
- If an Assumption Register exists, reference it rather than duplicating the assumptions list
- Version history should be maintained on a separate Change Log sheet, not in the Instructions

## 4. Constraints

- This skill ADDS a sheet — it does not modify existing data
- Do NOT overwrite an existing Instructions sheet without asking
- Documentation must be accurate to the current state of the workbook — do not describe sheets that do not exist
- Keep language simple and jargon-free — the audience may not be financial modellers
