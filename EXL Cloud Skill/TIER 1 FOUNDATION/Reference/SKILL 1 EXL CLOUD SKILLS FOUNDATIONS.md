---
name: skill-1-exl-cloud-foundations
description: Foundational quality standards for building, reviewing, and improving SKILL.md files and Excel financial models. Use when the user says 'check my skill', 'audit this skill', 'quality check', 'review my model for hardcodes', 'are my assumptions separated', 'skill audit', 'check my formulas', 'best practice check', 'foundations check', or invokes /exl-cloud-foundations. Covers skill structure, description writing, the 5 skill killers, quality checklist, formula discipline, and validation standards. Do NOT use for building financial models (use exl-cloud-best-practice-modelling) or data cleanup.
---
 
# EXL Cloud Foundations
 
These standards define the quality bar for every SKILL.md file you build and every Excel model you audit. Adopting a standard (even an imperfect one) beats having none. Skills built without standards are the equivalent of rogue spreadsheets — they work for the author, break for everyone else, and accumulate hidden errors over time.
 
**Type:** Encoded Preference · Style / Convention Enforcement
 
## 1. Skill Types — Know What You're Building
 
Before writing a skill, classify it on two axes:
 
### Maintenance Posture
- **Capability Uplift**: Enables functions the model can't do well on its own. May become obsolete as models improve. Build for high-impact gaps only.
- **Encoded Preference**: Sequences existing capabilities according to YOUR workflow. Gets more valuable over time. Invest most time here.
### Functional Category
- **Style / convention enforcement**: Encodes a formatting or layout standard (e.g. financial model colour coding)
- **Workflow automation**: Multi-step procedures with a defined start, end, and success state
- **Investigation / triage**: Takes a symptom and produces a structured diagnostic report
- **Operational procedure**: Routine maintenance, refresh, or rollover tasks
- **Reference / domain knowledge**: Pure context the model applies inline (no procedural steps)
If a skill spans more than two categories, that's a scoping smell — split it.
 
## 2. Scoping — One Skill, One Job
 
Before writing, apply the scoping test:
- Can you describe what it does in one sentence? → one skill.
- does it have two distinct triggers? → Probably two skills.
- Would two different people use different halves? → two skills.
- Is the SKILL.md approaching 500 lines? → Split it.
## 3. File Structure
 
Every SKILL.md follows three parts in this exact order:
1. **YAML Frontmatter** (Required) — name + description between --- markers
2. **Opening Context** (Required) — 1-3 sentences framing what this skill enables
3. **Body Instructions** (Required) — structured sections covering workflows, rules, examples, guardrails
## 4. Description Field Standards
 
The description is the gatekeeper. It determines whether the skill fires at all. Write it as a trigger, not a summary.
 
