# Open Questions

**Status:** Working document. Tracks what's resolved by the exemplar and the working session, what's still genuinely open and needs work before the framework can be built, and what's deferred to pilot.

---

## Resolved

These were either ambiguous in the v4 spec or actively under debate, and the exemplar plus the 2026-04-24 working session settled them:

**Persona model.** One canonical persona filter (the "online skeptic") applied by the AI internally. Library may grow over time; courses inherit from the library, they don't author per-course personas.

**Critic/detector agents.** None. Flatness, monotony, and opinion-audit sensibilities are baked into the persona filter prompts.

**Exemplar assignment count.** Default 2; AI reasons up to 3 when course trajectory diversity warrants. Surface reasoning to ID.

**Course plan format.** Two CSVs — `lo-map.csv` and `assessment-map.csv`. Schemas inherit from v3. Greg to share exemplars.

**Validation cadence.** Per-act batch validation with sampling inside the act. Same blind-context machinery as v3.

**Anti-anchoring mechanism.** ID provides vision in their own words first; AI shapes a single recommended plan around it. v4's 3+ candidates per step is dropped.

**Phase 0 shape.** Continuous conversation with locked artifacts, not eight gated steps with revision caps.

**Repository strategy.** New folder (`course-build-framework-v4/`), platform-agnostic. Existing physics repo is read-only.

**Pilot timing.** Framework-only for now. No specific course queued.

**Platform direction.** Cowork pending capability tests. If tests fail, fall back path is Claude Code.

---

## Open and blocking

These are not yet decided and they block design work that comes after them. Each needs a real answer, not a deferral.

### How are exam item count caps enforced?

The v3 ID feedback (see `v3-id-feedback-signal.md`) flagged that fast mode produced a 62-item / 380-point final when the build guide template specified ~24 items / 120 points. v3's workload contract treated counts as targets with post-generation deviation flags; fast mode drifted 2.5× and the flag fired after the fact.

**Working assumption.** Item count and point caps must be **pre-generation constraints**, not post-generation flags. The exam assembly prompt receives the cap; the formatter rejects exam compositions that exceed it. This is a small implementation question (where exactly does the cap get enforced — in the generation prompt, in the formatter, or both?) but the answer needs to be specific.

### How is cross-exam-bank deduplication enforced?

Same source. v3's checks enforce scenario uniqueness within and across units; they don't enforce question uniqueness across midterm 1, midterm 2, and final when those are assembled from a shared exam bank. The ID hit 5 duplicates on M1 and 6 on M2.

**Working assumption.** A discrete check that runs on exam *compositions*, not just on individual units. Either a `check_exam_assembly.py` script or an extension of the formatter that fails loudly when bank reuse is detected across exam events.

### How is textbook chapter coverage enforced?

Same source. v3 validates KLO coverage per unit but not chapter coverage. The course plan listed Ch 2.4 under Unit 2; the build skipped it.

**Working assumption.** New `check_unit.py` rule: every chapter assigned to a unit in `assessment-map.csv` must be referenced (via textbook citation) in at least one question for that unit. Mechanical check, low cost.

### ~~How are learning design standards encoded?~~ — RESOLVED 2026-04-25

**Resolution.** Standards architecture is drafted in `standards/standards-architecture.md`. Four enforcement layers: templates with embedded slots (Layer 1, prevention), scripted checks (Layer 2, mechanical detection), persona filter context (Layer 3, judgment-based detection), conductor / build-sequence logic (Layer 4). Every Pillars standard mapped to a layer.

The architectural commitment: standards live primarily on the discriminator side, not the generator side. Generator gets latitude inside structural slots; discriminator catches violations and surfaces them back. Per the structured-creative-process principle of never collapsing generator and discriminator.

`standards/standards.md` is the source of truth, imported from Greg's Pillars Learning Design Standards docx. Templates, scripts, and prompt fragments derive from it via `scripts/compile_standards.py` (TBD).

**What still needs to happen.** Implement `compile_standards.py`. Capture the parent Rize LDS standards (referenced throughout but not yet in the repo). Resolve the seven pending decisions in the standards' Appendix A.

### What is the structure of the canonical persona filter prompt?

**Partially resolved 2026-04-25.** The persona definition is now drafted at `personas/online-skeptic.md`, derived directly from LD-GEN-01 + LD-PIL-01. The four tests (clarity, path, purpose, sequence) are the persona's evaluation criteria.

What still needs to happen: write `prompts/persona-filter.md` — the prompt that takes the persona definition and an artifact and produces findings. Construction patterns are specified in `prompts/README.md` (XML structure, goal-backward review, severity-stratified findings, single-objective phase, self-evaluation integration). Also need to adapt the failure-mode language from v4 (flatness, monotony, unmotivated coverage, unclear purpose) into the persona's "what this persona notices" section if not already covered by the four tests.

