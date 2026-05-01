# `templates/` — Templates the agent works from

This folder holds the canonical templates Greg's team uses for course building. Templates are scaffolded with embedded standards — slots the AI cannot leave abstract — so violations are hard to commit accidentally.

## Templates already in hand

Two canonical templates are available now, copied verbatim from Greg's working set:

- **`source/course-plan-exam.xlsx`** — The course plan template for Exam-Primary courses. Two sheets: **LO Map** (PLO + CLOs + KLOs) and **Assessment Map** (per-unit assessment plan with built-in validation formulas). See structure documented below.
- **`source/exam-question-bank.xlsx`** — The question bank template. Three sheets: **Multiple Choice**, **Short Answers**, **Long Answers**. Column structure documented below.

These are the Pillars Exam-Primary templates. Project-Primary equivalents will likely arrive separately from Greg.

## What's load-bearing in the course plan template

The course plan template is more sophisticated than a typical scaffold — it has built-in Excel-formula validation for many of the structural standards. The **Assessment Map** sheet contains a `VALIDATION CHECKS` block (rows 19–29) that fires the following automatically as cells are populated:

- Total course = 1000 points (LD-PIL-08, LD-GRD-01).
- Final exam 350–450 points (LD-PIL-08, LD-PIL-21).
- Midterm 100–200 points (LD-PIL-21).
- Act 1 ≤ 250 points (LD-PIL-08).
- Max 5 units with secondary assessment (LD-PIL-09).
- No secondary assessment > 15 points (LD-PIL-10, LD-GRD-02).
- Midterm unit and Final unit are exam-only (LD-PIL-09).
- ≥ 9 exam-practice primaries (LD-PIL-09 problem set requirement).
- Skill benchmarked in Unit 6 (course-level template requirement).

**Architectural implication.** When the template carries formula-based validation, it satisfies *both* G1 (structural slot for the generator) and D1 (mechanical check for the discriminator) for those specific standards. We don't need a separate `check_course_plan.py` script for these items — the template enforces them at fill time, and the AI can read the validation cells to know whether the plan compiles.

The auditor (D2) still runs over the rest — PLO formula well-formedness, CLO roll-up to PLO, Bloom's verb selection, etc. — anything that requires judgment.

## Course plan template structure

**LO Map sheet** (4 columns):

| Column | Meaning |
|---|---|
| LO Type | `PLO`, `CLO`, or `KLO` |
| # | Sequential number within type (e.g., `1.0`, `2.0`) |
| Parent | `PLO` for CLOs; `CLO-N` for KLOs |
| Description | The LO text |

The reference template ships with 1 PLO, 9 CLOs, and example KLOs. **Note:** The current Pillars standard LD-PIL-04 specifies *max 8 CLOs*, but the reference template has 9. **Flagged for Greg — see "Divergences to resolve" below.**

**Assessment Map sheet** (16 columns):

| Column | Meaning |
|---|---|
| Unit | 1–14 |
| Act | 1, 2, or 3 |
| Rize Requirements | Pre-populated requirements per LD-PIL-07 fixed elements (Onboarding, MyCareer CM, midterm location, ROI Journey window, Cumulative Review, Final Exam) |
| CLOs Covered | Comma-separated CLO numbers |
| KLOs Covered | Comma-separated KLO numbers |
| Skill | Skill benchmarked in this unit (course-template-driven) |
| Primary Assessment | Activity name (controlled vocabulary from Pillars Appendix B) |
| Primary Pts | Points |
| Secondary Assessment | Activity name |
| Secondary Pts | Points |
| Career Milestone | CM name (if any) |
| CM Pts | Points |
| Unit Pts | `=SUM(primary + secondary + CM)` |
| Cumulative Pts | Running sum down the column |

Plus the validation block in rows 19–29 (above).

## Question bank template structure

**Multiple Choice sheet** (18 columns):

```
Q-ID | Unit | KLO | Allocation | Review Status | Points | Question Type |
Stem | Option A | Option A Comment | Option B | Option B Comment |
Option C | Option C Comment | Option D | Option D Comment |
Correct Answer | Feedback Neutral
```

**Short Answers sheet** (14 columns):

```
Q-ID | Unit | CLO | KLOs | Tier | Allocation | Review Status | Points |
Question Type | Question | Feedback Neutral | Model Answer | Key Points |
Explanation
```

