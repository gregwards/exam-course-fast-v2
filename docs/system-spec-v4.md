> **⚠ Superseded in part by `ideal-interaction.md`.**
>
> This v4 spec is preserved as background context. Where it disagrees with `ideal-interaction.md`, the exemplar wins. See `spec-vs-exemplar-deltas.md` for the mapping. In particular: the eight-step Phase 0 with locked gates, the 3+ candidates anti-anchoring mechanic, ID-authored per-course persona cards, and the specialized critic agents (flatness critic, monotony detector, course opinion auditor) are all replaced by the exemplar's simpler model. Read this spec to understand the *failure modes* v4 was designed to prevent — those failure modes still apply, just addressed differently.

---

# Course Build Pipeline — System Specification v4

**Status:** Draft as of 2026-04-24. Extends `system-spec-v3.md`. Partially superseded by `ideal-interaction.md` 2026-04-25.
**Scope:** Adds a design-layer phase upstream of v3, extends calibration and production with persona review, and names the agents, gates, and artifacts that make it operable. Platform-agnostic; implementation mapping in §8.

The v3 pipeline generates exam-primary course content efficiently and reliably. It does not protect against a specific failure: **the curriculum flattens when AI drives the design layer**. The ID spends the day managing AI conversations rather than shaping ideas, and the output, while structurally sound, loses the non-obvious spine that makes a course memorable. v4 closes that gap without breaking v3.

---

## 1. Purpose

### 1.1 What this system produces

For each course, v4 produces three things v3 does not:

1. A locked **course opinion** — a short, punchy, non-obvious position the course is teaching, specific enough to propagate down into unit and assignment design.
2. A locked **three-act structure** — how the student experience progresses, what each act opens and what each act lands, expressed concretely enough for downstream agents to build against.
3. A set of locked **student persona cards** — 3-4 named student archetypes used as reviewer agents throughout the build, with their own context, voice, and flags.

Together these become the **design lock** — the creative commitments every downstream agent defers to. v3's production pipeline runs unchanged once the design lock is in place, with one addition: persona review is sampled across units and batched at release.

**What a course opinion sounds like.** Not a tagline, not a thesis statement. One or two sentences a student might actually say to someone after a great version of the course. Non-obvious. Punchy. Slightly surprising. Examples across subjects, to calibrate register:

- *Mechanics.* Almost none of the forces you feel every day are mechanical. They're electromagnetic, pretending.
- *Microeconomics.* Prices are set by the people who almost didn't buy.
- *Psychology.* Most of your decisions are already made by the time you notice you're deciding.
- *Statistics.* The variation in your data is usually more interesting than the average.
- *American Government.* The system was built by people who didn't trust each other. It still runs on that mistrust.

If the opinion reads like a course description, a learning outcome, or a thing a textbook would say in bold type, it isn't there yet. The test is whether a student would want to tell someone about it at dinner.

### 1.2 Why v3 wasn't enough

v3 treats the course plan as an input. In practice, it gets drafted by AI with light ID oversight, which is where the flattening starts. By the time an ID sees a unit, they are reviewing the third-order consequence of an upstream design decision the AI made implicitly. Reviewing units is the wrong layer to catch flatness; the flatness was committed three artifacts earlier.

v4 moves the ID's creative work upstream to where it has leverage: the course opinion, the three-act arc, the exemplar assignments that anchor each act. Below that layer, AI drives. Above it, the ID drives. The junior-ID-proposes / senior-ID-calibrates pattern lets the AI do administrative legwork in the design layer (proposing candidates, surfacing trade-offs, pushing back) without letting it own the decisions.

### 1.3 Goals, in priority order

Lexicographic. A tension between two resolves by the earlier goal winning.

