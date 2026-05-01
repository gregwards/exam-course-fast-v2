# Course Build Framework

A Cowork plugin for building Rize Pillars undergraduate courses end-to-end. Drives the conversation from course vision through unit-by-unit production, with built-in review by a learning-design-standards auditor, the Online Skeptic persona filter, and blind-context subject-matter validation.

## What it does

You open Cowork. You say *"let's build a course"* (or *"continue working on CHEMY-I"*). The plugin's conductor takes over. It walks you through:

- **Course vision and research** — captures your perspective in your own words, runs subject research, returns a research summary you can react to.
- **Unified plan recommendation** — drafts a Course Opinion plus three-act narratives, surfaces what to avoid, names what success looks like. You react; it iterates; you lock.
- **Course plan build** — populates the canonical Pillars course plan template (`Course Narrative` + `LO Map` + `Assessment Map`), runs mechanical and standards-based checks as it goes.
- **Build notes** — captures the case studies, scenarios, and unit-specific guidance the plan didn't have room for.
- **Exemplar assignments and units** — proposes assignments at different points in the course's trajectory, builds them out against the standards, runs persona-filter and standards-auditor passes, surfaces findings with revision notes.
- **Per-act build** — once exemplars are locked, you ask for "all of Act 1" and it builds the units in sequence. Each act gets a sampled subject-matter validation pass and a standards-audit batch at the end.
- **Release-gate review** — a final audit reads all units against the locked Course Opinion to flag drift before upload.

The work survives discriminator review before you ever see it. You spend your time on judgment — the architecture, the voice, the case studies — not on chasing typos or reformatting CLOs.

## What's in this repo

This is the plugin itself. When installed via Cowork's plugin system, all of the following ship to the user's session:

- **`skills/build-course/`** — the conductor skill that drives the conversation.
- **`prompts/`** — the persona filter, standards auditor, and (forthcoming) subject-matter validator prompts that the conductor invokes via Cowork's Task tool in isolated context.
- **`personas/`** — the canonical student persona (Online Skeptic, derived directly from `LD-GEN-01`/`LD-PIL-01`).
- **`standards/`** — the canonical learning design standards (parent Rize LDS + Pillars layer) and the architecture doc explaining how each standard is enforced.
- **`templates/`** — the course plan template (`Course Narrative` + `LO Map` + `Assessment Map`), the question bank template, and the review output formats.
- **`scripts/`** — `check_course_plan.py` runs mechanical validation as a deterministic mirror of the spreadsheet's formulas.
- **`research/`** — calibration corpus (real exemplar course plans, plus research artifacts the framework reads for tone and approach).
- **`docs/`** — process flow, open questions, design rationale, and the dev log for anyone working on the framework itself.

Per-course content (your actual course folders) lives outside the plugin — by default at `~/Documents/Rize-Courses/`. The plugin stays read-only canonical infrastructure; your work-in-progress lives elsewhere.

## Getting started — first time

The framework will be distributed via Cowork's organization plugin marketplace. Once it's installed for you (auto-install for the ID team), open Cowork and say something like:

> "Let's build a course."

The conductor will ask where you want to keep your course folders (defaulting to `~/Documents/Rize-Courses/`), then ask which course you're starting on, and walk you through from there.

If you're picking up an in-progress course:

> "Continue CHEMY-I."

The conductor reads `~/Documents/Rize-Courses/CHEMY-I/design/design-lock.md` to figure out where you left off and resumes there.

## Getting started — local dev install (for framework contributors)

If you're contributing to the framework itself, install the plugin from a local filesystem path so edits take effect immediately:

1. Clone (or work directly in) `course-build-framework-v4/`.
2. In Cowork: Customize → Install plugin from local path → point at the cloned folder.
3. Edit any skill or prompt file. **Close the current Cowork chat and open a new one** to pick up the change (Cowork has no `/reload` command — new chats read skills from disk on activation). Test.
4. When ready to ship a change to the team, commit and push to the private GitHub repo backing the org marketplace; Cowork picks up the new version on next sync.

## How it works

For the architectural model — generator-side standards seeding, discriminator-side standards checking, isolated-context sub-agents, four-layer enforcement — read [`standards/standards-architecture.md`](standards/standards-architecture.md). For the end-to-end build flow with entry points and discriminator firing cadence, read [`docs/process-flow.md`](docs/process-flow.md). For the conversation shape this plugin tries to reproduce, read [`docs/ideal-interaction.md`](docs/ideal-interaction.md).

## Questions, gaps, decisions

[`docs/open-questions.md`](docs/open-questions.md) tracks resolved and open questions. [`docs/dev-log.md`](docs/dev-log.md) captures the working-session log of how the framework got here.

If you hit something the framework doesn't handle, surface it to Greg. The framework is designed to evolve through real ID use — your friction is the input.

---

*Version 0.1.0 — early local-dev iteration. Not yet distributed via the org marketplace; pending end-to-end validation on the first real course build.*
