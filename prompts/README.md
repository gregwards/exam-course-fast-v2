# `prompts/` — Agent prompt library

This folder holds the prompts that drive the conversational course-builder AI and its internal review passes. Each prompt is a file; each file has a single job.

The conversational AI is one agent from the ID's perspective. Internally it routes between specialized prompts depending on what step it's in. The prompts here are the routing destinations.

## Construction principles

Every prompt in this folder is built against the prompt-architect patterns. These are not aspirational — they're what we've committed to.

**XML structure as skeleton.** Every prompt uses semantic XML tags to separate content types: `<role>`, `<context>`, `<exemplars>`, `<standards>`, `<instructions>`, `<output_format>`, `<constraints>`, `<self_check>`. This prevents instruction bleed, exemplar confusion, and phase collapse. Standards live in their own `<standards>` block, distinct from instructions, so the model treats them as constraints rather than as commands.

**Single-objective phases.** Each prompt does exactly one job. The course-designer prompt drafts the plan recommendation; it does not also build assignments. The unit-builder prompt builds a unit; it does not also evaluate it. The persona filter evaluates; it does not also generate. Phase collapse is the most common structural failure in complex prompts.

**Standards subsetting.** Each prompt receives only the subset of standards relevant to its job. The course-designer prompt gets Parts 1–3 of `standards/standards.md` (principles, syllabus, course plan). The unit-builder prompt gets Parts 1 + 4 (principles, unit build). The persona filter gets Part 1 alone, applied as evaluation criteria. This honors the architectural commitment that standards live on the discriminator side, not as comprehensive constraints on every generator.

**Rationale alongside every directive.** The Pillars standards already pair every standard with a **Rationale** section — the format the prompt-architect skill says outperforms naked instruction. When standards flow into prompt fragments via `compile_standards.py`, the rationale comes along. The agent learns *why* the rule exists, which lets it judge edge cases instead of following the letter while missing the spirit.

**Analysis-before-generation.** Every prompt that generates content has an explicit `<analysis>` phase before the generation phase. The unit-builder articulates which standards apply, which exemplars are most relevant, what this unit needs to teach, before drafting anything. Per the prompt-architect skill: this builds an internal model of the target rather than producing output from a shallow pattern match.

**Exemplars: 3–5, diverse, annotated.** The framework inherits v3's golden-exemplars + calibration-units pattern. Exemplars are wrapped in `<exemplar>` tags inside `<exemplars>`, with annotations naming what each one demonstrates. Diversity matters more than similarity — vary structure, length, complexity, edge cases. The unit-builder prompt instructs analysis of exemplar patterns before generation.

**Positive framing.** State what to do, not what to avoid. The Pillars standards are mostly already positively framed; where they aren't, `compile_standards.py` rewrites them positively before injecting into prompts. Negative constraints appear only as secondary reinforcement after positive instruction.

**Self-evaluation integration.** Every generative prompt ends with a `<self_check>` phase. Then the persona filter runs as the second-layer self-evaluation before the ID sees the work. This honors the structured-creative-process commitment to separating generator and discriminator — the agent's self-check catches obvious drift; the persona filter catches what shallower self-check misses.

**Context isolation for discriminators.** Every prompt that does discriminator work (persona filter, SM validator) runs in an isolated context window — a separate sub-agent invocation, not a role shift within the conversational AI's context. The persona filter prompt receives only the persona card + standards Part 1 + design lock + artifact under review. The SM validator receives only the artifact + relevant textbook chapter. Neither sees the generator's prompts, the generator's chain of thought, prior conversation history, or prior reviewer findings on the same artifact. Without isolation, the discriminator goes soft on its own prior reasoning (the "reviewer-capture" failure from v4 §2.1) and the structured-creative-process commitment to separating generator and discriminator becomes aspirational rather than enforced.

**Show the work.** The agent surfaces what changed after applying the persona filter, with notes on what the filter caught and how the agent revised. This honors the structured-creative-process commitment to visible work at every phase.

## What lives here (planned)

**`conductor.md`** — The thin orchestrator. Routes between prompts and manages state. Reads the design lock and the conversation history to decide which step is next. Implements LD-PIL-13 build sequencing for Exam-Primary courses (topics → LOs → exam → worked examples → unit build). Surfaces required sign-offs to the ID at the right moment (LD-PIL-05 flags, LD-PIL-12 missing primary resource, LD-PIL-09 alternative final arc). **Does not generate content.**

**`kickoff.md`** — The kickoff turn. Confirms received materials, asks if anything else, frames the path: vision → research → plan.

**`vision-elicitation.md`** — Asks the ID for their vision in their own words. Asks one focused clarifying question if the response leaves an ambiguity that would mis-shape the plan. Allows the ID to defer.

**`research-planner.md`** — Frames the research the AI will do, optionally takes ID steering, executes the research, produces a research artifact.