1. **Design lock integrity.** Every downstream unit and assignment defers to the locked course opinion, three-act structure, and persona cards. An agent that silently contradicts the design lock is producing a defect, not a creative variation.
2. **Textbook fidelity.** Unchanged from v3. Every claim traces to a citation.
3. **Course-plan alignment.** Unchanged from v3. Every question maps to a KLO.
4. **Factual correctness.** Unchanged from v3. Blind-context validation catches errors.
5. **Workload discipline.** Unchanged from v3. Counts and points stay inside the contract.
6. **Freedom from flatness.** Every unit is reviewed against named failure modes (monotony, unclear purpose, safe-but-forgettable, unmotivated coverage). Flags are advisory but counted; unit-level flatness above a threshold halts release.
7. **ID cognitive load.** The ID spends their day on design judgment, not on managing AI conversations. If the system requires more than ~30 minutes of AI-management time per unit after design lock, the architecture is failing goal 7.

### 1.4 Goals, in human terms

- The ID feels like a senior designer directing a diligent junior, not a prompt operator supervising a pipeline.
- The AI proposes candidates; the ID reshapes them; neither of them is anchored to the first thing the other said.
- Persona reviewers are specific named students with their own voices, not abstract rubrics.
- The AI never quietly drops scope, simplifies a decision, or hedges a flag. When it would, it splits the work or escalates instead.
- Design commitments are files, not chat history. A session can die and resume without losing creative ground.

---

## 2. Design Commitments

The load-bearing commitments. Each is paired with the failure it exists to prevent.

1. **Context isolation by default.** Every agent spawned with a reviewing role gets a fresh context window. Reviewers never see the generator's chain of thought, the prior artifacts, or the ID's earlier direction. Prevents the reviewer-capture failure where an LLM goes soft on its own prior reasoning.

2. **Junior proposes, senior calibrates.** The AI's role at the design layer is to draft first passes, surface trade-offs, and push back on the ID's direction. The ID's role is to reshape what the AI produces. Prevents both extremes: the blank-page tax (ID does all the work) and the anchor tax (AI's first proposal becomes the answer).

3. **Multiple candidates, not one polished single.** Design-layer proposals come as 3+ diverse alternatives with trade-offs named explicitly, including what each rules out and why. Prevents anchoring on a single AI-generated "best" option. Applies to: course opinion, three-act structure, exemplar assignments. Does not apply to: execution tasks where one correct answer exists.

4. **Adversarial reviewers, advisory output.** Reviewer prompts open with "assume this artifact is flawed until evidence proves otherwise." Findings carry mandatory severity. The reviewer's job is to falsify, not to validate. The ID decides whether to fold in the flag. Prevents both the "all looks good" capture and the blocking-reviewer failure where advisory feedback becomes a gate.

5. **File-based state, chat is ephemeral.** Every design commitment, every persona card, every reviewer finding lives in a file. Sessions can close and resume. Git-trackable. Prevents the cognitive-surrender failure where the ID feels locked into a chat window because that's where the work lives.

6. **Bounded revision loops.** Every reviewer-revise-re-review cycle has a hard iteration cap. On cap exhaustion, the loop escalates to the ID. Prevents infinite revision cycles and the latency explosion that kills calibration flow.

7. **Named personas, not abstract rubrics.** Reviewers are specific students with backstories, course context, and characteristic failure modes. "A first-gen freshman with 4 hrs/week outside class, confident in algebra, shaky on vectors" performs better than "a novice reader." Persona cards are the reviewer's entire context; they read only the card and the artifact.

8. **Thin orchestration, specialized agents.** The conductor prompt routes and sequences. The agents do the thinking. Each agent has one job. Changes to sequencing do not touch agent prompts; changes to an agent's craft do not touch the conductor. Prevents constraint dilution and the monolithic-prompt failure.

9. **Scope reduction is a split, not a silent simplification.** Any agent that would reduce scope ("simplified treatment," "brief overview," "introductory version") instead flags a split. The ID chooses whether to accept the split or reject it. Prevents the specific flattening failure where AI quietly delivers less than the design called for.

