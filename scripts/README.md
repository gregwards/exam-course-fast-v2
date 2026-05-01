# `scripts/` — Pipeline code

This folder will hold the Python scripts that run mechanical parts of the build — the things that don't need an LLM and shouldn't use one. Direct port and extension of v3's script suite.

## What lives here (planned)

**Inherited from v3 (subject-agnostic, port unchanged):**

- `_common.py` — Shared module. CSV schemas, required fields, allocation enums, paths, CSV I/O, LO/assessment map loaders, workload contract parser, textbook lookups.
- `init_master_csvs.py` — One-time setup. Creates empty master CSVs.
- `check_unit.py` — Structural gate. Counts, coverage, scenario uniqueness, Q-ID integrity, field population. Subject-agnostic.
- `format_unit.py` — Deterministic formatting transforms. Reads master CSVs, produces the nine per-unit output files.
- `validate_batch.py` — Blind-context content verification via API. Per-act sampling cadence (extension from v3's end-of-production).
- `workload_dashboard.py` — Cross-unit workload report.

**New for v4:**

- `design_lock.py` — Manages the design lock manifest. Writes hashes, timestamps, validates that locked artifacts haven't changed without an explicit unlock step. Replaces v4's separate lock-file mechanism.
- `run_persona_filter.py` — Invokes the persona filter (D3) against a specified artifact. Reads the persona file, the prompt template, and the artifact; produces a review markdown file in `reviews/persona-filter/`. Same shape as v3's `validate_batch.py`.
- `run_standards_auditor.py` — Invokes the standards auditor (D2) against a specified artifact. Reads the relevant Part(s) of `standards.md`, the design lock, and the artifact; produces a review markdown file in `reviews/standards-audit/`. Two cadences: course-plan-level audit (fires once after course plan) and per-act unit audit (fires after each act is built, sampled).
- `run_sm_validator.py` — Per-act blind-context factual validation (D4). Replaces v3's batch-validate at end of production.
- `check_course_plan.py` — Mechanical validation (D1) for course-plan-level standards: total points = 1000, max 8 CLOs, 35–55 KLOs, Bloom's verb whitelist, etc. Runs after course plan is drafted.
- `compile_standards.py` — Reads `standards/standards.md` source-of-truth document; regenerates derived prompt fragments, template scaffolding, and check rules so nothing drifts from the standards. Run after standards updates.

**New for v4, motivated by v3 ID feedback** (see `docs/v3-id-feedback-signal.md`):

- `check_exam_assembly.py` — Runs on midterm and final exam compositions. Hard-fails if a question appears in more than one exam event, even when those exams pull from a shared bank. v3 ID feedback: 5 duplicates on M1, 6 on M2 — manual de-dup. This makes the check mechanical.
- Extension to `check_unit.py` — Adds a textbook chapter coverage check: every chapter assigned to a unit in `assessment-map.csv` must be referenced via citation in at least one question for that unit. v3 ID feedback: course plan listed Ch 2.4 under Unit 2; build skipped it.
- Pre-generation enforcement of exam item count caps — Either a generation-prompt-side constraint or a `check_exam_assembly.py` pre-pass. v3 ID feedback: fast mode produced a 62-item / 380-point final when the template was ~24 items / 120 points. Caps must be hard, not advisory.

**Possibly out of scope (Greg's team builds separately):**

- Sanity upload script. The framework writes Sanity-compatible output; the upload runs elsewhere.

## Design principles inherited from v3

- **CSV is the source of truth.** Master CSVs hold every generated question; every output file is a deterministic transform.
- **Generation is separate from formatting.** LLMs generate; Python formats. Different times, different scripts, different error modes.
- **Stdlib-only where possible.** v3's scripts are stdlib except `validate_batch.py` (anthropic SDK). v4 scripts continue this pattern.
- **Single canonical location for every concern.** CSV schemas in `_common.py`. Standards in `standards/`. Prompts in `prompts/`.

## Subject-agnostic vs. subject-specific

The scripts are subject-agnostic by design. Subject-specific content (textbook lookups, scenario bank, unit guidance) lives in `inputs/` and `data/`. To adapt the framework to a new course, the scripts don't change; the inputs do.

## Migration from v3

The physics repo's existing scripts are mostly subject-agnostic. The path forward: copy them over, update the v3 references to v4 conventions (per-act validation cadence, design lock awareness), and add the new v4 scripts. This is a discrete task tracked in the open-questions document.
