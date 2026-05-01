# Standards Architecture

**Status:** Working design 2026-04-25. Specifies how the standards in `standards.md` (Pillars-specific) and `rize-lds.md` (parent) integrate into the framework without compromising the soul, quality, diversity, or creativity of the output.

This is the document the framework's templates, scripts, prompts, and persona filter all defer to. It answers one question: **how do we enforce standards without strangling generation?**

The standards corpus has two layers: the parent **Rize LDS** (`rize-lds.md` — applies to all Rize courses) and the **Pillars-specific** layer (`standards.md` — adds to or replaces parent standards for Pillars courses). Per LD-GEN-04, accessibility is non-negotiable; otherwise, more specific standards win, with Pillars-specific overriding parent-general where the two address the same area.

---

## The tension we're resolving

Standards are constraints. Constraints applied to a generator, especially many constraints, produce constrained output. The Pillars standards have ~25 numbered items; the underlying Rize LDS has more. Inject all of them into the generator's prompt as rules-to-follow, and the AI produces structurally compliant, joyless work — exactly the "soulless course" failure the v3 ID feedback documented.

But standards exist for good reasons. The Online Skeptic doesn't have time for unclear instructions. Partners need predictable course architecture. Final exams need to be 350–450 points. Each standard has earned its place.

The architectural commitment that resolves this — standards live in **two places**:

> **Generator side: standards are seeded.** Hard structural slots in templates (prevention) and rationale-bearing standards subsets injected into agent prompts (awareness). The generator knows the rules and the why; it tries to comply.
>
> **Discriminator side: standards are checked.** Mechanical scripts catch the structural violations cheaply. A standards auditor (LLM, isolated context) audits codified-but-judgment-required standards. The persona filter catches experiential failures via the Online Skeptic lens. The SM validator catches factual errors. Each runs in isolated context per v4 §2.1.

This is consistent with the structured-creative-process commitment to "don't collapse generator and discriminator." The generator absolutely uses the standards while it works — that's how Phase 0's "define the discriminator first" feeds the generation phase. What the principle prohibits is *combining* generation and evaluation into one step. The generator generates with awareness; the discriminator evaluates with isolation.

---

## Generator side — standards seeded

The generator (the conversational AI shipping work) is *aware* of the standards while it generates. Two seeding mechanisms.

### G1 — Templates with embedded standards slots (prevention)

For structural standards with no creative latitude — point totals, controlled vocabulary, three-section exam structure, max 8 CLOs, 35–55 KLOs, problem set's three required components.

The template forces the standard at fill time. The agent cannot produce a course plan with 9 CLOs because the LO map template only has 8 rows. The agent cannot mis-spell "Career Milestone: MyCareer Onboarding" because the controlled vocabulary list is the dropdown.

**No drag on creativity** because there's no creative latitude in these slots anyway. Their values are discrete, finite, and defensible.

**Examples:**
- LD-PIL-04 (max 8 CLOs) → `lo-map.csv` template has 8 CLO rows.
- LD-PIL-08 (1000 points total) → `assessment-map.csv` template has a calculated `Total Points` field that must equal 1000.
- LD-PIL-16 (quiz vs. exam question types) → question templates have a `Question Type` slot constrained by allocation.
- LD-PIL-19 (problem set three components) → problem-set template has three required output files.
- LD-PIL-21 (three-section exam structure) → exam template has Section 1 / Section 2 / Section 3 scaffolding.
- Appendix B (controlled vocabulary) → activity labels are a fixed enum.

### G2 — Standards-as-prompt-context (awareness)

For standards that don't admit pure structural enforcement but the generator should know about while working — PLO formulas (LD-PIL-03), CLO Bloom's verbs and roll-up to PLO (LD-PIL-04), Act 1 onboarding requirements (LD-PIL-07A), exam-primary build sequence (LD-PIL-13), word count guidance (LD-PIL-18), problem set Steps purpose (LD-PIL-19), and so on.

Each agent prompt receives the standards subset relevant to its job, with rationale attached. The course-designer prompt receives Parts 1–3 of `standards.md`. The unit-builder prompt receives Parts 1 + 4. The persona filter prompt receives Part 1 alone (as evaluation criteria, not as generator constraints).

