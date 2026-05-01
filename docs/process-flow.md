# Process Flow

**Status:** Canonical 2026-04-25.
**Status of implementation:** Architecture mapped; agents and scripts being built.

This document describes how the framework operates end-to-end from kickoff to upload, including when each discriminator (D1 scripts, D2 standards auditor, D3 persona filter, D4 SM validator) fires and what files each phase produces. It defers to `docs/ideal-interaction.md` for the conversational shape and `standards/standards-architecture.md` for what each discriminator does.

The framework is one conversation between an ID and the course-builder AI. Internally, the AI is a thin conductor that routes between specialized sub-agents and discriminators. Each phase produces locked artifacts. Discriminators run in isolated context windows at specific gates. The ID sees only what survives the discriminators, with notes on what the AI changed.

---

## Entry points — IDs may start anywhere

Not every ID arrives with a blank slate. An ID may show up with:

- Nothing but a syllabus and textbook.
- A vision but no plan.
- A completed course plan (xlsx with Course Narrative + LO Map + Assessment Map).
- A plan plus exemplar assignments already drafted.
- A full course needing only validation and upload.

The conductor's first job at kickoff is to **inventory `design/`** and propose where to start. Phases below are sequenced for a fresh-start build; an ID arriving partway through skips the phases whose artifacts already exist (with the conductor confirming each existing artifact is current and locked, not stale).

A locked-artifact entry point means the framework treats the artifact as input, not regenerates it. The discriminators still run on locked-as-input artifacts during downstream phases — the ID gets findings even on work the framework didn't produce — but the AI doesn't try to redraft them.

```
Inventory at kickoff:
  /design/course-vision.md       → if missing, run Phase 1
  /design/research-summary.md    → if missing, run Phase 2
  /design/course-plan-exam.xlsx  → if missing, run Phase 3 + 4
  /design/build-notes.md         → if missing, run Phase 5
  /design/exemplars/*            → if missing, run Phase 6 (+ optionally 7)
  /output/unit-NN/*              → if missing for a unit, run Phase 8 for that act
```

If an artifact exists but is older than the design lock manifest indicates, surface it to the ID for confirm-or-redraft.

---

## Phases

### Phase 0 — Kickoff and intake

