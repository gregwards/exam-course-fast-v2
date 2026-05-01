# MATHS-VIII — Exemplar Annotations

Guidance accumulated through the Phase 6 exemplar builds (Unit 2 locked, Unit 11 in lock review). Every Phase 7 (exemplar unit) and Phase 8 (per-act unit build) reads this document alongside `course-principles.md`, `build-notes.md`, and the locked `course-plan-exam.xlsx` before drafting unit content. These are the structural and stylistic norms the exemplars established for the rest of the course.

---

## Architecture: bank as the source of truth

Every question in the course lives as a row in `data/master-questions.xlsx`. The bank is the database. Problem-set, answer-key, and learn-page markdown files are **projections** rendered from the bank by deterministic Python scripts. Same bank state in → same projection out. **Do not hand-edit the rendered projections.** If a question needs changing, edit the bank row and re-render.

The bank has three sheets:

- **Multiple Choice** — 19 columns. For Quiz and Exam Bank allocations only (LD-PIL-16). Never used in problem sets.
- **Short Answers** — 16 columns. SA-shaped questions for Problem Set, Walkthrough, or Exam Bank.
- **Long Answers** — 16 columns. Same shape as SA; multi-part `(a)/(b)/(c)` questions.

Q-IDs are sequential within sheet (`SA-001`, `SA-002`, …; `LA-001`, `LA-002`, …; `MC-001`, `MC-002`, …). The bank's `Allocation` × `Question Type` constraint and the `Scenario Tag` uniqueness check are enforced by `scripts/check_question_bank.py` at every write. Run that check after any bank edit and before rendering.

---

## Problem-set composition

### Per-unit shape rules

- **Units 1, 2.** Exempt from the count rule (LD-PIL-19A treats them informationally). May have fewer than 4 problems if scaffolding warrants.
- **Units 3+.** Minimum 4 problems. SA/LA mix required when both are pedagogically appropriate.
- **Units 8+.** Strict exam-mirror shape. Problem 1 always SA (Section 2 mirror). At least one LA somewhere in the set (Section 3 mirror). Default 2 SA + 2 LA; choose 3 SA + 1 LA for interpretation-heavy units (Unit 8 correlation, Unit 13 review) or 1 SA + 3 LA only when the unit must land three full workflows.

The course's overall mix should be SA-heavier than LA by count, mirroring the final's 5 SA + 4 LA non-AG portion.

### Per-problem sizing

Per-problem effort ratio is roughly 1:2.5 (LA = 2.5× the work of SA). For a 30-pt budget:

- SA: ~5–8 pts each, ~10–15 min student work, 2–3 sub-parts.
- LA: ~8–12 pts each, ~25–30 min student work, 4–5 sub-parts.

Total problem-set time: 60–90 minutes for a 30-pt primary. Stay within the unit's locked workload contract from the build notes.

---

## Problem set (student-facing) format

Render with this top-section structure:

1. **Title.** `# Unit N Problem Set: [Topic]`. Period-separated subtitle if needed; no em-dashes.
2. **Resources** (bulleted). Textbook chapters and sections, simulation tools (StatKey URL), reference tables (standard normal, t-distribution).
3. **Questions** (bulleted). One bullet for the count of short responses, one for the count of long responses.
4. **Showing your work** (bulleted). Per-deliverable guidance: how to present hand calculations, simulation outputs, hypotheses, and interpretations.
5. Per-problem sections.

**Do not include on the student-facing problem set:**

- Point values per problem (those live in the answer key)
- Time estimates per problem or for the set
- Pedagogy explanations ("This problem set follows the OpenIntro Section 5.2 pedagogy...")
- A separate "Submission expectations" line per problem (collapse into the top-section Showing-your-work block)
- Per-problem Show-your-work appendices (the top-section block covers all of them)

### Per-problem prose

Each problem renders as:

```
## Problem N: [Short|Long] Response

[Optional narrative setup. The historical or real-world story the prompt builds on, embedded in the prompt itself per the narrative-in-problems course principle. Don't quarantine motivation into a Context section that students might skip.]

[Setup paragraph: the scenario, given numbers, what they observed.]

(a) [First sub-question. Action-oriented, unambiguous about what's being asked, names the deliverable shape inline if non-obvious.]

(b) [Second sub-question. Same.]

(c) ...
```

Each part should be self-contained and unambiguous **without** a separate Show line. If the question prose is unclear, fix the prose, don't add an appendix. Examples of clarity moves:

