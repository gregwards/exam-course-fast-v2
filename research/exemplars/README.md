# `research/exemplars/` — Calibration corpus

This folder holds **real exemplars** the framework calibrates against — Greg's curated work, not the v3 chemistry build outputs.

The framing matters. The v3 chemistry artifacts in the sibling repo are **formatting references only** — useful as "this is what an answer key file looks like, structurally" but explicitly not as exemplars for tone, voice, opinion, or "what good looks like at the pedagogical level." That distinction comes from Greg's working session note that v3 produced a soulless course (the v3 ID feedback is captured in `docs/v3-id-feedback-signal.md`). Calibrating prompts against soulless work would propagate the soullessness.

The exemplars in this folder are different. They're the work Greg points to as "what good aims at."

## What lives here

### `course-plans/`

- **`CHEMY_I_Course_Plan_EXAM.xlsx`** — Chemistry I course plan (Exam-Primary). Has the **Course Narrative** tab with explicit Course Opinion + Act 1 / Act 2 / Act 3 narratives. The course opinion: *"You're going to learn a lot of formulas in this class, but chemistry is more than just a list of formulas to memorize. Chemistry is a set of models you build, layer by layer, until you can predict what matter will do before you touch it."* Greg's note: this one *includes an opinion and narrative* — closer to what the framework should aim at than the POSCI exemplar.
- **`POSCI_I_Course_Plan.xlsx`** — Political Science I course plan (Exam-Primary). Same column structure but no Course Narrative tab. Greg's note: *"could do with a bit more creativity."*

Both have failures in their built-in `VALIDATION CHECKS` blocks (POSCI fails on `Max 5 units w/ secondary`, `No secondary > 15 pts`, `>=9 exam-practice primaries`, `Skill benchmarked in Unit 6`; CHEMY fails on `Skill benchmarked in Unit 6`). This is *informative*: real exemplars are themselves in violation of some standards, presumably awaiting resolution. The framework's auditor should treat validation FAIL as "needs review" and surface it, not as a hard block on a plan that's otherwise good.

### Patterns observed across exemplars

These are observations the framework should incorporate, either into the canonical template or into the agent prompts:

**Course Narrative tab (chemistry only).** Three sections — Course Opinion, then Act 1 Narrative, Act 2 Narrative, Act 3 Narrative. The opinion is two-to-three sentences in the voice of a confident teacher addressing a student. The act narratives are 100–200 words each, framing what the act teaches and why it builds toward the next.

**Richer Assessment Map columns than the bare template.** Both exemplars add columns the canonical template doesn't have:

- `Topic` — Unit-level topic name (e.g., "The Mole Concept & Stoichiometry")
- `Primary Resource / Textbook Ch.` — Per-unit primary resource pointer (e.g., "Ch 3.1–3.2; Ch 4.1–4.2")
- `Unit Description` — Rich, voice-y unit framing in 100–200 words
- (POSCI also has `Assignment Ideas`)

These columns carry the LD-PIL-12 (Primary Instructional Resource) and LD-PIL-13 (Course Topics) requirements directly into the course plan artifact. Worth adding to the canonical template.

**LO Map `Notes` column (chemistry).** Captures author thinking — *"I'm not sure what you mean by 6. How is this different than..."*; *"Should we expand this to include dsp3 and d2sp3 as well?"* Useful as a working column during plan construction; gets stripped on export.

**Inline Topic + Exam markers in unit names (POSCI).** Unit names like "Civil Rights & Political Equality" or "Civil Liberties / Exam 1 (Units 1 - 3)". The exam scope is captured in the unit name when it falls in that unit. This works but is brittle — better to have a dedicated column for exam scope.

**Voice patterns across Unit Description fields.**

From CHEMY Unit 2:
> "This is the first real teaching week, and the workload is intentionally lighter than what follows. Coming out of onboarding, you're getting your bearings..."

From CHEMY Unit 6:
> "The focus shifts from how much to what is inside. Where Act 1 asked you to count and convert, this unit asks you to look inside the atom..."

From POSCI Unit 9:
> "Unit 9 gave you the actors: parties and interest groups. Now you'll study the arena where they compete..."

Patterns: second person, direct, references prior units explicitly to build through-line, names the cognitive shift (counting → looking inside; theory → application), avoids hype language ("intentionally lighter" not "easy"; "you'll go deeper" not "exciting deep-dive"). This voice profile is what the unit-description-writer prompt should aim at.

## What the framework does with these exemplars

When the course-designer prompt drafts a plan recommendation, it reads these exemplars (or extracts from them) as in-context calibration. Same for the unit-description writer, the build-notes writer, and the persona filter (which reads "would the Online Skeptic find this clear?" against the calibrated voice).

`compile_standards.py` (TBD) does *not* derive from these exemplars — it derives from `standards/standards.md` and `standards/rize-lds.md`. Exemplars are a separate calibration input read at prompt construction time, not at standards compile time.

## What's missing

These two course plans calibrate the upper layer of the framework (course planning). The exemplars still needed for downstream calibration:

- **Strong assignments** at different scaffolding tiers (early-act foundational + late-act applied) — calibrate the assignment-builder prompt.
- **Strong problem sets** (per LD-PIL-19's three-component structure) — calibrate the problem-set-builder prompt.
- **Strong worked examples** (per LD-PIL-14's 4–5-per-question-type) — calibrate the worked-example-builder prompt.
- **Strong exam structures** (per LD-PIL-21's three sections) — calibrate the exam-builder prompt. (The reference exam question bank template is a starting point but not yet a populated exemplar.)
- **Strong unit overviews / context sections** — calibrate the context-writer prompt against LD-ACT-05.

When these are shared, they live in `research/exemplars/{type}/`.

## Calibration altitude — what these exemplars are and aren't

Greg's framing on 2026-04-25:

> *"The reality is we've been building fast, we don't have truly deep, thoughtful versions, so I just want to be clear about where they are from a standards perspective."*

These exemplars are **standards-compliance reference**, not creative-ceiling reference. They show:

- How a real Pillars course plan looks structurally (column layout, validation block, controlled vocabulary in use).
- How CLOs and KLOs roll up to a PLO at a real subject scale (7 CLOs, ~43 KLOs in both cases).
- How Rize Requirements (Onboarding U1, MyCareer U3, Midterm default U7, ROI Journey U7-9, Cumulative U13, Final U14) get populated.
- Voice patterns in real Unit Description fields (second person, direct, references prior units, names cognitive shifts).
- The Course Narrative pattern (chemistry only) — opinion + three-act narratives.

They do *not* show:

- The creative ceiling. Both course plans were built fast under volume pressure (the same conditions the v3 ID feedback flagged). Calibrating the course-designer prompt to match these exactly would replicate "fast, fine, not great."
- A locked Project-Primary equivalent (both are Exam-Primary).

When we build the calibration corpus that prompts derive from, treat these exemplars as the **floor for standards adherence and structural shape**, and treat the prompt's pull-toward-better — anchored in `personas/online-skeptic.md`, the `standards/standards.md` rationale fields, and the structured-creative-process commitment to soul-not-flatness — as the mechanism for clearing the floor.

When stronger exemplars arrive (or when the framework produces work above this floor through pilot runs), update the calibration corpus and treat those as the new ceiling.