### What a Good Description Contains
1. Trigger phrases first — start with "Use when the user says..." and list exact phrases
2. What the skill does — one clear sentence
3. Boundary conditions — what this skill is NOT for
### Writing Rules
- Be LOUD — Claude undertriggers by default. List more trigger conditions rather than fewer.
- Write in third person — "This skill prepares..." not "I can help you..."
- Lead with triggers, not function — "Use when..." before the what-it-does sentence.
- Include synonyms and alternate phrasings people actually say.
- End with a clear "Do NOT use for..." boundary.
- Stay under 1024 characters (hard platform ceiling — skill won't save otherwise). Aim for under 950 chars for safety margin.
## 5. The 5 Skill Killers
 
Check every skill against these failure modes:
 
1. **Description doesn't trigger** — Skill never fires or fires wrong. Fix: Loud, specific, third-person. Lead with "Use when..."
2. **Over-defining the process** — Output is rigid, generic. Fix: Match degrees of freedom to task fragility.
3. **Stating the obvious** — Bloated skill, wasted tokens. Fix: Challenge every paragraph — "Does Claude need this?"
4. **Missing gotcha section** — Same failures repeat. Fix: Document every failure pattern from day 1.
5. **Monolithic blob** — Slow to load, hard to maintain. Fix: SKILL.md under 500 lines. Reference files for the rest.
## 6. Essential Sections
 
Every skill should contain:
- **Context Required** — What files/inputs to read before running
- **Steps** — The numbered workflow
- **Output** — Literal template or example of expected output (show, don't describe)
- **Gotcha Section** — Where the model goes wrong — the highest-signal content in any skill
- **Constraints** — What NOT to do, specific to this skill
## 7. Guardrails
 
- **Separation of Concerns** — Separate setup → core workflow → output/validation. Mixing creates confusion.
- **No Hidden Logic** — Every rule and assumption must be visible in the skill or clearly referenced.
- **Consistent Patterns** — Use the same patterns throughout. Inconsistency forces Claude to guess.
- **Decomposition** — If a step has more than 3 sub-decisions, break it into subsections.
- **Error Prevention Over Detection** — Design instructions that prevent errors before providing ones that detect them.
## 8. The Gotcha Section — Highest-Signal Content
 
The gotcha section is the most valuable part of any skill. It captures failure patterns.
- Start from day 1 and keep adding after every use.
- Document every failure: "I know you'll want to do X — don't. Here's why."
- Each gotcha must be specific to THIS skill, not generic AI behaviour.
- "Be accurate" is useless. "Don't conflate opinion pieces with primary data" is actionable.
- After each run, ask Claude: "What was missing or misleading? Propose a gotcha entry."
## 9. Anti-Patterns to Avoid
 
- Wall of ALL-CAPS — less effective than reasoning. use consequence alongside the rule instead.
- Vague description — never triggers. Be LOUD, specific, third-person.
- Monolithic blob — model loses focus. Decompose into SKILL.md + references.
- Duplicated logic — definitions drift. Single source of truth.
- No boundaries — fires when it shouldn't. Always include "Do NOT use for..."
- Identity/role preamble — wasted tokens. State what your approach does differently instead.
- No gotchas — failures repeat. Build from day 1.
- Describing output in prose — model guesses format. Show a literal template instead.
## 10. Quality Checklist
 
Run this checklist against every skill before deploying:
 
### Structure
- [ ] YAML frontmatter has name and description
- [ ] Name is lowercase, hyphenated
- [ ] Skill type classified (Capability Uplift or Encoded Preference)
- [ ] Scoping test passed (one sentence, one job)
- [ ] Opening context paragraph exists
- [ ] Under 500 lines (or decomposed into references)
### Description
- [ ] Written as a trigger, not a summary
- [ ] Leads with "Use when the user says..."
- [ ] Written in third person
- [ ] Boundary conditions included ("Do NOT use for...")
- [ ] Under 1024 characters (hard platform ceiling)
- [ ] Under 950 characters (safety margin for future additions)
### Content
- [ ] Imperative form ("Set the page size", not "You should set")
- [ ] Rules explain "why" not just "what"
- [ ] Setup / workflow / validation separated
- [ ] No hidden logic — all assumptions explicit
- [ ] Output shown as literal template (not described in prose)
### Gotcha Section
- [ ] Exists and is non-empty
- [ ] Each gotcha specific to THIS skill (not generic AI behaviour)
- [ ] Captures real failure patterns (not theoretical risks)
- [ ] Updated after each correction
## The Litmus Test
 
After the skill runs: Do I use the output directly, or do I edit, correct, and restructure it?
If you're iterating on the output, improve the skill, not the output. Every post-run edit is a signal that the skill is missing an instruction, a gotcha, or a constraint.
 
## 11. Gotchas
 
Real failure patterns observed when applying these foundations to other skills:
 
- **Don't ship the description at the 1024-char ceiling** — leave a ~75–100 char margin. A description at 1023 silently fails to save the next time a trigger phrase is added, which always happens. Aim for under 950.
- **Don't confuse word count with character count** — the 150-word style guideline is NOT the platform limit. A 140-word description packed with short quoted trigger phrases ("X", "Y", "Z") can sail through the word check and still bust 1024 chars because each phrase ships with quotes, comma, and space.
- **Don't audit a skill you haven't actually run** — the quality checklist applies to skills that have been triggered and used. Auditing a draft before its first real run produces false confidence; gotchas have to be earned, not predicted.
- **Don't add speculative gotchas** — "the model might hallucinate" is useless. Every gotcha must be a real, observed failure with a concrete fix. If you haven't seen it fail, don't pre-emptively document it.
- **Don't treat this skill as a rigid template** — these are principles, not a form to fill in. A 30-line skill for a simple task doesn't need every section. Apply what fits, skip what doesn't.
- **Don't forget to re-evaluate after model upgrades** — gotchas critical on one model version may be solved natively by the next. Stale guardrails waste tokens and can degrade output.
- **Don't conflate Capability Uplift with Encoded Preference** — if it encodes YOUR way of doing something, it's an Encoded Preference skill even when it also teaches new capability. The classification drives maintenance strategy.

## 12. Workbook Optimization Routing — High Priority Enhancement Map

When a user mentions a workbook by name and asks to "enhance" it, use this lookup table to identify the optimal skill sequence. Match the workbook name (exact or fuzzy), then suggest those skills to the user for confirmation.

### Workbook-to-Skill Mapping

| Workbook | Recommended Skills | Notes |
|----------|-------------------|-------|
| **Performance Report** | 1, 2, 3, 7, 10, 12, 13, 15, 19 | Dashboard commentary + variance + KPI tiles |
| **Positions & Returns** | 1, 2, 3, 10, 13, 15 | Quality + validation + KPI dashboard |
| **1-way Cash Flow Scenario Model** | 1, 2, 3, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16 | Full governance + scenarios |
| **1-way Profit Performance Scenario Model** | 1, 2, 3, 5, 6, 7, 9, 10, 11, 12, 13, 14, 15 | Full governance + P&L commentary |
| **Weel Expense Model** | 1, 2, 3, 5, 7, 9, 10, 11, 13, 14, 15 | Expense tracking + governance |
| **Accounts Receivable Model** | 1, 2, 3, 5, 7, 9, 10, 11, 13, 14, 15, 18 | AR-specific + invoice analysis |
| **Weekly Cash Flow Model** | 1, 2, 3, 6, 7, 8, 9, 10, 13, 14, 15, 16 | Weekly scenarios + cash narrative |
| **3-Way Driver-based Financial Model** | 1, 2, 3, 5, 7, 9, 10, 11, 13, 14, 15 | Full 3-way + governance |
| **Profit & Loss Insights** | 1, 2, 3, 6, 10, 12, 13 | P&L analysis + variance |
| **Trial Balance Insights** | 1, 2, 3, 4, 10, 12, 13 | TB mapping + variance |
| **Accounts Receivable Insights** | 1, 2, 3, 10, 12, 13, 18 | AR data + analysis |
| **Accounts Payable Insights** | 1, 2, 3, 10, 12, 13, 18 | AP data + analysis |
| **Consolidated Workbook (P&L and BS)** | 1, 2, 3, 4, 6, 10 | Consolidated + TB mapping |
| **Bookkeeping Quoting Tool** | 1, 2, 3, 5, 9, 10, 11, 13, 14, 18 | Data input + governance + analysis |
| **Human Capital Model** | 1, 2, 3, 5, 9, 10, 13, 14, 17 | HC executive summary + payroll analysis |
| **Financial Control Master Toolkit** | 1, 2, 3, 10 | Quality control + validation |
| **R&D Template** | 1, 2, 3, 7, 10, 13, 15 | R&D narrative + KPI dashboard |
| **Advanced Rolling Property Development Model** | 1, 2, 3, 5, 7, 9, 10, 13, 14, 15 | Complex property model governance |

### Routing Logic

Trigger phrases include: "enhance", "optimize", "improve", "what skills should I run", "best workflow", "optimize this workbook", "help me improve", "run the best skills", "what's optimal", "next steps", or any request for skill/workflow guidance