- "Compute the formula by hand using SE = √(p̂(1 − p̂)/n). Plug in p̂ = 0.47 and n = 1,200." — formula given, substitution required.
- "In two to three sentences, explain in plain language what this number means in the context of the experiment. Avoid the textbook phrasing 'probability of the data given the null'." — length specified, forbidden phrasings named.
- "In StatKey, open 'Test for Single Proportion.' Set the null proportion to 0.5 and observed count to 8 successes out of 8 trials. Run 10,000 simulated guesses with the 'Right Tail' option and report the proportion." — exact tool, exact settings, exact deliverable.

When a part requires a calculation that students should show, the prompt names it directly ("compute by hand," "show the formula and arithmetic") so the deliverable is implicit in the question.

---

## Answer key (instructor-facing) format

Renders with:

- Per-problem section header: `## Problem N: [Short|Long] Response (P pts), [Q-ID]`
- Q-ID, KLOs, CLOs, scenario tag in metadata block
- **Model answer** section with full worked answer for each part
- **Point breakdown** section listing per-part point allocation and what earns full vs. partial credit
- **Instructor explanation** section with pedagogical notes (the misconception target, the "bonus rigor" observation worth noting if students catch it, why this problem is shaped the way it is)
- **Annotations** section with build-time decisions worth surfacing for video walkthrough scripting (Phase 7) or future revision
- Total points line at the bottom

Point breakdowns must be specific and grading-deterministic. Per LD-PIL-19, problem sets use answer keys with point breakdowns, NOT rubrics. A student who gets parts (a), (b) right and (c) wrong should get a determinate score from the breakdown alone, without holistic instructor judgment.

---

## Learn page (Sanity, student-facing) format

Per `LD-ACT-03`, the learn page has exactly three sections in this order:

### 1. Turn In

What the student submits — deliverables, file formats, nothing else. Don't restate instructions from Steps. Per `LD-ACT-04`, keep it brief.

For MATHS-VIII (math course, hand calculation work), submission format is:

> Hand-written work for all problems, submitted as a single PDF or as photos (one image per page is fine). Label each part clearly. For tool-based parts (StatKey, etc.), include screenshots showing inputs and the reported result alongside your hand-written work.

Do **not** accept Word docs or markdown as primary submission format for problem-set work. Hand-written PDFs or photos only. Tool screenshots supplement the hand-written portion.

### 2. Context

Per `LD-ACT-05` and `LD-PIL-18`:

- **≤200 words.**
- Includes the scenario or framing, why the skill matters in the real world, how the unit connects to the course arc.
- Does NOT teach. Does NOT include how-to instructions, detailed requirements, definitions, or background knowledge.
- **Voice: instructor-natural, conversational.** Should sound like an instructor wrote it, not a templated AI summary.

**AI tells to avoid in Context:**

- The "X needs A. Y needs B. Z needs C. All three are inference questions..." opener pattern. Reads as AI-templated; cut it. Lead with substance.
- Self-questions ("What's the secret to inference?", "Here's the thing about p-values...").
- "Here's what's happening here" / "Let me walk you through" framings.
- Em-dashes (per assignment-copy-editor skill — absolute rule).
- Listing "two warnings, both of which..." structures. Cut the meta-framing; just say what the warning is.
- Forward-looking promises about what the problem set will rehearse. Steps section handles that.

**Test:** Remove the Context entirely. Do the Steps still work? If yes, Context is doing its job (motivating, not instructing). If Steps stop making sense without Context, you've put instruction in Context — move it to Steps or to a resource.

### 3. Steps

Per `LD-ACT-06`:

- Each step has a **short, descriptive title** (e.g., "Read OpenIntro on the foundations of inference").
- **Number the steps explicitly.** Use `### Step 1: Title`, `### Step 2: Title`, etc. The standard says the LMS handles numbering, but explicit numbering on the rendered markdown reads better and is preferred for MATHS-VIII.
- **Action-oriented.** Each step's body directs students to do something specific. Start with a verb.
- **Brief.** Maximum 3 sentences plus any list.
- **Resource-integrated.** Links to OpenIntro, StatKey, walkthrough videos, problem-set download — at point of need, inside the relevant step.
- **No teaching.** Background, definitions, or explanations belong in resources, not in Steps.
- **End with the submission step.** "Submit your completed work" is the last step.

**Don't specify counts that may change.** Avoid wording like "Watch the four walkthrough videos" — rephrase as "Watch the walkthrough videos" so a future content addition or removal doesn't invalidate the step. Same for resource counts.

**Don't number resources within steps unless ordering is essential.** Use bullets for unordered lists, ordered lists when sequence matters.

---

## Voice and copy-editing rules

