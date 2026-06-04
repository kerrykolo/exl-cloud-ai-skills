---
name: exl-cloud-tier-1
description: "Router for EXL Cloud Tier 1 quality and modelling standards. Covers: (1) skill/model auditing, quality checks, hardcode review, formula discipline, skill structure — use for 'check my skill', 'audit this skill', 'quality check', 'foundations check', 'skill audit'; (2) Excel formatting, colour coding, number formats, EXL Cloud conventions — use for 'format my model', 'colour code inputs', 'apply EXL Cloud standards', 'blue inputs black formulas', 'fix number formats'. Always views the correct Reference/ file before proceeding — never guesses."
---

# EXL Cloud Tier 1 — Standards Router

Router for foundational quality and modelling standards. Identify the task, load the correct `Reference/` file, follow its instructions. Never guess — always load.

---

## Sub-Skill Directory

| # | Skill | File | Use When |
|---|-------|------|----------|
| 1 | Foundations | `Reference/skill-1-exl-cloud-foundations.md` | Auditing a skill or model for quality, hardcodes, formula discipline, structure |
| 2 | Best Practice Modelling | `Reference/skill-2-exl-cloud-best-practice-modelling.md` | Formatting, colour coding, number formats, tab structure, EXL Cloud conventions |

---

## Routing Rules

### Single-Skill Tasks

| Task | Load |
|------|------|
| "check my skill", "audit this skill", "quality check", "skill audit", "foundations check", "review my model for hardcodes", "check my formulas", "best practice check" | `Reference/skill-1-exl-cloud-foundations.md` |
| "format my model", "colour code my inputs", "apply modelling best practice", "fix my number formats", "audit model structure", "apply EXL Cloud standards", "blue inputs black formulas", "financial model formatting" | `Reference/skill-2-exl-cloud-best-practice-modelling.md` |

### Multi-Skill Tasks

| Task | Load Order |
|------|------------|
| Full model quality audit + formatting fix | `skill-1` → `skill-2` |

---

## Decision Tree

```
What is the task?
│
├── Auditing a skill or model for quality / hardcodes / structure?
│    └── VIEW: Reference/skill-1-exl-cloud-foundations.md
│
├── Formatting a model / applying EXL Cloud colour and number conventions?
│    └── VIEW: Reference/skill-2-exl-cloud-best-practice-modelling.md
│
└── Both quality audit AND formatting?
     └── VIEW: skill-1 first → then skill-2
```

---

## Notes

- **Skill 1 before Skill 2** — always audit quality first; apply formatting conventions after structural issues are resolved
- Skill 1 covers SKILL.md authoring standards as well as Excel model auditing
- Skill 2 covers style and convention only — it does not build calculation engines or proprietary logic
