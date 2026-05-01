# `reviews/` — Reviewer outputs

This folder holds outputs from the persona filter and the subject matter validator — the two review passes the AI runs internally during a build.

## What lives here

**`persona-filter/`** — Outputs from the canonical student persona filter (D3). One file per artifact reviewed. Naming: `{artifact-id}-{persona-id}-review.md`. Contains:

- Reviewer context (which persona, what artifact)
- Summary (2–3 sentence narrative read)
- Findings, each with severity (BLOCKER / WARNING / NOTE), location, observation, why-it-matters, and an optional fix hint
- Adjustments the AI made before surfacing to the ID (if any)

Voice: real student. Findings reference what the persona experienced, not which standard was violated.

**`standards-audit/`** — Outputs from the codified-standards compliance auditor (D2). Two cadences:

- `course-plan-audit.md` — fires once, after the course plan is drafted, before the AI surfaces it to the ID. Audits Parts 1–3 of `standards/standards.md`.
- `act-N-audit.md` — fires once per act, after the act is built, sampled within the act. Audits Part 4 standards for the units in that act.

Findings reference specific standard IDs (e.g., "LD-PIL-04: CLO #3 'Examine the policy landscape' opens with a Bloom's verb below the Apply level the standard requires"). Voice: compliance reviewer.

**`subject-matter/`** — Outputs from the blind-context subject matter validator (D4). Per-act files: `act-N-validation-report.md`. Contains:

- Sample drawn (which questions from which units)
- Per-question independent answer vs. stored model answer comparison (AGREE / REASONING DIVERGENCE / SUBSTANTIVE DISAGREEMENT)
- Distractor audit for MC items
- Chain-logic audit for LA items

Voice: textbook expert checking facts.

**`release/`** — End-of-build aggregated review surfaced before the Sanity upload.

## Why files, not chat

Persistence across sessions. Auditability. Diff-able. The ID can read these later without recovering the conversation that generated them.

## Why isolated context

Both the persona filter and the SM validator run in **isolated context windows** — separate sub-agent invocations with fresh context, not role shifts within the conversational AI. The persona filter sees the persona card + standards Part 1 + design lock + artifact only. The SM validator sees the artifact + relevant textbook chapter only. Neither sees the generator's prompts, chain of thought, conversation history, or other reviewer findings.

Without isolation, an LLM goes soft on its own prior reasoning (the reviewer-capture failure from v4 §2.1). With isolation, the discriminator produces honest findings. The reviewer outputs that land in this folder are the result of those isolated invocations.

## Format conformance

All reviewer outputs follow the standard finding format documented in `prompts/persona-filter.md` (TBD). Findings without a severity are invalid; the prompt enforces this explicitly.

## What does NOT live here

The persona prompts themselves live in `prompts/` and `personas/`. The blind-context validation script lives in `scripts/`.