**ID does:** Brings syllabus, textbook reference, any other context.
**AI does:** Confirms what was received. Inventories `design/` to determine entry point. Frames the path forward.
**Discriminators:** None.
**Artifacts produced:** None. (The conductor reads what's there.)

### Phase 1 — Vision elicitation

**ID does:** Describes their vision in their own words, or defers if they don't have one.
**AI does:** Asks at most one focused clarifying question if the response leaves an ambiguity that would mis-shape the plan.
**Discriminators:** None. (Vision is captured intent, not generated content.)
**Artifacts produced:** `design/course-vision.md` (or `design/course-vision.md` containing "deferred").

### Phase 2 — Research

**ID does:** Optionally steers research with specific angles.
**AI does:** Offers a default research plan or takes ID steering. Executes research (subject overview, comparable courses, novel approaches, common pitfalls, source list with annotations). Produces a research summary.
**Discriminators:** None. (Research is informational input the ID reviews in the next phase.)
**Artifacts produced:** `design/research-summary.md`.

### Phase 3 — Course plan recommendation

**ID does:** Reacts to the recommendation; iterates with AI; locks.
**AI does:** Drafts the unified plan recommendation: Course Opinion + Act 1/2/3 Narratives + What to Avoid + What Success Looks Like, with research citations. Surfaces with notes on what the persona filter caught and how the AI revised.
**Discriminators that fire:**

- **D3 Persona filter (Online Skeptic)** — internal pass on the recommendation before ID sees it. AI applies light iterations based on findings. Findings of `BLOCKER` severity escalate to ID rather than burning iterations.

**Artifacts produced:** `design/course-plan-recommendation.md` (the AI's draft, locked once ID approves).

### Phase 4 — Course plan build (xlsx)

**ID does:** Reviews populated plan; iterates; locks.
**AI does:** Populates the LO Map (1 PLO, ≤8 CLOs, KLOs) and Assessment Map (14 units with Topic, CLOs/KLOs covered, Skill, Primary/Secondary Assessment, points, Career Milestone, Primary Resource, Unit Description) sheets of the xlsx. Reads the Course Narrative populated in Phase 3. Surfaces with notes on what each discriminator caught.
**Discriminators that fire:**

- **D1 Mechanical scripts.** `check_course_plan.py` mirrors the spreadsheet's `VALIDATION CHECKS` formulas in Python (Total = 1000, Final 350-450, Act 1 ≤ 250, max 5 secondary units, etc.). Runs as the AI populates cells; provides immediate feedback.
- **D2 Standards auditor (course-plan scope).** Single batch pass after course plan is populated. Audits Parts 1–3 of standards (Online Skeptic principles, syllabus standards, course plan standards). Catches things scripts can't: PLO well-formedness, CLO roll-up to PLO, Bloom's verb selection, alternative-final-arc components if used, Career Milestone placement, and so on.

**Artifacts produced:** Fully populated `design/course-plan-exam.xlsx` (Course Narrative + LO Map + Assessment Map).

### Phase 5 — Build notes

**ID does:** Reviews; locks.
**AI does:** Surfaces research and conversation insights that didn't fit the course plan. Specific case studies to use, unit-specific guidance, scenario recommendations, cross-unit connections to surface in `Feedback Neutral` fields, calibration notes for the assignment-builder.
**Discriminators:** None. (Captured intent, not generated content to audit.)
**Artifacts produced:** `design/build-notes.md`.

### Phase 6 — Exemplar assignments

**ID does:** Reviews each assignment; iterates; locks.
**AI does:** Proposes 2 exemplar assignments at different points in the course's trajectory (default 2; reasons up to 3 when course diversity warrants). For each: reads standards subset (Parts 1 + 4) and the calibration corpus (`research/exemplars/`), articulates analysis, generates against `templates/assignment-template.md`, runs internal self-check. Surfaces with notes on filter and auditor findings.
**Discriminators that fire (per assignment):**

- **D3 Persona filter (Online Skeptic)** — reads the assignment as a real student. Catches clarity / path / purpose / sequence test failures.
- **D2 Standards auditor (assignment scope)** — audits assignment-level standards: LD-ACT-03 three-heading structure, LD-ACT-04/05/06 quality (Turn In/Context/Steps), LD-PIL-19 problem-set structure if applicable, LD-ASG-01 assignment guidelines, LD-RES-04 resource tier marking, LD-PIL-18 word counts, etc.

**Artifacts produced:** `design/exemplars/exemplar-assignment-unit-NN.md` per assignment.

### Phase 7 — Exemplar units (optional)

**ID does:** Picks one or both assignments to extend; reviews; locks.
**AI does:** Extends each chosen assignment into a full unit build: problem set components (Learn page + downloadable file + grading guide per LD-PIL-19), walkthrough rows for the question bank (LD-PIL-14 4–5 worked examples per question type), exam-bank rows, quick-reference resource (LD-PIL-14), unit description.
**Discriminators that fire:**

- **D3 Persona filter** on the unit-as-whole.
- **D2 Standards auditor (unit scope)** on the unit's components.
- **D4 SM validator (first appearance)** — sampled blind-context factual validation on the question bank rows. Per v3's `validate_batch.py` mechanic: independent answer + AGREE/REASONING DIVERGENCE/SUBSTANTIVE DISAGREEMENT classification.

**Artifacts produced:** `design/exemplars/exemplar-unit-NN/` directory containing all unit files.

### Phase 8 — Per-act build

**ID does:** Asks for "build all of Act 1" (or any act). Reviews aggregated findings at end of act; iterates; locks the act.
**AI does:** Builds units in sequence within the act, calibrating against locked exemplars (which serve as in-context exemplars). Each unit: reads relevant slice of plan + build notes + standards subset, generates against templates, internal self-check.
**Discriminators that fire:**

- **Per unit during act:** D1 scripts (`check_unit.py`) for mechanical structural validation; D3 persona filter, lighter pass since calibration is locked.
- **At end of act:** D2 standards auditor batch fires on Part 4 standards across the act's units, sampled. D4 SM validator batch fires, also sampled.

The end-of-act batches are the heavy passes. Per-unit during the act is a lighter rhythm.

**Artifacts produced:** `output/unit-NN/` for each unit in the act, plus `reviews/standards-audit/act-N-audit.md` and `reviews/subject-matter/act-N-validation-report.md`.

### Phase 9 — Full course completion

**ID does:** Reviews remaining acts as they complete (same as Phase 8). At end of all acts, reviews release-gate findings; approves release.
**AI does:** Runs Phases 8 for remaining acts. At release, runs final pass.
**Discriminators that fire at release:**

- **D2 Standards auditor (release scope)** — full-course audit reading all units against the locked course opinion. Reads the locked Course Narrative + sampled artifacts from each act and asks: does the work visibly build toward the opinion?
- Optional: course-opinion auditor as a specialized D2 mode if release-gate findings warrant deeper opinion-drift analysis.

**Artifacts produced:** `reviews/release/full-audit.md`.

### Phase 10 — Sanity upload

**ID does:** Invokes the (separately-developed) Sanity upload script.
**AI does:** Nothing — this is out of scope.
**Discriminators:** None.
**Artifacts produced:** Sanity-side records.

---

## Discriminator firing summary

| Phase | D1 Scripts | D2 Auditor | D3 Filter | D4 SM Validator |
|---|---|---|---|---|
| 0 — Kickoff | — | — | — | — |
| 1 — Vision | — | — | — | — |
| 2 — Research | — | — | — | — |
| 3 — Plan recommendation | — | — | per artifact | — |
| 4 — Plan build (xlsx) | continuous | once at end | — | — |
| 5 — Build notes | — | — | — | — |
| 6 — Exemplar assignments | per artifact | per artifact | per artifact | — |
| 7 — Exemplar units | per artifact | per artifact | per artifact | sampled |
| 8 — Per-act build | per unit | once per act (sampled) | per unit (light) | once per act (sampled) |
| 9 — Release | — | once at release | — | — |
| 10 — Sanity upload | — | — | — | — |

D1 scripts are cheap and continuous. D3 persona filter is the most frequent LLM-based discriminator — once per major artifact, throughout the build. D2 auditor and D4 SM validator are batched at gates to manage cost and cognitive load on the ID.

## ID interaction summary

Roughly 8–10 review-and-lock moments across a complete build:

1. Kickoff confirmation.
2. Vision elicitation (if not deferred).
3. Research direction (optional steering).
4. Course plan recommendation lock.
5. Course plan build lock.
6. Build notes lock.
7. Exemplar assignments lock (1 or 2 rounds depending on assignment count).
8. Exemplar units lock (if extended).
9. Per-act locks (3 acts).
10. Release approval.

Per the framework's goal 7 (ID cognitive load), the ID's time goes to judgment, not to managing AI conversations. Discriminators handle the work between locks; the AI surfaces only the iterated version with notes.

## Where each artifact lives

```
{course-id}/
├── design/                               # Phase 0–7 artifacts
│   ├── course-vision.md                  # Phase 1
│   ├── research-summary.md               # Phase 2
│   ├── course-plan-recommendation.md     # Phase 3 (working draft)
│   ├── course-plan-exam.xlsx             # Phase 3 + 4 (locked artifact)
│   ├── build-notes.md                    # Phase 5
│   ├── exemplars/
│   │   ├── exemplar-assignment-unit-02.md
│   │   ├── exemplar-assignment-unit-10.md
│   │   └── exemplar-unit-02/
│   │       └── *
│   └── design-lock.md                    # Manifest (artifact hashes + timestamps)
├── output/                               # Phase 8 artifacts
│   ├── unit-01/
│   ├── unit-02/
│   └── ...
├── reviews/                              # Discriminator outputs
│   ├── persona-filter/
│   │   ├── course-plan-recommendation-online-skeptic.md
│   │   └── ...
│   ├── standards-audit/
│   │   ├── course-plan-audit.md
│   │   ├── act-1-audit.md
│   │   └── ...
│   ├── subject-matter/
│   │   └── act-1-validation-report.md
│   └── release/
│       └── full-audit.md
└── data/                                 # Working CSVs (framework-internal schema)
    ├── master-mc.csv
    ├── master-sa.csv
    └── master-la.csv
```

The framework-level files (`standards/`, `personas/`, `prompts/`, `templates/`, `scripts/`, `research/`) live at the framework root; per-course content lives in the course's own folder.

---

## What's not in this document

- The exact contents of each prompt — see `prompts/`.
- The exact validation rules — see `standards/standards-architecture.md` per-standard mapping.
- The Sanity upload schema — out of scope; ID-team-owned.

When the framework is operationalized, this document gets updated to reflect what was implemented vs. designed.