10. **Goal-backward review.** Reviewers work from the intended student outcome back to the artifact, not forward from the artifact to a checklist. "What must be true for the student to leave this unit able to X? Does the unit make each of those things true?" catches what forward review misses.

---

## 3. Three-Phase Architecture

### 3.1 Phase 0 — Design lock-in (new, interactive)

**Purpose.** Produce the three design artifacts (course opinion, three-act structure, persona cards) plus the course plan and exemplar assignments, with all of them locked before any unit build begins.

| Step | Activity | Output |
|------|----------|--------|
| P0.1 | Course opinion elicitation | `design/course-opinion.md` |
| P0.2 | Three-act structure derivation | `design/three-acts.md` |
| P0.3 | Student persona authoring | `design/personas/*.md` (3-4 cards) |
| P0.4 | Course plan generation (extends v3 prompt, takes P0.1-3 as input) | `inputs/lo-map.csv`, `inputs/assessment-map.csv`, `inputs/unit-descriptions.csv` |
| P0.5 | Course plan persona review | `reviews/phase-0/plan-review.md` |
| P0.6 | Exemplar assignment design (one per act, three total) | `design/exemplars/act-{1,2,3}.md` |
| P0.7 | Exemplar persona review | `reviews/phase-0/exemplar-review.md` |
| P0.8 | Design lock | `design/design-lock.md` |

Each of P0.1, P0.2, P0.3, P0.6 is a structured interaction following the junior-senior pattern. The AI proposes 3+ candidates; the ID reacts; revisions are shown as diffs; the ID locks explicitly. See §4 for the session shape.

P0.5 and P0.7 run the persona reviewers in parallel with fresh context. Their findings are advisory. The ID reads the findings, decides what to fold in, and advances.

P0.8 is the design lock: a manifest file listing every artifact hash at the moment of lock. Downstream agents reading the design lock fail loudly if any referenced artifact has changed without an explicit unlock step.

**Exit criterion.** Course opinion, three-act structure, persona cards, course plan, and three exemplar assignments are all locked. Persona review on the course plan and exemplars has been run and the ID has decided what to fold in.

### 3.2 Phase A — Calibration (extended, interactive)

Unchanged from v3 structurally. Two calibration units, different archetypes, to lock voice and production standards. Extensions:

- **Unit archetype selection.** In v3, archetype choice was ad hoc. In v4, calibration units are drawn from the three-act structure — one from Act 1, one from Act 2 or Act 3. The pair covers the content and voice range the design lock commits to.
- **Persona review on each calibration unit.** Before the ID reviews the unit, the persona reviewers read the unit cold and produce flags. The ID reviews the unit *and* the flags together.
- **Scaffolding-fade seeded by three-act structure.** Scaffolding tier is derived from the act the unit belongs to, not chosen per-unit. Act 1 units ship at Tier 1; Act 2 units at Tier 2; Act 3 units at Tier 3.

**Exit criterion.** Both calibration units pass structural checks, pass blind-context factual validation, and have been reviewed by personas with no unresolved blocker-severity flags. ID has explicitly approved both.

### 3.3 Phase B — Production (extended, autonomous)

Unchanged from v3 in mechanics. Extensions:

- **Persona review, sampled.** One in three production units gets a full persona review pass. Sampling rotates persona assignments so every unit is eventually readable by every persona before the release gate.
- **Release-gate batch persona pass.** Before release, every unit is read by every persona in a single batch run. Output is aggregated into a release review document.
- **Course opinion auditor, new.** At release, a dedicated agent reads all units against the course opinion and flags units that do not visibly build toward it. The opinion auditor is adversarial: it assumes the course fails to deliver the opinion and forces the ID to see evidence that it does.
- **Factual validation, earlier.** In v3, factual validation runs at end of production. In v4, a lighter factual sanity pass runs every 3 units during production, so systemic arithmetic or citation patterns are caught before they compound across the batch.

