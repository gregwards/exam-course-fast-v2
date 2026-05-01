# v3 ID Feedback — Signal and Framework Response

**Status:** Captured 2026-04-25. Empirical input from an ID who built a chemistry course end-to-end using the v3 fast-mode pipeline. This is the most direct signal we have on where v3 worked, where it broke, and what the v4 framework needs to address that wasn't otherwise on the radar.

The ID's full feedback is preserved below as quoted excerpts. The signal extraction follows.

---

## What the ID did

Built core of Units 1–4 manually in Google Docs over ~3 days using a Claude project with deep design-approach context. Used fast mode for Units 6–9, 11–12 (~½ day) and exams (~40 min). Did not review fast-mode output closely as it ran. Then **stepped way back, reworked the course opinion, and retrofitted "Course Narrative" into the plan after the fact**. Rewrote the final (fast-mode version was 2.5× oversize), rewrote midterms, built early-unit quizzes, reviewed cross-unit consistency, and used Google Docs as a forcing function for proofreading before sending to SME.

---

## The "soulless course" finding

> *"At this point I stopped. Decided to step way back and rework the course opinion. At this point I had a technically accurate, well-aligned, soulless course. Clarity and consistency and exam alignment were nailed, but I couldn't answer 'why are we learning this? Why are we here?' I had all my raw materials and questions, so I went back into my Claude course dev project and added a course opinion and act overviews."*

This is the exact failure mode v4 §1 named ("the curriculum flattens when AI drives the design layer") and that the exemplar is structurally designed to prevent. Empirical confirmation that Phase 0 — vision elicitation, course opinion, three-act structure produced *before* unit content — is load-bearing, not theoretical.

**Framework response.** No change required. The ideal-interaction exemplar already commits to opinion-and-acts-first. This finding is empirical justification for that commitment.

---

## Specific defects fast mode produced

### Repeated questions across exams

> *"Repeated questions across exams. Fast mode pulled from the same exam bank for both midterms and the final. Had to identify and replace duplicates manually — 5 on Midterm 1, 6 on Midterm 2."*

v3's `check_unit.py` enforces scenario uniqueness within and across units. It does not enforce question uniqueness across midterm 1, midterm 2, and final exams when those are assembled from a shared exam bank.

**Framework response.** New requirement: cross-exam-bank deduplication is a hard check before any exam document ships. Captured in `scripts/README.md` and `docs/open-questions.md`. Likely implemented as either (a) `check_exam_assembly.py` that runs on midterm and final exam compositions, or (b) extension of `format_unit.py` that fails loudly when exam-bank reuse is detected across exam events.

### Final exam massively oversized

> *"Final exam was massively oversized. Fast mode generated 62 items / 380 points — roughly 2.5× the build guide template. Had to trim to 24 items / 120 raw points."*

v3's workload contract specifies targets but the deviation check only fires *after* generation as a flag. The AI was free to drift 2.5× and the check caught it as a flag rather than a constraint.

**Framework response.** Item count and point caps must be pre-generation constraints, not post-generation flags. The generation prompt receives the cap; the formatter rejects exam compositions that exceed the cap. Captured in `scripts/README.md` and `docs/open-questions.md`.

### Textbook chapter coverage gap

> *"Course plan listed Ch 2.4 under Unit 2, but the build skipped it. Minor — Ch 2.4 (Chemical Formulas) is short and directly relevant, so it was added to the reading list."*

v3 validates KLO coverage per unit. It doesn't validate that every chapter listed in `assessment-map.csv` for a unit is cited in at least one question.

**Framework response.** New `check_unit.py` rule: every chapter assigned to a unit must be referenced (via textbook citation) in at least one question for that unit. Captured in `scripts/README.md`.

### Accuracy / hallucination risk

> *"Accuracy risk remains open. I'm not a chemistry SME. The content looks right to me, but I can't fully fact-check it. This is the bet on SME review quality."*

v3's `validate_batch.py` does blind-context verification but the ID notes it's "tempting to just let Claude run without checking it, especially in a time crunch." The validation is optional in practice.

**Framework response.** Per-act subject matter validation is a gate, not a step. The exemplar's per-act build cadence makes validation a normal part of the rhythm rather than a discrete end-of-build chore that gets skipped. Already captured in `docs/ideal-interaction.md`.

---

## Concerns the framework partially addresses

### "I don't know if my units are actually good"