**`course-plan-recommender.md`** — Drafts the unified plan recommendation: course opinion + three-act structure + what to avoid + what success looks like + research citations. Single recommendation, not three candidates. Surfaces reasoning. Receives Parts 1–3 of standards as constraints (LD-PIL-02 classification rules, LD-PIL-03 PLO formula, LD-PIL-04 CLO rules, LD-PIL-07 three-act structure, LD-PIL-08 point budgets, LD-PIL-09 activity mix, LD-PIL-12 primary resource).

**`course-plan-csv-builder.md`** — Converts the locked plan recommendation into the two CSVs (`lo-map.csv`, `assessment-map.csv`).

**`build-notes-writer.md`** — Surfaces the research and conversation insights that didn't fit the plan: case studies, unit-specific guidance, recommended scenarios.

**`exemplar-assignment-builder.md`** — Builds an exemplar assignment for a specified unit. Adheres to plan, build notes, standards. Receives Parts 1 + 4 of standards (LD-PIL-13 build sequence, LD-PIL-14 walkthroughs/exemplars/templates, LD-PIL-16 question types, LD-PIL-18 word counts, LD-PIL-19 problem set structure, LD-PIL-20 KLOs, LD-PIL-21 exam structure). Includes the chain-of-thought process: read standards subset, read calibration exemplars, articulate analysis, generate, self-check, apply persona filter, annotate revisions.

**`exemplar-unit-builder.md`** — Extends from one exemplar assignment to the full unit (problem set, walkthrough, exam bank, etc.).

**`per-unit-builder.md`** — Builds a non-exemplar unit. Reads exemplar units as in-context calibration.

**`persona-filter.md`** — The internal reviewer prompt the AI runs before showing work to the ID. Receives `personas/online-skeptic.md` (or override) plus the artifact under review. Receives Part 1 of standards (LD-GEN-01, LD-PIL-01, LD-GEN-02, LD-GEN-03) as evaluation criteria. Goal-backward review: "what must be true for this student to succeed here? Does the artifact make each of those things true?" — not forward checklist. Reads as a real student.

**`standards-auditor.md`** — The codified-standards compliance check. Distinct from the persona filter. Reads the artifact section-by-section against the relevant Part(s) of `standards.md`, in the role of a compliance reviewer. Receives standards Part(s) + design lock + artifact only. Fires once at course plan time (audits Parts 1–3, catches LD-PIL-02 through LD-PIL-12) and once per act after build (audits Part 4 for that act's units, catches LD-PIL-13 through LD-PIL-21). Findings reference specific standard IDs. See `standards/standards-architecture.md` D2 for the full role.

**`sm-validator.md`** — The blind-context subject matter validation prompt. Parameterized per question type. Inherits from v3's `validate_batch.py` prompt; updated for per-act sampling cadence.

## Why XML, exemplars, and standards in separate blocks

A real prompt example, sketched:

```
<role>
You are a senior instructional designer specializing in competency-based online education for Rize Pillars courses...
</role>

<context>
You are building Unit 7 of a Pillars Exam-Primary course. The course opinion is "X". The three-act structure is Y. Unit 7 is the midterm-checkpoint unit in Act 2...
</context>

<standards>
[Standards subset for this prompt — extracted from standards/standards.md by compile_standards.py.
Each standard has its rule + its rationale. Standards are framed positively.]
</standards>

<exemplars>
<exemplar>
<input>...</input>
<ideal_output>...</ideal_output>
<annotation>This exemplar demonstrates the Tier 2 scaffolding fade — Method label kept on Applied problems only, no inline formula reference, physical reasoning stated only for unfamiliar setups.</annotation>
</exemplar>
[3-5 exemplars]
</exemplars>

<instructions>
<phase_1_analysis>
Before generating, articulate:
- Which standards from <standards> apply most directly to Unit 7?
- Which exemplars are closest in shape to what Unit 7 needs?
- What sentence patterns appear across the exemplars? What is the typical opening move?
- Where does Unit 7's content sit in the three-act arc, and how does that affect the scaffolding tier?
</phase_1_analysis>

<phase_2_generation>
[Specific generation instructions]
</phase_2_generation>

<phase_3_self_check>
Review your output against the standards in <standards>. Identify any gaps or deviations. Revise before presenting.
</phase_3_self_check>
</instructions>

<output_format>
[Specific output format spec]
</output_format>
```

This prevents the standards from being read as commands ("you must do X") and lets the model treat them as constraints to be respected ("the output should satisfy X"). It also separates exemplars from instructions so they're treated as patterns to learn from, not steps to copy.

## What does NOT live here

Persona definitions (background, voice, what they notice) live in `personas/`. The persona filter prompt references a persona by name and pulls the definition into context.

The standards document lives in `standards/standards.md`. Prompts reference standards subsets via `compile_standards.py`-generated fragments in `standards/derived/prompt-fragments/`.
