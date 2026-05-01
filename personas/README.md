# `personas/` — Canonical student persona library

This folder holds the persona definitions the AI applies as internal review filters. Each persona is one file. Personas are framework-level, not per-course — courses inherit from this library.

## Default persona

**`online-skeptic.md`** — The canonical default. Derived directly from `standards/standards.md` LD-GEN-01 + LD-PIL-01. The standards already named the persona we should be reading work through; this file operationalizes that.

The Online Skeptic applies four tests to every artifact:

- **Clarity test** — Would a frustrated student know exactly what to do first?
- **Path test** — Can a student complete this successfully using only provided resources and instructions?
- **Purpose test** — Can you state why this matters in one sentence?
- **Sequence test** — Does this task prepare a student for what comes next in the course, and the next course in the program?

These are not abstract criteria. They're the lenses through which the persona reads every artifact, and findings name which test failed and where.

## Why a library, not a single persona

Different student archetypes catch different things. The Online Skeptic surfaces clarity, path, purpose, and sequence failures. Future personas might catch:

- Linguistic opacity (an ESL student in their second year)
- Anxiety patterns (an anxious finals-week student)
- Disciplinary depth gaps (a strong student who's bored)
- Accessibility issues (a student using assistive technology)

For most artifacts and most courses, the Online Skeptic suffices. Multi-persona review is the exception, not the rule. This is a key delta from v4 spec, which had 3–4 parallel personas as the default.

## Persona file structure

Each persona file contains enough about the student that the persona filter prompt produces specific findings, not generic ones. Sections (see `online-skeptic.md` as the canonical example):

- **One-line identity.**
- **Background.** Years of schooling, prior subject exposure, language proficiency, life context.
- **Goals.** Why they're taking this course. What they want at the end.
- **Constraints.** Time available per week, study tools, attention patterns.
- **What this persona notices.** The kinds of things this persona catches that others wouldn't.
- **What frustrates this persona.** Concrete behaviors that would cause disengagement.
- **The tests this persona applies.** From standards or domain knowledge.
- **What this persona is NOT.** Bound the persona — prevent stereotype drift.
- **Voice.** Two or three example sentences in this persona's voice, so findings sound like the persona, not like an AI rubric.

## Why specific, not generic

A persona that says "a busy student" will produce findings like "this could be clearer." A persona that says "a first-year community college student taking organic chemistry online because their home institution doesn't offer it that semester, who has 6 hours per week for this course and has been frustrated by unclear instructions in three prior online courses" will produce findings like "the second sentence of the unit 4 context section says 'briefly' but doesn't define how brief — they'll either over-write and lose points on conciseness, or under-write and miss the depth criterion."

Specificity in the persona produces specificity in the findings. Vague personas produce vague reviews.

## What does NOT live here

The persona filter prompt — the prompt that takes a persona definition and an artifact and produces findings — lives in `prompts/persona-filter.md` (TBD). The persona file is the *content* it operates on; the prompt is the *machinery*.

## Calibrating the library

Real personas, not invented ones. Pull from:

- The standards document (LD-GEN-01 + LD-PIL-01 produced the Online Skeptic directly)
- Greg's experience with actual students of these courses
- Student reviews of comparable courses
- Demographic data on the target audience
- The v3 ID feedback (the "soulless course" finding implies a persona who would have caught that — worth thinking about)

If a persona reads like a marketing brief or a stock character, rewrite it.