**Long Answers sheet** (14 columns): identical to Short Answers.

These match v3's master CSV structure with two notable omissions — see "Divergences to resolve" below.

## Divergences resolved

Both flagged divergences from earlier are now resolved per Greg's call.

### CLO count: max 8 enforced — RESOLVED 2026-04-25

The canonical `templates/source/course-plan-exam.xlsx` previously shipped with 9 CLOs (CLO 1.0–9.0). LD-PIL-04 specifies "Maximum 8 CLOs per course." Per Greg's instruction to enforce the max-8 rule, the template now ships with **8 CLO rows** (CLO 9.0 removed). KLO rows shifted up; assessment-map validation formulas reference the assessment-map sheet only and were unaffected by the LO-map row removal.

### Question bank schema: keep v3's extra columns — RESOLVED 2026-04-25

The new question bank xlsx template ships without the `Annotations` and `Scenario Tag` columns that v3's CSV schemas carried. Per Greg's call: keep both columns in the framework's internal CSV schema; downstream applications strip them as needed.

This means the framework's working schema is a **superset** of the xlsx export schema:

- **Internal CSV schema** (in `data/master-{mc,sa,la}.csv`): the xlsx columns + `Annotations` + `Scenario Tag`.
- **xlsx export to Sanity / instructor-facing surfaces**: the canonical xlsx columns; `Annotations` and `Scenario Tag` stripped.

`Annotations` continues to support walkthrough scripting per LD-PIL-14. `Scenario Tag` continues to support cross-exam dedup checks per the v3 ID feedback. Neither imposes any cost on consumers that don't need them — they're stripped on export.

## Canonical template extensions (applied 2026-04-25)

Per Greg's go-ahead on 2026-04-25, three extensions were applied to `templates/source/course-plan-exam.xlsx`. The canonical template now matches the patterns observed in the chemistry exemplar plus the full unified-plan structure from the ideal conversation.

### Course Narrative sheet (now first sheet)

A new sheet was added — Course Narrative — placed **before** LO Map and Assessment Map because it's the upstream artifact in the build sequence. Six sections, each with bold heading + italic gray guidance + placeholder content slot:

1. **Course Opinion** — Two or three sentences a student might say after a great version of the course. Non-obvious, punchy, slightly surprising.
2. **Act 1 Narrative — Onboarding (Units 1–3)** — 100–200 words framing what the act teaches and the cognitive shift students make.
3. **Act 2 Narrative — Core (Units 4–11/12)** — 100–200 words on what Act 2 builds on and what new thing it teaches.
4. **Act 3 Narrative — Final (Units 12/13–14)** — 100–200 words on what Act 3 delivers (cumulative review + final for Exam-Primary; final project + Async Fair for Project-Primary).
5. **What to Avoid** — The specific failure mode this course is most at risk of. Naming it explicitly helps the AI and ID guard against it.
6. **What Success Looks Like** — Specific student behaviors or outcomes that signal the opinion landed.

Sections 5 and 6 were added beyond the chemistry exemplar's structure because they were explicit pieces of the unified plan recommendation in the ideal conversation but missing from the chemistry tab. The canonical template now ships with all six.

### Richer Assessment Map columns

Three columns added:

- **`Topic`** at column D (between `Rize Requirements` and `CLOs Covered`). Per-unit topic name. Carries LD-PIL-13 step 1 (Course Topics).
- **`Primary Resource / Textbook Ch.`** at column P (after `Cumulative Pts`). Per-unit primary resource pointer. Carries LD-PIL-12 (Primary Instructional Resource Identification).
- **`Unit Description`** at column Q. Rich, voice-y unit framing in 100–200 words. Carries the unit-description content per `inputs/unit-descriptions.csv` in v3.

The validation formulas in rows 19–29 were updated to track the column shift — `Total=1000` now references `N17`, `Final 350–450` references `I15`, `Act 1 ≤ 250` references `SUM(N2:N4)`, and so on. All ten validation checks verified post-update.

### LO Map `Notes` column

A fifth column `Notes` was added to the LO Map sheet. Optional; captures author thinking during plan construction; gets stripped on export.

### What didn't change

- The 8 CLO row count (LD-PIL-04) is preserved.
- The 14 unit row count is preserved.
- The Assessment Map's pre-populated `Rize Requirements` per LD-PIL-07 fixed elements (Onboarding U1, MyCareer U3, Midterm U7 default, ROI Journey U7-9 window, Cumulative U13, Final U14) is preserved.
- All validation checks in the `VALIDATION CHECKS` block evaluate the same logic, just with shifted column references.