**Exit criterion.** All units pass structural, factual, and release-gate persona review. Course opinion auditor reports no unresolved blockers. ID approves release.

---

## 4. The ID Interaction Model

### 4.1 Junior proposes, senior calibrates

The AI opens every Phase 0 step by proposing. Never with a blank page, never with a single recommendation, never with "what do you want?"

**Opening template for every Phase 0 step:**

1. "Here is what I read." *(Short confirmation of source materials the AI loaded.)*
2. "Here are three candidate {opinions / acts / personas / exemplars} for this course." *(Numbered, diverse, each 2-3 sentences.)*
3. "What each one buys you." *(One bullet per candidate, naming what the choice commits to and what it rules out.)*
4. "Where they disagree." *(A short note on the specific axes the candidates differ on — often more useful than the candidates themselves.)*
5. "Which direction wants a push back?" *(One focused question, not "what do you think?")*

The ID reacts. The AI revises. The AI shows the revision as a diff against the previous version, not a full rewrite. Revisions propose, not declare. The ID locks when ready. Lock is explicit and writes a file.

### 4.2 Anti-anchoring mechanics

Three mechanisms prevent the AI's first proposal from becoming the default:

- **3+ candidates, never 1.** A single candidate anchors. Three forces the ID to compare, which forces evaluation.
- **Rejected alternatives are named.** Each proposal carries "candidates I considered and dropped because X." Exposes the design space the AI explored and makes it cheap for the ID to revive a rejected option.
- **Push-back questions over open questions.** "Which of these three students is this course most trying to change?" is concrete and answerable. "What kind of students is this course for?" is abstract and produces low-signal output.

If the ID says "none of these, try again," the AI regenerates from a different starting point, not a variation of the same set. If the ID says "none of these, try again" twice in a row, the conductor escalates: the AI asks a diagnostic question to understand why the space is wrong, rather than generating a third losing set.

### 4.3 Session shape — course opinion step, walkthrough

**AI opens:**
> I've read the syllabus, the target audience brief, and the textbook TOC. Three candidate course opinions to react to:
>
> **A. Almost none of the forces you feel every day are mechanical. They're electromagnetic, pretending.** Commits the course to making "what's really happening at the contact" a recurring question — every time the student draws a normal force, tension, friction, they're drawing an effective force, and the course foregrounds that.
>
> **B. Nothing in mechanics is ever happening for free — every change is paid for by another change.** Commits the course to treating energy and momentum bookkeeping as the spine. Every problem ends with "where did it go."
>
> **C. You can predict almost any motion if you know what's pushing and what isn't.** Commits the course to force diagrams as the primary mode of attack, with Newton's laws as the machine that takes diagrams and gives you motion.
>
> **Where they disagree:** A is an opinion about what mechanics is really *about* (hidden physics). B is an opinion about what mechanics is *watching* (the trades). C is an opinion about what mechanics is *good for* (prediction from pushes).
>
> **Which direction wants a push-back?** My read is that A is the most memorable but the hardest to deliver — you'd need to keep returning to the electromagnetic substrate in a way most textbooks don't. C is the most standard and the easiest to execute. B is in between. Which matters more: memorability or executional ease?

**ID responds:** *"A is the most interesting but you're right it'd be hard to deliver — I don't want the whole course to be about electromagnetism. The part of A I like is that it's making the student notice something hidden. Is there a version closer to B or C that has that quality?"*

**AI responds with diff:**
> Keeping B's spine (change is paid for), adding the "hidden until you look" quality from A:
>
> **B'.** In mechanics, nothing changes for free. Every motion, every stop, every bounce — something gave something up. Most of the time you can't see what; that's what the course is teaching you to see.
>
> The change: B said "every change is paid for by another change." B' says "most of the time you can't see what paid." That's the hidden-thing quality.

