# Course Build Framework v4

**Status:** Scaffolded 2026-04-25. Pre-implementation. No agent prompts, scripts, or templates exist yet.

This folder is the working repo for the v4 course-build framework — the platform-agnostic skeleton that future courses will be built from. The v3 physics build (in the sibling folder `exam-course-fast-mode/`) is the read-only reference exemplar; this folder is where v4 gets designed and built.

---

## Where to start reading

**Source of truth (read first):**

1. **`docs/ideal-interaction.md`** — The exemplar conversation between an ID and the course-builder AI. Every other document in this folder defers to it.
2. **`standards/rize-lds.md`** — Parent Rize Learning Design & Formatting Standards (applies to all Rize courses).
3. **`standards/standards.md`** — Pillars-specific layer that extends or replaces the parent. LD-GEN-04 governs supersession.
4. **`standards/standards-architecture.md`** — How standards integrate into the framework without compromising soul, quality, diversity, or creativity. Two-track model (generator-seeded + discriminator-checked); per-standard mapping covers both standards layers.
5. **`templates/source/`** — Canonical templates Greg's team uses: course plan (Exam-Primary) and exam question bank.

**Working documents:**

6. **`docs/spec-vs-exemplar-deltas.md`** — Maps the exemplar against the v4 spec, captures every divergence and resolution.
7. **`docs/open-questions.md`** — What's resolved, what's still genuinely open and blocking, what's deferred to pilot.
8. **`docs/research-fronts.md`** — The research and calibration work that needs to happen before agent prompts and templates can be designed well.
9. **`docs/v3-id-feedback-signal.md`** — Empirical input from a working ID who built a chemistry course end-to-end on v3 fast mode.
10. **`docs/system-spec-v4.md`** — Background. The v4 spec the exemplar partially supersedes.

**Operational artifacts (drafted):**

11. **`personas/online-skeptic.md`** — The canonical persona filter. Derived directly from LD-GEN-01 + LD-PIL-01. Applies four tests (clarity, path, purpose, sequence).

The subdirectory READMEs (`design/`, `reviews/`, `prompts/`, `personas/`, `scripts/`, `standards/`, `templates/`, `research/`) describe what eventually lives in each folder.

---

## Folder structure

```
course-build-framework-v4/
├── README.md                          # This file
├── docs/                              # The thinking
│   ├── ideal-interaction.md           # ★ Source of truth
│   ├── spec-vs-exemplar-deltas.md
│   ├── open-questions.md
│   ├── research-fronts.md
│   └── system-spec-v4.md              # Background, partially superseded
│
├── design/                            # Per-course Phase 0 artifacts (empty)
├── reviews/                           # Persona filter + SM validator outputs (empty)
├── prompts/                           # Agent prompt library (empty)
├── personas/                          # Canonical persona library (empty)
├── scripts/                           # Pipeline code (empty)
├── standards/                         # Greg's learning design standards (empty)
├── templates/                         # Templates the agent fills in (empty)
└── research/                          # Calibration research artifacts (empty)
```

Each subdirectory has a `README.md` that describes what eventually lives there and what shape it takes.

---

## Relationship to other repos

**The v3 physics build (`exam-course-fast-mode/`) is read-only.** It's the reference v3 implementation. Its `system-spec-v3.md`, scripts, templates, and CLAUDE.md inform v4 but are not modified.

**The v4 spec (`docs/system-spec-v4.md`) is partially superseded.** The exemplar replaces v4's anti-anchoring mechanic, eight-step Phase 0, ID-authored personas, and specialized critic agents. It preserves v4's failure-mode analysis and the file-based-state principle.

**The Sanity upload script is out of scope.** It's developed separately. The framework's output schemas need to be Sanity-compatible; the upload itself runs elsewhere.

---

## What this framework is and isn't

**Is.**

- A platform-agnostic skeleton for building courses with AI assistance, end-to-end from kickoff to release-ready content.
- An opinionated design philosophy: the AI ships work and the ID reacts; standards are non-negotiable; the persona filter catches what scripts can't.
- A working folder where research, calibration, and prompt-engineering happen before any course is actually built.

**Isn't.**

- A course. The first pilot course is a future event.
- A complete implementation. The agent prompts, templates, and scripts don't exist yet.
- Subject-specific. Physics, stats, psychology — all should run through the same framework with subject-specific inputs.

---

## What's next

The standards corpus is now complete (parent + Pillars), the canonical course plan and question bank templates are in hand, the persona is drafted, and the architecture is settled. Real prompt-writing work can start.

Three high-leverage early moves:

1. **Draft the persona filter prompt** (`prompts/persona-filter.md`) and run it against existing v3 chemistry artifacts to see if Online Skeptic findings match what the v3 ID flagged. Cheapest test of whether the central architecture catches real problems.
2. **Run the Cowork capability tests** (10 tests from v4 §8.1, captured in `docs/research-fronts.md` Front #6). Unblocks the platform decision.
3. **Resolve the two template divergences flagged in `templates/README.md`** — the CLO-count discrepancy (template has 9, standard says max 8) and the missing `Annotations` / `Scenario Tag` fields in the question bank schema.

Then, in parallel: catalog research (course opinions, three-act structures, innovative scaffolding); discriminator and generator prompts; the standards-auditor prompt; the `compile_standards.py` script.

Real exemplars (calibrated to Greg's house style) are still needed before the generator prompts can be calibrated well. The v3 chemistry artifacts are formatting reference only, not exemplars.

The standards architecture (`standards/standards-architecture.md`) is the central design document — most subsequent work derives from it.

---

## Working session log

- **2026-04-25 (operational):** First operational components built. `docs/process-flow.md` captures the end-to-end flow with entry points. Persona filter (D3): `prompts/persona-filter.md` + `scripts/run_persona_filter.py` + `templates/persona-finding.md`. Standards auditor (D2): `prompts/standards-auditor.md` + `scripts/run_standards_auditor.py` + `templates/audit-finding.md`. Course plan mechanical check (D1): `scripts/check_course_plan.py` mirrors the spreadsheet's VALIDATION CHECKS in Python. Shared module: `scripts/_common.py`. All scripts dry-run verified; `check_course_plan.py` validated against the chemistry exemplar (16 PASS / 5 real FAIL).
- **2026-04-25 (planning):** Two real exemplar course plans imported as calibration corpus. Canonical course plan template extended: Course Narrative sheet (6 sections), Topic / Primary Resource / Unit Description columns, LO Map Notes column, max 8 CLOs enforced. Belt-and-suspenders pattern for course-plan validation documented.
- **2026-04-25 (mid):** Parent Rize LDS captured at `standards/rize-lds.md` and integrated. Course plan and exam question bank templates imported. Per-standard mapping in `standards-architecture.md` extended to cover both layers. v3 outputs reframed as formatting-reference only, not exemplars.
- **2026-04-25 (mid):** Pillars Learning Design Standards imported. Standards architecture (`standards/standards-architecture.md`) drafted using patterns from the structured-creative-process and prompt-architect skills. Online Skeptic persona (`personas/online-skeptic.md`) derived from LD-GEN-01 + LD-PIL-01. Prompts and templates READMEs updated with prompt-architect construction patterns.
- **2026-04-25:** Folder scaffolded. Ideal interaction exemplar captured. v4 spec deltas documented. Subdirectory READMEs written. v3 ID feedback captured as design signal; new failure modes (cross-exam dedup, exam count caps, chapter coverage, longitudinal signal) folded into open questions, research fronts, and scripts plan.
- **2026-04-24:** Working session with Greg. Decisions captured in `docs/open-questions.md`.