### What is the structure of the research artifact?

The exemplar has the AI do research and produce an artifact. The structure isn't specified. Decisions needed:

- What sections does it have? (e.g., subject overview, innovative approaches, common pitfalls, recommended source list with annotations)
- How does it integrate the ID's research steering?
- How does it cite sources?
- How is it later referenced by the plan recommendation, build notes, and unit-level work?

### What is the chain-of-thought assignment-building process?

The exemplar references it but doesn't specify it. The agent is supposed to adhere to prior context, course plan, and learning design standards while building an assignment. The process probably has steps:

1. Read the relevant slice of the plan, build notes, and standards.
2. Identify the KLOs the assignment must cover.
3. Choose scenarios from the bank.
4. Draft the question(s) with explicit standards cited.
5. Run the persona filter.
6. Iterate based on filter findings.
7. Surface the iterated version with change notes.

But each step needs detail. This is one of the largest pieces of prompt-engineering work in the framework.

### ~~How does Sanity compatibility shape the output schemas?~~ — DEFERRED

**Resolution 2026-04-25.** Greg has a separate script that handles Sanity upload. The framework's output formats can be designed without a Sanity schema reference; the upload script handles the mapping. Removed as a blocker.

### ~~Where do the Rize LDS (parent) standards live?~~ — RESOLVED 2026-04-25

**Resolution.** Captured at `standards/rize-lds.md`, imported from Greg's "Rize Learning Design & Formatting Standards (Jan 2026)" txt source. Source preserved at `standards/source/Rize_Learning_Design_Standards_2026-01.txt`.

The framework now has both layers in source-of-truth form. `standards-architecture.md`'s per-standard mapping table extended to cover all parent standards, with pure-supersession cases (LD-ACT-01, LD-GRD-01, LD-GRD-02) marked as superseded by their Pillars equivalents.

LD-GEN-04 (the supersession rule itself) is now codified — accessibility highest priority, then foundational principles, then course-level templates, then specific standards, then Online Skeptic test as final tiebreaker. The framework's conductor enforces this priority order when standards collide.

---

## Open but deferrable

These need answers eventually but can defer until the pilot informs them.

**Research planner specifics.** The "typical research plan" the AI defaults to. Probably looks like: subject overview, comparable courses, novel approaches, common student difficulties, citation-worthy sources. Refine through pilot use.

**Build notes structure.** Format and required sections. Pilot will surface what's actually useful.

**Per-act validation sample size.** "Sample within the act" is the cadence; the actual N is calibration work.

**Subject matter validator prompt.** v3's `validate_batch.py` prompt is the starting point. May need adjustment for non-physics subjects.

**Multiple persona library entries.** The exemplar names "online skeptic" as the canonical filter. Other personas (e.g., "anxious finals-week student," "highly capable but bored," "ESL student in their second year") are valuable but not blocking. Add as the library matures.

**Cowork-specific UI affordances.** Tabbed CSV view, side-by-side artifact rendering, AI-shows-work transparency. Subject to what Cowork actually supports.

**Lecture flow integration.** v3 has lecture recommendation as part of unit output. Exemplar mentions lectures may shift scope based on research. The exact integration point needs pilot calibration.

**Sanity upload script.** Out of scope for this framework but the schema we write to needs to support it.

**Longitudinal signal capture.** The v3 ID feedback flagged "I still don't know if my units are actually good." No framework fully solves this without downstream signal from real students. Probably out of scope for v4's first cut, but worth scoping as a future capability so output schemas accommodate longitudinal feedback (completion rates, struggle metrics, time-on-task, post-test outcomes). Tracked as a research front.

**Textbook frame escape.** The v3 ID feedback flagged that the process copies the journey the textbook already chose. Phase 0's three-act structure can in principle break free of textbook chapter sequence when the course opinion warrants it. The course-designer prompt should invite the AI and ID to consider whether the textbook's frame is right for *this* course's opinion, or whether the arc reorganizes the textbook's content. Prompt-engineering note for the eventual `prompts/course-plan-recommender.md`.

---

## Acknowledged but out of scope

Concerns the framework cannot solve but worth surfacing so they're not silently absorbed:

**SME review bet.** The framework can reduce what SMEs need to catch (per-act validation, persona filter, structural checks) but cannot replace SME judgment. SME quality is itself uncontrolled. From v3 ID feedback: *"hard to tell if they've done a bad review unless it's obviously bad."* The framework's only mitigation is to make what SMEs *should* check very explicit and cheap to verify.

**Volume problem.** From v3 ID feedback: *"A reduction in course volume is the only way to maintain a high quality bar."* Per-unit time savings the framework provides should not be read as license to take on more courses per ID. Framework docs should say this explicitly.