**ID:** *"Yes, that's closer. But 'nothing changes for free' is better than the whole first sentence. Start with that."*

**AI proposes B'':**
> **B''.** Nothing in mechanics changes for free — every motion, every stop, every bounce, something gave something up. Most of the time you can't see what. The course is teaching you to see it.

ID edits the last sentence. Locks. File is written.

Total turns: 4-6. Total time: 15-25 minutes. The ID has done real creative work (rejected A's execution cost, saw the "hidden" thread worth preserving, pushed on sentence order) without drafting from scratch. The AI has done real work (proposed three genuine alternatives, surfaced the axis they differ on, produced a clean diff, cut redundancy on request) without owning the decision.

This is the texture every Phase 0 step should have.

---

## 5. Agent Roster

### 5.1 Named agents

| Agent | Phase | Role | Context |
|---|---|---|---|
| **Course designer** | 0 | Generates candidates for opinion, acts, personas, exemplars. Drives the junior-proposes flow. Conversational. | Full: syllabus, LOs, textbook, target audience, course opinion (once locked), prior steps |
| **Course plan generator** | 0 | Extends v3's course plan generator. Now takes design lock as input. | Full: design lock + v3 inputs |
| **Student persona** (×3-4) | 0, A, B | Reads an artifact cold as the named student. Returns flags with severity. | Isolated: persona card + artifact only |
| **Flatness critic** | 0, A, B | Adversarial reviewer. Named failure modes (monotony, safe-but-forgettable, unmotivated coverage, unclear purpose). Returns flags with severity. | Isolated: artifact + course opinion + three-act doc |
| **Factual validator** | A, B | v3's blind-context validator, unchanged. | Isolated: artifact + textbook chapters |
| **Workload checker** | A, B | v3's structural script, unchanged. | Isolated: CSV only |
| **Monotony detector** | B | Reads a sequence of 3-5 adjacent units for pacing, variety, voice drift. Returns flags. | Isolated: unit artifacts only |
| **Course opinion auditor** | B release gate | Reads all units against the course opinion. Adversarial. Assumes the course fails to deliver the opinion unless proven otherwise. | Isolated: all units + course opinion |
| **Conductor** | all | Thin orchestrator. Routes between agents, manages gates, writes lock files. Does no content generation. | Minimal: state + next-step instructions |

### 5.2 Context isolation rules

Three rules:

1. **Every reviewer is a separate invocation with a fresh context window.** No reviewer sees the generator's prior output, the ID's earlier direction, or other reviewers' findings.
2. **Reviewers read only the artifact plus their job-specific reference.** Persona reviewers read persona card + artifact. Factual validators read textbook + artifact. Flatness critics read artifact + course opinion + three-acts. Never more than that.
3. **The conductor does not generate content.** If the conductor is drafting, the architecture is wrong.

### 5.3 Parallelism

| Agent | Runs |
|---|---|
| Course designer | Sequential (inherently conversational) |
| Student personas | Parallel across personas, per artifact |
| Flatness critic | Parallel with other reviewers |
| Factual validator | Parallel with other reviewers |
| Workload checker | Parallel (it's a script) |
| Monotony detector | Sequential (needs unit sequences, runs at end of batch) |
| Course opinion auditor | Sequential (needs all units, runs at release) |

The practical implication: a unit under review in Phase B triggers 4-6 parallel agents (personas × 3, flatness, factual, workload). The ID sees aggregated findings in a single dashboard, not six serial reports. Latency is bounded by the slowest agent, not the sum.

---

## 6. Gates and Revisions

| Phase | Gate | Revision cap | On cap exhaustion |
|---|---|---|---|
| 0.1 | Course opinion lock | 2 ID-initiated revisions | Escalate: AI asks diagnostic question about the design space |
| 0.2 | Three-act lock | 2 | Same |
| 0.3 | Persona lock | 2 | Same |
| 0.5 | Course plan review fold-in | 1 revision pass | ID reviews raw findings |
| 0.7 | Exemplar review fold-in | 1 | Same |
| 0.8 | Design lock | — | — |
| A.1 | Calibration unit 1 approval | 3 revise cycles | ID adjudicates directly |
| A.2 | Calibration unit 2 approval | 3 | Same |
| B.n | Per-unit persona review fold-in (sampled) | 2 auto-revisions | Flag unit, continue batch, surface at release |
| B release | Release gate | — | Course opinion auditor blocks if unresolved |

Gates write lock files. Lock files carry timestamps and artifact hashes. Downstream agents fail loudly if a hash has changed without an explicit unlock step. This is the mechanism that keeps the design lock honest across multiple sessions and multiple IDs.

---

## 7. Artifacts and State

```
exam-course-fast-mode/
├── design/                          # NEW — Phase 0 artifacts
│   ├── course-opinion.md
│   ├── three-acts.md
│   ├── personas/
│   │   ├── persona-01-first-gen.md
│   │   ├── persona-02-transfer.md
│   │   └── persona-03-strong-bored.md
│   ├── exemplars/
│   │   ├── exemplar-act-1.md
│   │   ├── exemplar-act-2.md
│   │   └── exemplar-act-3.md
│   └── design-lock.md               # Manifest w/ timestamps + hashes
│
├── reviews/                         # NEW — reviewer outputs
│   ├── phase-0/
│   │   ├── plan-review.md
│   │   └── exemplar-review.md
│   ├── phase-a/
│   │   ├── unit-01-persona-review.md
│   │   └── unit-02-persona-review.md
│   └── phase-b/
│       ├── sampled/
│       │   └── unit-NN-persona-review.md
│       └── release/
│           ├── full-persona-batch.md
│           ├── monotony-report.md
│           └── opinion-audit.md
│
├── inputs/                          # existing, unchanged
├── data/                            # existing, unchanged
├── scripts/
│   ├── _common.py                   # existing
│   ├── check_unit.py                # existing
│   ├── format_unit.py               # existing
│   ├── validate_batch.py            # existing
│   ├── run_persona_review.py        # NEW
│   ├── run_flatness_critic.py       # NEW
│   ├── run_monotony_detector.py     # NEW
│   ├── run_opinion_auditor.py       # NEW
│   └── design_lock.py               # NEW — writes/validates manifest
└── output/                          # existing, unchanged
```

All review outputs follow a standard shape:

```markdown
# {Reviewer name} — {artifact under review}
**Reviewer context:** {what this reviewer saw}
**Date:** YYYY-MM-DD

## Summary
{2-3 sentence narrative read}

## Findings

### Finding 1 — {BLOCKER | WARNING | NOTE}
**Location:** {specific quote or section}
**Observation:** {what the reviewer noticed}
**Why it matters:** {consequence for the student / course opinion / workload}
**Fix hint:** {optional, only if the reviewer has a specific suggestion}

### Finding 2 — {...}
...
```

Findings without a severity are invalid. The reviewer prompt enforces this explicitly.

---

## 8. Implementation Mapping

The logical spec is platform-agnostic. Three mappings are plausible. The choice depends on Cowork validation (see test protocol from prior conversation).

### 8.1 Platform comparison

| Capability | Claude Code | Cowork | API + custom app |
|---|---|---|---|
| File-based state | Native | TBD | Native (FS access) |
| Fresh-context sub-agents | Task tool | TBD | API calls |
| Streaming progress | Native | Likely yes | SSE / WebSockets |
| Persistent session across restarts | Via files | TBD | Via files |
| Custom named agents | Skills | TBD | Prompt library |
| Multi-phase workflow | Scripts + skills | TBD | Orchestrator |
| ID ergonomics (non-terminal) | Poor | Strong | Custom |

Cowork cells are TBD pending the validation protocol. If tests 1, 2, and 7 (persistent state, isolated-context sub-agents, named reusable agents) all pass, Cowork is the strongest candidate for Phase 0 and Phase A. If any of those three fail, Phase 0/A goes to a custom app on the API.

### 8.2 Likely end state

- **Phase 0 and Phase A (ID-facing, conversational)** — Cowork if validation passes, otherwise a small API-driven web app.
- **Phase B production pipeline** — Claude Code, unchanged from v3. New reviewer scripts (`run_persona_review.py`, `run_flatness_critic.py`, etc.) are Python with `anthropic` SDK calls, same shape as `validate_batch.py`.
- **Design lock manifest** — Python script, runs in either environment, writes a JSON file with artifact hashes.

Phase 0 and Phase A live where the ID works. Phase B lives where the repo lives. Design artifacts from Phase 0 get committed to the repo so Phase B can read them.

### 8.3 Minimum viable implementation (for pilot)

If the goal is to test v4 on one course before committing:

1. Phase 0 runs as a single Claude project (web app), with the ID and AI following the §4 session shape by convention. Persona reviewers are manually invoked as separate conversations with persona-card system prompts. Design artifacts are saved as markdown files in a shared drive.
2. Phase A runs as v3 does today, with two additions: a `run_persona_review.py` script that fires parallel API calls with persona cards and returns a review markdown file, and a hook to run it before ID review.
3. Phase B runs as v3 does today, with sampled persona review added as a periodic Phase B script invocation.

This is enough to test the design-layer theory without a custom app or Cowork dependency.

---

## 9. Migration from v3

v4 is additive. v3 production pipelines can continue running while v4 is being piloted. Migration order:

1. **Pilot Phase 0 on one course.** Pick a course that hasn't had its course plan drafted yet. Run Phase 0 end-to-end. Produce the design lock. Carry forward to v3's Phase A/B unchanged. Evaluate: did the ID feel like a senior directing a junior? Did the resulting course have a discernible opinion?
2. **Add persona review to Phase A on the pilot course.** Minimal: one `run_persona_review.py` invocation per calibration unit before ID review. Evaluate: did the persona findings catch things the ID was going to miss?
3. **Add sampled persona review to Phase B on the pilot course.** Evaluate: did release-gate review stay bounded in time?
4. **Roll forward to all ~38 courses.** Do not roll back v3 production for courses mid-build unless they hit design-layer problems that the new persona review would catch.

Courses currently mid-build in v3 do not re-enter Phase 0. They ship under v3 semantics. v4 applies to courses not yet in Phase A.

---

## 10. Open Questions

1. **Cowork capability validation.** Tests 1-10 from the prior protocol are not yet run. Cells in §8.1 marked TBD depend on these.
2. **Persona card review.** The personas themselves are authored in Phase 0 step P0.3. Should the persona cards go through their own review pass (e.g., the course designer auditing whether the personas are plausibly different from each other), or is ID authorship sufficient?
3. **Thinking-model assignment.** Whether to use extended-thinking mode for specific agents (flatness critic, opinion auditor both plausibly benefit). Needs evaluation.
4. **Exemplar count per act.** Currently one exemplar per act (three total). Greg may want 2-3 per act for better downstream calibration. Decision deferred until pilot run informs it.
5. **Persona count.** 3-4 personas is the working number. Courses with exceptional learner heterogeneity (e.g., GenEd courses serving every major) may need 5. Deferred until pilot.
6. **Opinion auditor authority.** At release, should the opinion auditor block release, or warn? Current draft says block on unresolved blockers, which means the auditor's severity judgments are load-bearing. May need calibration against real runs.
7. **Factual sanity pass cadence.** "Every 3 units" is a guess. Too frequent wastes API budget; too infrequent lets systemic errors compound. Tune against the pilot.

---

*This specification is living. When a pilot run surfaces a design failure worth guarding against, update this file first and the scripts second. The file is the spec; the scripts are the implementation.*