### Final canonical column layout

**Course Narrative sheet** (single column, multiple rows): Title; Course Opinion + guidance + slot; Act 1 Narrative + guidance + slot; Act 2 Narrative + guidance + slot; Act 3 Narrative + guidance + slot; What to Avoid + guidance + slot; What Success Looks Like + guidance + slot.

**LO Map sheet** (5 columns):

```
LO Type | # | Parent | Description | Notes
```

**Assessment Map sheet** (17 columns):

```
Unit | Act | Rize Requirements | Topic | CLOs Covered | KLOs Covered | Skill |
Primary Assessment | Primary Pts | Secondary Assessment | Secondary Pts |
Career Milestone | CM Pts | Unit Pts | Cumulative Pts |
Primary Resource / Textbook Ch. | Unit Description
```

Plus the `VALIDATION CHECKS` block in rows 19–29.

## What still needs to be built

Templates not yet in hand. Some inherit from v3's `templates/` folder; others are new for v4.

- `course-plan-project.xlsx` — Project-Primary version of the course plan template. Different Act 2 boundary (ends at Unit 11), different fixed elements (Final Project Proposal, Draft, Submission, Async Fair).
- `assignment-template.md` — Per LD-ACT-03's Three-Heading Structure (Turn In / Context / Steps). With LD-ACT-04, 05, 06 quality requirements as embedded slots.
- `problem-set-template.md` — Per LD-PIL-19's three required components (Learn assignment page, downloadable file, grading guide).
- `exam-template.md` — Per LD-PIL-21's three-section structure (Auto-gradable / Short Response / Long Response with PLO-mapped final).
- `quiz-template.md` — Per LD-QZ-01 (auto-gradable types only) and LD-PIL-15 (progress quiz with prior-unit retrieval cadence).
- `worked-example-template.md` — Per LD-PIL-14's 4–5 worked examples per question type for Exam-Primary.
- `quick-reference-template.md` — Per LD-PIL-14's quick-reference resource requirement.
- `resource-page-templates/` — One per LD-RES-04 tier (Tier 1 / 2 / 3, with structure per QA-07).
- `discussion-template.md` — Per LD-DSC-01 (subjective, open-ended) and LD-DSC-02 (responses optional default).
- Per-tier rubric scaffolds (3-level for discussion / participation, 6-level for assignments per LD-GRD-03 + 04 + 05).

Plus review-level templates:

- `persona-finding.md` — Reviewer finding format used by D3 (persona filter).
- `audit-finding.md` — Finding format used by D2 (standards auditor).
- `validation-finding.md` — Finding format used by D4 (SM validator).

## How templates encode standards

Two patterns:

**Hard structural slots.** Discrete, finite values become fixed slots. The course plan template's LO Map has bounded CLO rows; the Assessment Map has a calculated total that must equal 1000; activity-type fields are constrained to controlled vocabulary (Pillars Appendix B).

**Forced fields.** "Must include X" standards become non-optional slots. LD-PIL-19's three problem-set components become three required output files in the problem-set template. LD-ACT-03's three headings become three required sections in the assignment template.

Where the template is a spreadsheet, formulas can carry mechanical checks at fill time (the course plan template does this extensively). Where the template is a markdown document, the structural enforcement is by-presence (the slot exists; the AI must populate it) and the mechanical check happens in scripts (D1).

## What templates can't enforce

Pedagogical judgment. The persona filter (D3) and standards auditor (D2) handle that. See `standards/standards-architecture.md`.

## What does NOT live here

Pre-filled per-course content (this course's actual lo-map.csv with KLOs populated) lives in `design/`, not `templates/`. Templates are header-only or scaffold-only — except where the spreadsheet has illustrative example content for clarity, which gets stripped before use.

Greg's existing illustrative course outputs from the v3 chemistry build are **not** exemplars — they are useful as formatting references only. Real exemplars (course plans and assignments calibrated to Greg's house style for "what good looks like") are coming separately.

## Status

Two templates in hand (course plan, question bank). All other templates are TBD. Compile script (`scripts/compile_standards.py`) is also TBD. The first concrete next step is drafting the `assignment-template.md` and `problem-set-template.md` against the standards, since those are the high-volume per-unit templates.
