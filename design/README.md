# `design/` — Per-course design artifacts

This folder holds the **design lock** for a course being built — the artifacts the conversational AI produces during the kickoff and planning phase. In a multi-course world, this folder will probably become `courses/{course-id}/design/`; for now it's a flat single-course scaffold.

## What lives here

When a course is being built, this folder fills with:

- `course-vision.md` — The ID's vision in their own words, captured from the elicitation step. May be empty or "deferred" if the ID didn't bring a vision.
- `research-summary.md` — The research artifact produced by the research agent. Subject overview, comparable courses, novel approaches, common student difficulties, source list with annotations.
- `course-plan-recommendation.md` — The unified plan the AI ships for ID review. Course opinion + three-act structure + what to avoid + what success looks like + research citations.
- `lo-map.csv` — Locked LO map (PLO, CLOs, KLOs).
- `assessment-map.csv` — Locked assessment map (per-unit KLO coverage, point totals, textbook chapter targets, primary/secondary assessment types).
- `build-notes.md` — Additional notes from research and conversation that didn't fit the plan: case studies, unit-specific guidance, research-derived recommendations.
- `exemplars/` — The exemplar assignments and exemplar units the AI builds before the per-act build begins.
  - `exemplar-assignment-unit-NN.md`
  - `exemplar-unit-NN/` (full unit if the ID requested it)
- `design-lock.md` — Manifest with the artifacts above plus their hashes and timestamps. Downstream agents fail loudly if any locked artifact has changed without an explicit unlock step.

## Design principle

Every artifact here is a file, not a chat history. A session can die and resume without losing creative ground. The conversational AI reads this folder at the start of every session to recover state.

## What does NOT live here

Per-unit production output (problem sets, walkthrough references, exam-bank questions, etc.) lives in `output/unit-NN/` — same shape as v3.

The standards document and the persona library live at the framework level (`standards/`, `personas/`), not per-course.