**Compliance/judgment paradigm.** From v3 ID feedback: *"Creating a paradigm of compliance and volume over creativity and judgment on the ID team."* The exemplar's commitment to ID-provides-vision-first is the structural answer — vision is the part the AI cannot do. Framework should make this commitment durable and explicit so it doesn't get diluted under volume pressure.

---

## Cowork capability tests (blocking platform decision)

From v4 §8.1 and §10.1. These need to run before committing to Cowork as the host environment.

**Critical (must pass for Cowork to be viable for Phase 0):**

1. File-based state across sessions — can a Cowork session resume into prior file state?
2. Fresh-context sub-agents — can a reviewer be invoked without seeing the generator's chain of thought?
3. Long conversational sessions — can a 50+ turn Phase 0 conversation hold without context collapse?

**Important but not deal-breakers:**

4. Parallel agent invocation — can the persona filter and SM validation run in parallel?
5. Streaming progress on long-running tasks — does the ID see something while the agent works?
6. Custom named agents — can specific agent personalities be invoked by name?
7. Artifact rendering — can two CSVs render side-by-side or tabbed?
8. Tool access for research — can the agent search the web, read PDFs, fetch transcripts?

**Nice to have:**

9. Diff rendering for revisions — does the ID see what changed visually?
10. Export to repo — do artifacts land as files in a working directory?

A test protocol document is to-be-written; lives in `research/cowork-capability-tests.md` once written.

---

## Decisions captured for traceability

For each decision, capture: the decision, the date, the source (exemplar, working session, etc.), and the alternative considered.

| Date | Decision | Source | Alternative considered |
|------|----------|--------|------------------------|
| 2026-04-24 | New folder for v4, physics repo read-only | Working session | Evolve physics repo into v4 |
| 2026-04-24 | Framework-only for now | Working session | Pilot course inline |
| 2026-04-24 | Cowork as platform target | Working session | Claude Code |
| 2026-04-25 | Exemplar supersedes v4 spec | Working session | Treat them as compatible |
| 2026-04-25 | One canonical persona filter | Working session | 3-4 ID-authored personas (v4) |
| 2026-04-25 | No specialized critic agents | Working session | Flatness/monotony/opinion-audit agents (v4) |
| 2026-04-25 | Default 2 exemplar assignments, reasoned up to 3 | Working session | One per act, three total (v4) |
| 2026-04-25 | Course plan as two CSVs | Working session | xlsx (exemplar mention) |
| 2026-04-25 | Per-act validation, sampled within | Working session | Per-unit sampled (v4) |
| 2026-04-25 | Cross-exam-bank dedup is a hard check | v3 ID feedback | Trust the bank to dedupe itself |
| 2026-04-25 | Exam item count caps are pre-generation constraints | v3 ID feedback | Post-generation deviation flags only (v3) |
| 2026-04-25 | Textbook chapter coverage is a structural check | v3 ID feedback | KLO coverage only (v3) |
| 2026-04-25 | Standards live on discriminator side, generator gets latitude | Structured creative process + Pillars standards | Inject every standard into every generator prompt |
| 2026-04-25 | Four enforcement layers: templates, scripts, persona filter, conductor | Standards architecture work | Single layer (e.g., all-in-prompt) |
| 2026-04-25 | Online Skeptic from LD-GEN-01 = the canonical persona filter | Standards alignment with exemplar | Author a separate persona |
| 2026-04-25 | Sanity output schema is separate ID-owned script | Working session | Build Sanity upload into framework |
| 2026-04-25 | Parent Rize LDS captured at standards/rize-lds.md | Greg shared source | Defer until pilot |
| 2026-04-25 | Templates can carry both G1 + D1 via spreadsheet formulas | Course plan template inspection | Templates are slots only; checks are scripts |
| 2026-04-25 | v3 outputs are formatting reference only, not exemplars | Greg correction | Use v3 chemistry artifacts as exemplars |
| 2026-04-25 | Max 8 CLOs enforced; canonical template updated to 8 CLO rows | Greg instruction | Allow 9 (template-driven) |
| 2026-04-25 | Annotations + Scenario Tag stay in framework internal schema; stripped on export | Greg instruction | Drop columns to match xlsx export shape |
| 2026-04-25 | Belt-and-suspenders for course-plan validation — spreadsheet defines, check_course_plan.py mirrors | Spreadsheet-math reliability analysis | Trust spreadsheet cache alone |
| 2026-04-25 | Course Narrative sheet added to canonical template (6 sections from ideal conversation) | Greg go-ahead | Keep narrative as separate markdown |
| 2026-04-25 | Topic / Primary Resource / Unit Description added to canonical Assessment Map | Greg go-ahead | Split across separate input files |
| 2026-04-25 | Notes column added to canonical LO Map | Greg go-ahead | Working notes live in chat |
| 2026-04-25 | Current exemplars are standards-compliance floor, not creative ceiling | Greg framing | Calibrate prompts to match exemplars exactly |
