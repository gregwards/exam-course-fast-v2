# Deltas: v4 Spec vs. Ideal Interaction Exemplar

**Status:** Working document. Captures every place the exemplar (`ideal-interaction.md`) diverges from `system-spec-v4.md`, plus the resolutions captured in the working session of 2026-04-24.

The exemplar is the source of truth. This document exists so the v4 spec's reasoning isn't lost — many of its failure-mode arguments still apply, just resolved differently.

---

## Material shifts in shape

### The interaction model inverts

v4 §4 has the AI open every Phase 0 step by proposing 3+ candidates with rejected alternatives named. The anti-anchoring guarantee depends on the AI never showing a single recommendation.

The exemplar has the AI ask the ID for their vision in their own words first, ask one focused clarifying question, do research, then ship a *single* unified recommended plan (opinion + three acts + what to avoid + success criteria) for the ID to react to.

**Why this works.** Anti-anchoring is solved by the *ID describing their vision unprompted*, which the AI shapes a recommendation against. The ID anchors the AI, not the other way around. The exemplar gives the ID permission to defer the vision step — in courses where they don't bring vision, the AI falls back to a more candidate-driven flow. In practice, most IDs bring vision.

**What we keep from v4 here.** The clarifying question pattern (one focused question, not "what do you think?") and the diff-based revision pattern. Both transfer cleanly.

### Phase 0 collapses from eight steps into a continuous conversation

v4 specifies eight discrete Phase 0 steps (P0.1–P0.8) each with its own lock file and gate. Course opinion lock → three-act lock → persona authoring → course plan → plan review → exemplar design → exemplar review → design lock.

The exemplar is one conversation: kickoff → vision → research → unified plan recommendation → plan iteration → build notes → exemplar assignments → exemplar unit(s) → per-act build → full course. Locks become artifacts the conversation passes through, not gated steps.

**What we keep from v4 here.** The locked-artifact-as-source-of-truth idea. The course plan, build notes, and exemplar assignments are still locked artifacts that downstream work defers to. We just don't gate the conversation around them.

**What we drop from v4 here.** §6's gates table, the explicit revision caps, and the "AI asks diagnostic question" escalation paths. The exemplar handles latency through ship-then-react rather than through gate-and-revise.

### Personas collapse from 3–4 ID-authored cards to a canonical filter library

v4 has the ID author 3–4 student persona cards per course in P0.3, with parallel review across all of them at every gate.

The exemplar has "the standard 'online skeptic' student persona filter" — a single, system-level asset that the AI runs itself before showing the ID. The persona becomes a quality gate the AI applies, not a reviewer the ID consults.

**Resolution from working session.** The persona is a system asset, not a per-course one. The library may grow over time — different personas catch different things — but courses inherit personas; they don't author them. The default for a given course is one canonical filter applied by the AI.

### Critic/detector agents fold into the persona filter

v4 names three specialized reviewer agents: flatness critic, monotony detector, course opinion auditor. Each runs in isolation against the work with its own adversarial prompt.

The exemplar collapses these into the persona filter. A persona reading the work as a real student naturally surfaces flatness ("this unit feels like the last one"), monotony ("five questions in a row about pulleys"), and opinion drift ("the course said it was about X but this unit doesn't show that").

**Resolution from working session.** No specialized critic agents. The sensibilities live inside the persona filter. Different personas in the library angle the critique differently.

**What we keep from v4 here.** The *failure modes* the critics targeted — flatness, monotony, unmotivated coverage, unclear purpose — still need to be caught. They're now caught by the persona filter rather than by named agents. The persona prompt needs to reference them explicitly.

---

## Smaller deltas

### Exemplar count: reasoned with a default

v4 specifies "one exemplar per act, three total" (and §10.4 hedges this as an open question).

The exemplar specifies "two exemplars at different trajectory points" (Unit 2 and Unit 10).

**Resolution from working session.** Default is 2; the AI reasons up to 3 when course diversity warrants (e.g., a third act pedagogically distinct from the second). Reasoning is surfaced to the ID. Not dogmatic.

### Course plan format: two CSVs

v4 keeps `inputs/lo-map.csv`, `inputs/assessment-map.csv`, and `inputs/unit-descriptions.csv` as the v3 framework specifies. The exemplar mentions a "2-tab xlsx."

**Resolution from working session.** Two CSVs — `lo-map.csv` and `assessment-map.csv` — matching v3's existing schemas. Cowork's UI presents them side-by-side or tabbed; the file format remains CSV. Greg will share exemplars of these schemas; we'll inherit them.

### Validation cadence: per-act batch with sampling inside

v4 §3.3 specifies "one in three units" sampled persona review during production, plus a release-gate full batch.

The exemplar specifies per-act subject matter validation.

**Resolution from working session.** Per-act batch validation, sampled within the act. Same blind-context machinery as v3's `validate_batch.py`, different cadence.

### Research phase: new

v4 has no research phase. The exemplar adds one: between vision elicitation and plan generation, the AI does subject research with optional ID steering and produces a research artifact summarizing findings.

**What this implies.** New agent (research planner / executor), new artifact format (research summary), new context input to the plan-recommendation step. The plan recommendation should cite the research.

### Build notes: new artifact

v4 has no equivalent. The exemplar surfaces "build notes" as a separate artifact after the course plan — research-derived recommendations, case studies, unit-specific notes that didn't fit in the plan itself.

**What this implies.** New artifact between course plan and exemplar assignments. Format TBD; structure should support unit-specific anchoring (e.g., "for Unit 6, use the FiveThirtyEight 2016 polling case study").

### Self-annotated revisions

v4 has diff-based revisions in §4.3 but doesn't require the AI to narrate what changed and why.

The exemplar shows the AI presenting the iterated version to the ID *with notes on the adjustments made from the persona filter*.

**What this implies.** When the AI runs an internal pass (persona filter, SM validation), it surfaces what changed when it shows the result to the ID. Transparency baked into the interaction shape.

### Sanity is the terminus

v4 ends at "release." The exemplar ends at a Sanity upload script.

**What this implies.** The schemas this framework writes need to be Sanity-compatible. The Sanity upload script is out of scope (developed separately) but the framework's output format must map cleanly to Sanity's content model. We need a Sanity schema reference before the framework's output schemas are finalized.

---

## What v4 reasoning still applies

The exemplar replaces the *mechanics*, not the *failure modes*. These v4 arguments still hold and inform the persona prompts and templates:

- Curricula flatten when AI drives the design layer (§1).
- The ID's creative work has the most leverage at the upstream design layer, not in unit review (§1.2).
- Reviewers should work goal-backward, not artifact-forward (§2.10).
- Scope reduction is a split, not a silent simplification — agents that would simplify should flag a split, not deliver less (§2.9).
- File-based state with explicit locks survives across sessions; chat is ephemeral (§2.5).
- The conductor doesn't generate content; agents do (§2.8).

These principles inform the agent prompts and the templates the framework will eventually ship.

---

## Things still to be decided

These come out of the deltas but aren't yet resolved. Tracked in `open-questions.md`:

- The shape of the standard student persona filter prompt
- The shape of the research artifact
- The shape of the course plan recommendation document
- The shape of the build notes document
- The chain-of-thought assignment-building process
- How learning design standards are encoded into templates, prompts, and checks
- How Sanity compatibility shapes the framework's output schemas