The Pillars standards' rule + rationale format is itself optimal for prompt context — per the prompt-architect skill, instruction + motivation outperforms naked instruction. When standards flow into prompts, the rationale comes along.

The generator works with awareness of these standards and tries to comply. The discriminator side then audits whether it actually did.

---

## Discriminator side — standards checked

Four discriminators, each with a different scope and cadence. All run in **isolated context windows** (separate sub-agent invocations) per v4 §2.1.

### D1 — Mechanical scripts

For numerical and structural standards that admit a single right answer — point budgets per act, word count limits, activity caps, retrieval cadence, chapter coverage, cross-exam dedup.

Scripts run after generation, fail loudly when violated. The agent generates freely; the script catches the violation; the agent revises with the violation surfaced. **Bounded iteration**: one revision pass before escalating to ID.

**No drag on creativity** because the agent isn't pre-constrained — it's free to make creative choices that happen to fall within the cap. **Cheap and fast** — these checks are simple Python, no LLM calls.

**Examples:**
- LD-PIL-08 (act point ranges, 300 pt/unit cap, final exam 350–450 pts) → `check_course_plan.py` and `check_unit.py`.
- LD-PIL-09 (max 2 activities/unit, max 2 non-pset units, max 5 secondary-assessment units) → `check_unit.py`.
- LD-PIL-15 (every third unit has progress quiz with prior-unit retrieval) → `check_unit.py`.
- LD-PIL-18 (200 word context, 100 word resource intro, 800 word signal threshold) → `check_unit.py`.
- LD-PIL-20 (35–55 KLOs per course) → `check_course_plan.py`.
- LD-PIL-04 (max 8 CLOs, Bloom's verbs from approved list) → `check_course_plan.py`.
- v3-feedback-derived checks: cross-exam dedup, chapter coverage, exam item count caps.

### D2 — Standards auditor (LLM, isolated context, batched)

For codified standards that require some judgment — does this PLO actually follow the formula? does each CLO visibly contribute to the PLO? does Act 1 satisfy the three onboarding requirements? does this unit's quick-reference resource match the standard's format expectations?

The standards auditor reads the artifact section-by-section against the codified rules, in the role of a compliance reviewer. Different from the persona filter (which reads as a real student); different from the SM validator (which reads as a textbook expert checking facts).

**Cadence — most fires at course plan time, smaller batch per act.**

- **Course plan audit.** Fires once after the course plan is drafted, before the AI surfaces it to the ID. Audits Part 1 (principles) + Part 2 (syllabus standards) + Part 3 (course plan standards). Catches LD-PIL-02 through LD-PIL-12 and most of the document. This is *most* of the audit work because most standards apply at the course plan level.
- **Per-act unit audit.** Fires once per act after the act is built (sampled within the act, like SM validation). Audits Part 4 (unit build standards) — LD-PIL-13 through LD-PIL-21 — for the units in that act. Smaller scope, fewer findings.

**Why batched.** Batching audit at gates rather than per-artifact is cheaper, lower-friction, and matches the exemplar's per-act validation cadence. Most course-plan standards only fire once per course. Most unit-build standards admit batched review without losing signal.

**Isolated context.** The auditor sees only `standards.md` (the relevant Part) + the artifact under review + the design lock (where needed for cross-references like LD-PIL-04's "every CLO contributes to the PLO"). It does not see generator prompts, chain of thought, prior conversation, or other reviewer findings.

**Output.** Findings in the standard format (severity, location, observation, why-it-matters, optional fix hint) referencing specific standard IDs (e.g., "LD-PIL-04: CLO #3 'Examine the policy landscape' opens with a Bloom's verb below the Apply level the standard requires"). Lands in `reviews/standards-audit/`.

**Examples of what fires here that scripts can't catch:**
- LD-PIL-03 (PLO formula): is the PLO actually well-formed against the Project-Primary or Exam-Primary template?
- LD-PIL-04 ("every CLO must visibly contribute to the PLO"): does each CLO trace back to the PLO, or are some orphaned?
- LD-PIL-07A (Act 1 onboarding three things): does Act 1 actually accomplish all three?
- LD-PIL-09 (alternative final arc requirements): if a non-standard final arc was used, does it include the required components (scoping/proposal stage, feedback cycle, sharing/reflection)?
- LD-PIL-19 (Steps purpose): do problem set Steps orient + integrate resources + remind to show work, without restating problems?
- LD-PIL-12 (primary instructional resource): is one identified, and does the topic sequence actually map to it?

### D3 — Persona filter (Online Skeptic, LLM, isolated context, per-artifact)

For experiential failures the Online Skeptic catches — clarity, path, purpose, sequence. Different job from the auditor: reads as a real student would, not as a compliance reviewer.

The Online Skeptic IS the persona filter. The standards document already named the canonical persona (LD-GEN-01); the framework adopts it directly.

**Context isolation is load-bearing.** Runs as a separate sub-agent invocation with a fresh context window. Sees only:

- The persona card (`personas/online-skeptic.md`)
- Part 1 of `standards.md` (the four tests as evaluation criteria)
- The design lock (course opinion + three-act structure)
- The artifact under review

Does **not** see: generator prompts, chain of thought, prior conversation, prior filter findings on the same artifact, other reviewer findings.

**Cadence — per-artifact, internal pass before ID review.** The AI applies the filter, makes light adjustments, surfaces what changed. Blocker-severity findings escalate to the ID; warnings get folded in; notes are FYI.

**Examples of what fires here that the auditor wouldn't catch:**
- "I read the instructions twice and I'm still not sure what to do first" — clarity test failure.
- "I read everything they gave me — am I supposed to know how to do this now?" — path test failure.
- "Why is this assignment here? It feels disconnected from the course's opinion" — purpose / sequence test failure.

### D4 — Subject matter validator

v3's blind-context factual check, ported with the per-act sampling cadence the exemplar specifies. Sees only the artifact + relevant textbook chapter. Catches factual errors and broken question chains.

### Cross-cutting: Conductor

For meta-standards that govern the order operations run — the build sequence, the locked vs. flexible elements, the required sign-off escalations.

These are conductor-level routing logic. The conductor reads them, sequences agents accordingly, surfaces required sign-offs to the ID at the right moment.

**Examples:**
- LD-PIL-13 (Exam-Primary build sequence: topics → LOs → exam → worked examples → unit build) → conductor enforces sequence; the unit-builder agent does not run before the exam-generation agent has produced output.
- LD-PIL-07 fixed elements (onboarding in U1, MyCareer CM in U3, midterm at default U7) → conductor pre-populates these in the course plan template; ID confirms or signs off on changes.
- LD-PIL-05 / LD-PIL-12 escalation paths ("flag to Laura and Greg") → conductor surfaces these at the appropriate step.

---

## Context isolation by discriminator

| Discriminator | Sees | Does NOT see |
|---|---|---|
| Templates (G1) | The artifact slot it's filling. | Anything else. (Not an LLM context — structural constraint.) |
| Scripts (D1) | The CSV / artifact under review. | Anything else. (No LLM context.) |
| Standards auditor (D2) | Standards Part(s) + design lock + artifact. | Generator prompts, chain of thought, prior conversation, other reviewer findings. |
| Persona filter (D3) | Persona card + standards Part 1 + design lock + artifact. | Generator prompts, chain of thought, prior conversation, other reviewer findings. |
| SM validator (D4) | Artifact + relevant textbook chapter. | Design lock, persona card, generator reasoning, anything else. |
| Conductor | All artifact outputs (it routes between them). | Generator chain of thought. (Conductor doesn't generate content.) |

D2, D3, D4 each run as separate sub-agent invocations with fresh context. If they share context with the generator — even accidentally, by being implemented as role shifts in the same conversation — they go soft on prior reasoning and degrade into rubber-stamping.

This is the structural mechanism that makes the structured-creative-process commitment to "don't collapse generator and discriminator" enforceable rather than aspirational.

## Why two judgment-based discriminators (auditor + filter), not one

The auditor and the filter both use LLMs in isolated context, but they do different work, and collapsing them would lose signal:

- **The auditor reads section-by-section against the codified rules.** Finding format: "LD-PIL-04: CLO #3 starts with Examine, which is below the Apply Bloom's level the standard requires." Compliance reviewer voice.
- **The filter reads as a real student.** Finding format: "I read the unit overview twice and I still don't know what I'm being asked to do first." Online Skeptic voice.

Some standards fire on both — LD-PIL-07A (Act 1 onboarding) is structural enough for the auditor and experiential enough for the filter. That's fine. The two findings reinforce each other and give the ID richer signal.

If we collapsed them into one prompt, the LLM would either default to compliance-voice (and miss experiential failures) or default to student-voice (and miss codified-rule violations). Two prompts, two passes, isolated contexts.

---

## Why this preserves soul, quality, diversity, creativity

Four mechanisms keep the generator alive:

**The generator never receives the full standards document as constraints.** Each agent prompt receives only the subset of standards relevant to its job. The course-designer prompt gets Part 1 (principles) + Part 2 (syllabus standards) + Part 3 (course plan standards). The unit-builder prompt gets Part 1 + Part 4 (unit build standards). The persona filter gets Part 1 alone, as evaluation criteria. This honors the prompt-architect principle of single-objective phases — one prompt, one job, only the context that job needs.

**The vision is generated upstream, not constrained downstream.** The course opinion, three-act structure, and build notes are produced before standards apply. Soul comes from the upstream Phase 0; standards govern the downstream execution. The exemplar's commitment to ID-provides-vision-first is what makes this work — vision is the part the AI cannot do, and the ID has to bring it.

**The persona filter is goal-backward, not forward-checklist.** Per v4 §2.10. The filter doesn't grade against a rubric; it asks "what must be true for this student to succeed here? Does the artifact make each of those things true?" That's a different cognitive task — one that surfaces the qualitative failures (flatness, monotony, unmotivated coverage) the structured-creative-process skill names as the failure modes of pure forward review.

**Iteration is bounded.** The exemplar commits to "AI applies persona filter, makes light iterations, presents to ID." Light iterations only. If the persona filter finds blocker-severity issues, the AI surfaces them rather than burning iterations trying to fix them. This prevents the over-revision death spiral where the AI grinds toward bland compliance.

---

## Why this avoids drag

Drag has three sources, and the architecture addresses each:

**Pre-generation over-constraint.** Solved by single-objective phases. Each agent prompt receives only the standards it needs. The course-designer agent doesn't see the word count limits; the unit-builder agent doesn't see the CLO Bloom's verb list.

**Post-generation rework cycles.** Solved by bounded iteration. Light iterations, blocker escalation. Mechanical checks run cheap and fast (Python, no LLM calls). The persona filter is one pass before ID, not many.

**ID intervention friction.** Solved by severity stratification. The ID sees blockers first; warnings can be folded in or accepted as-is; notes are FYI. The AI handles its own self-correction loop before involving the ID. This honors the framework's goal 7 (ID cognitive load) — the ID spends time on judgment, not on managing AI conversations.

---

## How standards flow through the system

```
                              standards.md  (canonical)
                                    │
                                    ▼
                           compile_standards.py
                                    │
        ┌─────────────────┬─────────┴─────────┬─────────────────┐
        ▼                 ▼                   ▼                 ▼
  templates/derived/  scripts/derived/  prompts/derived/  prompts/derived/
                                        (generator)        (discriminator)

GENERATOR SIDE (seeded)                 DISCRIMINATOR SIDE (checked)
  G1 templates                            D1 scripts (mechanical)
  G2 prompt context                       D2 standards auditor (LLM)
                                          D3 persona filter (LLM)
                                          D4 SM validator (LLM)
                                          + conductor (cross-cutting)
```

When standards change, the workflow is:

1. Edit `standards.md`.
2. Run `scripts/compile_standards.py`.
3. Derived artifacts regenerate.
4. Commit the change.

No manual touching of derived files. If a derived file is hand-edited, the next compile overwrites the change. This is intentional: it forces standards updates to flow through the source-of-truth document.

---

## Templates that carry G1 + D1 (the spreadsheet pattern, with caveats)

The course plan template (`templates/source/course-plan-exam.xlsx`) is a working example of a template that defines mechanical checks via embedded Excel formulas. Its `VALIDATION CHECKS` block defines the following:

- Total = 1000 pts (LD-PIL-08, LD-GRD-01)
- Final exam 350–450 pts (LD-PIL-08, LD-PIL-21)
- Midterm 100–200 pts (LD-PIL-21)
- Act 1 ≤ 250 pts (LD-PIL-08)
- Max 5 units w/ secondary (LD-PIL-09)
- No secondary > 15 pts (LD-PIL-10, LD-GRD-02)
- Midterm and Final units exam-only (LD-PIL-09)
- ≥ 9 problem-set primaries (LD-PIL-09)
- Skill benchmarked in Unit 6 (course-template requirement)

For human ID users opening the file in Excel, Sheets, or LibreOffice, these formulas evaluate at fill time and provide structural enforcement (G1) plus immediate mechanical feedback (D1) in one artifact.

### Why the agent still needs `check_course_plan.py`

The agentic flow cannot rely on the spreadsheet's calculated values alone, for three reasons:

1. **`openpyxl` does not recalculate formulas.** When the agent opens an xlsx programmatically, `data_only=True` returns the *cached* values from the last time the file was saved by Excel/Sheets/LibreOffice. If the agent has just edited cells, those cached values are stale. Reading them gives apparent PASS for an actually-FAIL state.
2. **A new agent-generated xlsx has no cached values.** When the agent writes a fresh xlsx with formulas, the cells contain formula text but no calculated outputs until a calc-engine opens and saves the file. Agents in headless environments would need `soffice --headless --convert-to xlsx --recalc` (or equivalent) to refresh the cache — added dependency, slower roundtrip, more failure surface.
3. **Floating-point edge cases differ across calc engines.** Same formula in Excel, Sheets, and LibreOffice can produce slightly different results in pathological cases. Deterministic Python validation removes the engine variable.

### Belt-and-suspenders is the architecture

The spreadsheet's formulas remain the **source-of-truth definition** for mechanical course-plan standards. They're easy to inspect and edit in Excel, where the original standards lives. `check_course_plan.py` mirrors the same logic in Python so headless agents get deterministic validation independent of any calc engine. They should agree — `compile_standards.py` reads the spreadsheet's `VALIDATION CHECKS` block and emits a Python check function with matching logic, so updating one updates both.

```
standards.md + course-plan-exam.xlsx VALIDATION CHECKS
                          │
                          ▼
                compile_standards.py
                          │
                          ▼
              check_course_plan.py rules
              (same logic, Python-native)
```

When the spreadsheet and the script disagree on a course plan's compliance, the run halts. Disagreement signals either a bug in the compile script or a stale cache in the xlsx — either way, the agent surfaces it and waits for the ID rather than choosing whichever answer it prefers.

### Generalization

For any spreadsheet-based template with embedded formulas, the same pattern applies: the spreadsheet defines the rules in human-friendly Excel syntax; the compile script translates them to Python; the script is what runs in the discriminator path; the spreadsheet is what runs at human fill time. The LLM-based auditor (D2) covers the rest — judgment-required standards that no formula can express (PLO well-formedness, CLO roll-up to PLO, Bloom's verb selection, etc.).

---

## Per-standard mapping

Every standard from `standards.md` and the relevant overlapping ones from `rize-lds.md`, with its generator-side seeding (G) and discriminator-side check (D). Most standards have one of each; some are pure-generator (G1 alone) or pure-discriminator (D1 alone).

| Standard | Generator side | Discriminator side | Notes |
|---|---|---|---|
| LD-GEN-01 (Online Skeptic) | G2 (filter prompt + generator awareness) | D3 | Filter's core evaluation criteria |
| LD-PIL-01 (Pillars Online Skeptic) | G2 | D3 | Adds sequence test to LD-GEN-01 |
| LD-GEN-02 (Hierarchy of Needs) | G2 | D3 | Filter checks hierarchy preserved |
| LD-GEN-03 (Every Action Earned) | G2 | D3 | Filter asks "why is this here?" |
| LD-PIL-02 (Course Classification) | G2 | Conductor + D2 | Conductor routes by classification; auditor checks rule application |
| LD-PIL-03 (PLO formula) | G1 + G2 | D2 | Template scaffolds; auditor checks well-formedness |
| LD-PIL-04 (CLOs: max 8, Bloom's, PLO roll-up) | G1 + G2 | D1 + D2 | Template has 8 rows; script validates count + verb list; auditor checks PLO roll-up |
| LD-PIL-05 (Exam-assessable CLOs) | G2 | Conductor + D2 | Conductor escalates flagged CLOs; auditor verifies supplemental coverage |
| LD-PIL-06 (2x assessment frequency) | G2 | D1 | Script validates KLO/CLO assessment count |
| LD-PIL-07 (Three-act structure) | G1 + G2 | Conductor + D2 | Template fixes Act boundaries; conductor enforces fixed elements; auditor checks flexible elements |
| LD-PIL-07A (Act 1 Onboarding requirements) | G2 | D2 + D3 | Auditor checks three requirements; filter validates experientially |
| LD-PIL-08 (Point budgets) | G1 + G2 | D1 | Template totals to 1000; script validates per-act ranges + 300 pt/unit cap |
| LD-PIL-09 (Activity mix) | G2 | D1 + D2 | Script validates caps; auditor checks "every item earns its place" |
| LD-PIL-10 (Quizzes vs. Exams) | G1 + G2 | Conductor + D2 | Template differentiates types; conductor enforces proctoring; auditor checks open/closed-note documentation |
| LD-PIL-11 (Career Milestones) | G2 | Conductor | Conductor places per LD-PIL-07 |
| LD-PIL-12 (Primary instructional resource) | G2 | Conductor + D2 | Conductor blocks unit build without resource; auditor checks topic mapping |
| LD-PIL-13 (Exam-Primary build sequence) | — | Conductor | Pure routing logic |
| LD-PIL-14 (Walkthroughs / Exemplars / Templates) | G1 + G2 | D2 + D3 | Templates scaffold; auditor checks 4-5 worked examples per question type; filter validates quick-reference resource placement |
| LD-PIL-15 (Quizzes as retrieval) | G2 | D1 | Script validates progress quiz cadence + prior-unit content |
| LD-PIL-16 (Question types) | G1 | — | Question templates have allocation-constrained type enums; structural enough to need no separate check |
| LD-PIL-17 (Peer Review) | G1 + G2 | D1 + D2 | Template scaffolds; script validates staggered due dates; auditor checks structured response prompt |
| LD-PIL-18 (Word counts) | G2 | D1 + D3 | Script counts words; filter applies purpose test to prose |
| LD-PIL-19 (Problem Set Structure) | G1 + G2 | D2 | Three required output files in template; auditor checks Steps purpose |
| LD-PIL-20 (KLOs: 35–55) | G2 | D1 + D2 | Script validates count; auditor checks format ([Bloom's] + [object] + [scope]) and assessability |
| LD-PIL-21 (Standard Exam Structure) | G1 + G2 | D2 | Exam template has three sections; auditor checks PLO mapping in Section 3 |
| Appendix B (Controlled Vocabulary) | G1 | — | Activity labels are a fixed enum in templates |

### Parent Rize LDS standards (in addition to the Pillars layer above)

| Standard | Generator side | Discriminator side | Notes |
|---|---|---|---|
| LD-GEN-04 (When Standards Conflict) | G2 | Conductor | The supersession rule itself. Conductor applies it when standards collide |
| LD-ARC-01 (14 Units, "Unit" not "Week") | G1 + G2 | D1 | Course plan template fixes 14 unit rows; QA-05 enforced by script via banned-phrasing match on "week"/"weeks" |
| LD-ARC-02 (2x LO assessment + 6-level rubric) | G2 | D1 + D2 | Script counts assessments per LO; auditor verifies rubric is 6-level for assignment-based assessments |
| LD-FP-01 (Portfolio-Worthy Product) | G2 | D2 + D3 | Auditor checks success indicators; persona filter validates "I could show this to an employer" experientially |
| LD-FP-02 (Project Milestones: Proposal/Draft/Submission/Async Fair) | G1 + G2 | D2 | Course plan template scaffolds the four milestones; auditor verifies presence and pacing |
| LD-FP-03 (Project Originality) | G2 | D2 | Auditor checks "every student's submission is unique" requirement |
| LD-FP-04 (Project Complexity by Level: 100/200/300/400) | G2 | D2 | Auditor verifies complexity matches course level |
| LD-FP-06 (LO Mapping in Final Project) | G2 | D2 | Auditor verifies all PLOs and 3+ CLOs are assessed in final |
| LD-ACT-01 (Activity Mix — replaced by LD-PIL-09 for Pillars) | — | — | Superseded by LD-PIL-09 in Pillars context |
| LD-ACT-02 (Activity Purpose: 6-level vs 3-level rubric) | G1 + G2 | D1 + D2 | Templates scaffold rubric type by activity; script validates rubric level matches activity type |
| LD-ACT-03 (Three-Heading Structure: Turn In / Context / Steps) | G1 | D1 | Assignment template has three required headings; script validates presence |
| LD-ACT-04 (Turn In Quality) | G2 | D2 + D3 | Auditor checks for instruction-leakage from Steps; filter validates a student can scan it |
| LD-ACT-05 (Context Quality) | G2 | D2 + D3 | Auditor applies "remove Context — do Steps still work?" test; filter checks "why does this matter to me?" |
| LD-ACT-06 (Steps Quality) | G2 | D2 + D3 | Auditor applies "Could you follow Steps with Context+Resources hidden?" test |
| LD-ACT-07 (Exemplars) | G1 + G2 | D2 | Exemplar template + auditor checks A-level + annotation requirements |
| LD-ACT-08 (Templates linked at point of need) | G2 | D2 | Auditor verifies template links present in Steps with /copy URL ending |
| LD-ASG-01 (Assignment Guidelines) | G2 | D2 + D3 | Auditor checks success indicators; filter validates "could be shown in interview" framing |
| LD-ASG-02 (No Group Work) | G1 + G2 | D1 | Templates lack group-deliverable slots; script flags "group", "team", "partner" patterns |
| LD-ASG-03 (AI-Aware Stoplight) | G1 | — | UI-implemented per source note; no copy in assignments |
| LD-DSC-01 (Discussion Guidelines: subjective + open-ended) | G2 | D2 | Auditor applies "would this generate discussion in a room?" test |
| LD-DSC-02 (Discussion Responses optional default — Pillars LD-PIL-17 modifies for peer review) | G2 | D2 | Auditor checks: optional unless peer review |
| LD-QZ-01 (Quiz Policies: 2 attempts, untimed, allowed types) | G1 | D1 | Quiz template constrains question types; conductor sets attempt/timer metadata |
| LD-QZ-02 (Quiz Feedback: negative response text) | G1 + G2 | D2 | Quiz template has negative-response slot; auditor verifies non-empty + resource pointer |
| LD-RES-02 (Resource Purpose: required only, no optional) | G2 | D2 | Auditor flags any resource not tied to a specific Step |
| LD-RES-03 (External over Custom) | G2 | D2 | Auditor checks Tier-3 justification per "no suitable external" criteria |
| LD-RES-04 (Three Resource Tiers: Light/Guided/Remix) | G1 + G2 | D2 | Resource templates differ per tier; auditor verifies tier marking matches content |
| LD-GRD-01 (Grade Pacing — replaced by LD-PIL-08 for Pillars) | — | — | Superseded by LD-PIL-08 |
| LD-GRD-02 (Point Allocation — replaced by LD-PIL-09 + LD-PIL-10 for Pillars) | — | — | Superseded |
| LD-GRD-03 (Right-Sized Rubric: 3-level vs 6-level) | G1 | D1 | Rubric templates by activity type |
| LD-GRD-04 (No Surprises: rubric ↔ Steps) | G2 | D2 | Auditor checks every rubric criterion appears in Steps and Turn In |
| LD-GRD-05 (Rubric Criteria: max 3/4/5) | G1 + G2 | D1 | Rubric templates have bounded criterion slots |
| LD-GRD-06 (Peer Review Grading) | G2 | D2 | Auditor verifies peer review graded as effort-based, separate, low-stakes |
| QA-01 (Activity Names: descriptive, no numbering) | G1 | D1 | Activity name slot is plain string; script flags "Unit N:", "Week N:" patterns |
| QA-02 (Resource Names: descriptive, no numbering) | G1 | D1 | Same as QA-01 for resources |
| QA-03 / QA-04 (Activity / Resource References: bolded descriptive name) | G2 | D1 | Script validates bold formatting on reference patterns |
| QA-05 (Unit references: never "Week") | G2 | D1 | Script flags "week"/"weeks" in course copy |
| QA-06 (Rich Text Spacing) | G2 | D1 | Script validates single line breaks |
| QA-07 (Resource Page Requirements by Tier) | G1 + G2 | D2 | Per-tier resource templates; auditor verifies all required components |
| QA-08 (Resource Page Consistency within course) | G2 | D2 | Auditor compares resource pages within a course for structural consistency |
| QA-09 (Content Navigation: page numbers / timestamps) | G2 | D2 + D3 | Auditor and filter both check "I know where in the resource to look" |
| QA-10 (Exemplar Presentation) | G1 + G2 | D2 | Exemplar template has annotation slot; auditor verifies presence + quality |
| QA-11 (Source Citations on Tier 3) | G1 | D1 | Tier-3 resource template has Sources section; script validates non-empty |
| ACC-01 (Headings & Lists hierarchy) | G2 | D1 | Script validates heading hierarchy (H2→H3, no skips) |
| ACC-02 (Hyperlinks descriptive) | G2 | D1 | Script flags "click here", "read more" |
| ACC-03 (Alt Text on informational images) | G2 | D1 | Script validates alt text presence on `<img>` tags ≤ 150 chars |
| ACC-04 (Bold/Italic for emphasis) | G2 | D1 | Script flags ALL CAPS or color-only emphasis |
| ACC-05 (PDF Avoidance) | G2 | D1 | Script flags `.pdf` links to required resources |
| ACC-06 (Image Quality 558px+) | G2 | D1 | Script validates image dimensions |

---

## How the prompt-architect patterns apply

The agent prompts that consume standards (course-designer, unit-builder, persona filter, conductor) follow these prompt-architect principles:

**XML structure as skeleton.** Every prompt uses `<role>`, `<context>`, `<exemplars>`, `<standards>`, `<instructions>`, `<output_format>`, `<constraints>`, `<self_check>` tags. Standards live in their own `<standards>` block, distinct from instructions, so they're applied as constraints rather than as commands.

**Rationale alongside every standard.** The Pillars standards already include a **Rationale** section per item — the format the prompt-architect skill says outperforms naked instruction. When the compile script extracts standards into prompt fragments, it preserves the rationale. The agent learns *why* the rule exists, which lets it judge edge cases.

**Single-objective phases.** Each agent prompt does one job. Course-designer drafts the plan. Unit-builder builds a unit. Persona filter evaluates. Conductor routes. Standards are scoped per agent — only the relevant subset is injected.

**Analysis-before-generation.** The unit-builder prompt has an explicit `<analysis>` phase that articulates: which standards apply to this unit, which exemplars (calibration units / golden exemplars) are most relevant, what the unit needs to teach. Generation follows analysis, not vice versa.

**Positive framing.** The Pillars standards are mostly already positively framed (LD-PIL-15 says "must include retrieval practice" not "don't omit retrieval"). When standards flow into prompt fragments, this framing is preserved. Where standards are negatively framed, the compile script rewrites them positively.

**Self-evaluation integration.** The persona filter IS the self-evaluation step — it runs on the AI's draft before the ID sees it. The agent surfaces what changed, which honors the structured-creative-process commitment to "show the work."

**Exemplar design.** The framework inherits v3's golden-exemplars + calibration units pattern. 3–5 diverse exemplars (foundational SA, applied SA, foundational LA, applied LA in v3; extended for v4 to include exemplar assignments per the ideal interaction). The unit-builder prompt instructs analysis of exemplar patterns before generation.

---

## What's NOT in this document

**The Rize LDS (parent) standards** are referenced from `standards.md` but not yet captured. ACC-01 through ACC-06, LD-ACT-01 through LD-ACT-07, LD-DSC-02, LD-GRD-06, LD-QZ-01, LD-ARC-02, QA-01 through QA-12 — all referenced, none in the repo. Tracked as a gap; needs Greg to share the source.

**The compile_standards.py script** does not yet exist. Its design is implied by this architecture — read `standards.md`, produce derived templates / scripts / prompt fragments — but the implementation is downstream work.

**Per-prompt standards subsetting rules** — which standards each agent prompt receives — are sketched here but not specified concretely. Those land when the prompts get written.

---

*This document is living. When the standards architecture proves wrong in practice — a layer assignment that turns out to drag the generator, a check that misses a real failure mode, a persona filter prompt that surfaces too many false positives — update this document first, then regenerate the derivatives.*