These apply to all student-facing prose (problem set, learn page, walkthrough, exam items):

- **No em-dashes** (`—`, U+2014). Hard rule from the `assignment-copy-editor` skill. Use commas, periods, parens, or semicolons. En-dashes in numerical ranges (e.g., `5.2–5.3`, `[3.1, 11.6]`) are fine.
- **No self-questions or rhetorical "here's the thing" transitions.** Both are AI tells.
- **Sentence variety.** Mix short and long sentences naturally; avoid both formula-of-shorts and formula-of-longs.
- **Lead with concrete facts and named figures.** "In 1979, Bradley Efron worked out the bootstrap at Stanford" — not "the bootstrap is a powerful resampling technique that allows…"
- **No emojis** unless the user (i.e., the ID) asks for them.
- **No marketing words** ("amazing," "powerful," "elegant"). Describe the work; don't sell it.

---

## Forbidden phrasings to flag in interpretation problems

When a problem asks a student to interpret a statistical concept in plain language, name common misinterpretations explicitly as forbidden in the prompt:

- For p-values: forbid "probability of the data given the null" without a plain-language gloss. The prompt says: *"Avoid the textbook phrasing 'probability of the data given the null'; write it the way you'd explain it to a friend who hasn't taken statistics."*
- For confidence intervals: forbid "95% probability the true value is in this interval" framing. The prompt says: *"Note: avoid the misreading 'the true value is in this interval with 95% probability' — that's a Bayesian interpretation that doesn't match the frequentist procedure."*
- For correlation: forbid causal language without a hedge. The prompt says: *"Does the correlation tell you whether X causes Y? Justify your answer."*

Naming the wrong interpretation directly in the prompt is the misconception remediation. A student who can't articulate why the wrong reading is wrong hasn't transferred the concept.

---

## Real-world numbers and citations

When using real-world data or numbers, cite the source in the answer-key annotations and (when relevant) in the problem prompt itself. Examples accumulated so far:

- **Pfizer/BioNTech 95% efficacy** — Polack et al., NEJM, December 2020. Used in Unit 4 (binomial) and Unit 11 (vaccine-trial-themed permutation test).
- **Eddy 1982 mammogram-misinterpretation study** — Eddy, "Probabilistic reasoning in clinical medicine," 1982. Used in Unit 2.
- **People v. Collins, Sally Clark, Lucia de Berk** — referenced as exemplars of the prosecutor's fallacy in Unit 2 annotations (not surfaced to students; Phase 5 build-notes decision was no named cases).
- **Quetelet's Scottish soldiers** — Sur l'homme, 1835, with summary statistics from the Edinburgh Medical and Surgical Journal, 1817.
- **Tyler Vigen Spurious Correlations** — site, used in Unit 8.
- **ASA 2016 statement on p-values** — Wasserstein & Lazar, "On p-values: context, process, and purpose," The American Statistician, 2016. Used in Unit 11 misconception remediation.

When a unit's problem requires a real-world dataset that doesn't have a clean published source, build a synthetic dataset that's plausibly real-flavored and note the synthetic provenance in the annotations.

---

## Iteration discipline

When discriminator findings come back on an exemplar:

- **BLOCKER:** escalate to the ID directly. Don't burn iterations trying to auto-fix.
- **WARNING:** apply at most one revision pass before surfacing to the ID. Show the diff and the rationale.
- **NOTE:** pass through; informational only.

When the auditor cites a course principle, verify the citation is correct against the current `course-principles.md` (it may have been revised). The audit is correct against the principle as written; if the principle itself was over-reaching, the fix is to revise the principle (with ID sign-off), not to change the artifact.

---

## What this document is not

- **Not a substitute for the standards.** `LD-PIL-19`, `LD-PIL-19A`, `LD-PIL-21`, `LD-ACT-03` through `LD-ACT-07` are the canonical sources. This document operationalizes them for MATHS-VIII specifically.
- **Not a substitute for `course-principles.md`.** Course-level principles (audience, sequencing, vision anchors) live there; this doc covers per-exemplar structural and stylistic norms.
- **Not a substitute for `build-notes.md`.** Per-unit content guidance (which historical figure, which dataset, which misconception target) lives in the build notes.

When all three documents agree, follow them. When they conflict, the standards win, then `course-principles.md`, then `build-notes.md`, then this document.

---

## Revision history

- **2026-04-30 — Initial draft.** Written after Unit 11 exemplar lock review, capturing the structural and stylistic norms established by the Unit 2 and Unit 11 exemplar builds. Phase 7 and Phase 8 unit builds read this alongside the other design-layer documents.