> *"I still don't know if my units are actually good. They seem OK?"*

The persona filter helps — it's a structured way to get "would a real student find this good" feedback before the course ships. But the persona filter is still an LLM proxy for student experience, not the real thing. No framework fully solves this without downstream signal from real students taking the course.

**Framework response.** New research front (added to `docs/research-fronts.md`): how does the framework capture and feed back longitudinal signal? Student completion rates, struggle metrics, time-on-task, post-test outcomes. Probably out of scope for v4's first cut, but worth scoping as a future capability so output schemas accommodate it.

### Fatigue across multiple courses

> *"This was doable for me for one course because I prefer rough-drafting then polishing. I wonder how my brain would keep up across 4+ courses. After a while you get fatigued and more things slip by."*

v4 reduces per-unit ID time, which helps. But the fatigue concern is partially organizational — the framework can't fix it alone.

**Framework response.** Acknowledge in framework docs that per-unit time savings should not be read as license to take on more courses per ID. The framework is designed to make a course better, not to make more courses possible.

---

## Concerns the framework cannot solve

### Volume problem

> *"This is the natural result of having to develop so much so fast. I don't think we can process our way out of it under the current workload. A reduction in course volume is the only way to maintain a high quality bar."*

This is an organizational decision. Captured here so it's not lost.

### SME review bet

> *"The SME is critical for quality control but is also an external agent outside our control. We can't really check their work, and it's hard to tell if they've done a bad review unless it's obviously bad."*

The framework can reduce what SMEs need to catch (via per-act validation, persona filter, structural checks) but cannot replace SME judgment. Worth surfacing as a known unaddressed risk in `docs/open-questions.md`.

### Compliance/judgment paradigm

> *"I'm also worried we're creating a paradigm of compliance and volume over creativity and judgment on the ID team. … I have more comfort bending those rules because I'm closer to the decision process, but not everyone on the team does."*

The exemplar's commitment to **ID-provides-vision-first** is the structural answer — vision is the part the AI cannot do, and the ID has to bring it. The framework should make this commitment explicit and durable so it doesn't get diluted under volume pressure.

**Framework response.** Surface this in the README and in the eventual `prompts/conductor.md` — the AI explicitly invites the ID to bring vision and gives them permission to defer if they don't have it yet. The vision elicitation step is not optional.

### Textbook-as-skeleton problem

> *"The process is designed for exam and textbook alignment, not for a coherent student learning journey or building metacognition. We're copying the journey the textbook already chose."*

This is the deepest critique. Phase 0's three-act structure can in principle break free of textbook chapter sequence — the course opinion drives the arc, not the chapter order. But the framework currently doesn't push for this; the AI defaults to mapping units to chapters as v3 did.

**Framework response.** The course-designer prompt should explicitly invite the AI and the ID to consider whether the textbook's frame is the right frame for *this* course's opinion, or whether the arc warrants reorganizing the textbook's content into a different sequence. Captured as a prompt-engineering note for `prompts/course-plan-recommender.md` (TBD).

---

## What the ID got right that the framework should preserve

A few process moves from the ID's account that the framework should explicitly support:

- **Manual rough-drafting at high care for early units.** The ID built Units 1–4 manually with deep iteration, then moved to fast mode for the rest. This is essentially calibration-then-production. v4's per-act build with exemplar units in Act 1 supports this directly.
- **Forcing functions for review.** The ID created Google Docs files manually as a forcing function for proofreading. Cowork's UI affordances should support this kind of "I have to look at this artifact in a different surface" pattern.
- **Stepping back to rework opinion mid-build.** The ID stopped, reworked, and rebuilt. This needs to be cheap, not a reset. The exemplar's design lock + per-act build supports this — opinion changes invalidate later acts but not earlier ones.

---

## Updates this signal triggered

- `docs/open-questions.md` — Added cross-exam dedup, exam count caps, chapter coverage checks (open and blocking), SME bet (out of scope but acknowledged), longitudinal signal (deferred), volume/compliance concerns (out of scope but acknowledged).
- `docs/research-fronts.md` — Added longitudinal-signal research front.
- `scripts/README.md` — Added the three new mechanical checks (cross-exam dedup, exam count enforcement pre-generation, chapter coverage).
- This document, capturing the raw signal so it's not lost.

---

*Empirical input from a working ID is one of the highest-signal sources for framework design. Future ID feedback should land in this folder, dated, with signal-extraction and framework-response sections.*
