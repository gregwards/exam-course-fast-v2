# `research/` — Research and calibration artifacts

This folder holds the research outputs that calibrate the framework's prompts and templates. Two kinds of artifacts live here:

1. **Framework-level research** — One-time research that shapes the framework itself (course opinion catalog, three-act case studies, persona literature review, Cowork capability tests, Sanity schema reference).
2. **Per-course research artifacts** — Outputs of the in-conversation research phase the AI runs when building a specific course.

For now, framework-level research takes precedence. Per-course research will move to per-course folders once the framework supports multi-course.

## What lives here (planned, framework-level)

- `course-opinions-catalog.md` — 20–30 real college courses with strong opinions. Calibrates the course-designer prompt. (Front #1 in `docs/research-fronts.md`.)
- `three-act-structures.md` — Case studies of courses where the three-act structure was load-bearing. (Front #2.)
- `persona-review-literature.md` — Lit review on persona-based review from UX/HCI/instructional design. (Front #3.)
- `cowork-capability-tests.md` — Test results for the 10 capabilities from v4 §8.1. (Front #6.)
- `sanity-schema-reference.md` — Sanity content schema the framework outputs map to. (Front #7.)
- `innovative-scaffolding-approaches.md` — Novel approaches to scaffolded practice across subjects. (Front #8.)
- `research-planner-spec.md` — Specification of the "typical research plan" the AI defaults to. (Front #5.)

## What's NOT here

Greg's existing learning design standards survey lives in `standards/standards-survey.md`, not here, because it's specifically about the standards corpus and feeds the standards architecture decision.

## Status

All planned. None written. Sequencing in `docs/research-fronts.md`.
