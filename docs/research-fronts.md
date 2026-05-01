# Research Fronts

**Status:** Working document. Lists the research/calibration work that needs to happen before the framework's prompts and templates can be designed well. Order is roughly priority; a few are parallelizable.

The framing: agent prompts are only as good as the substrate they're calibrated against. If we write the course-builder prompt before we have a catalog of real courses with strong opinions, the AI will produce the AI version of "what a course opinion sounds like" — exactly the flattening v4 was designed to prevent. Each front below produces a calibration artifact the prompts later defer to.

**Important calibration policy** (2026-04-25, Greg's correction): the v3 chemistry artifacts in the sibling repo are **not exemplars**. They were produced by a process that hit the "soulless course" failure mode and are useful only as **formatting reference** (this is what an answer key file looks like; this is the structure of a problem set's three components). For tone, voice, opinion, and "what good looks like at the pedagogical level," exemplars must come from Greg's curated set — not from the v3 outputs. When prompts are calibrated against exemplars, calibrate to Greg's curated work, not to the v3 builds.

---

## 1. Real-world course opinions catalog

**Why.** The exemplar's load-bearing claim is that great courses have a punchy, non-obvious opinion. The v4 spec gives five sample opinions (mechanics, microecon, psych, stats, gov). Before we write the course-designer prompt, we need 20–30 more — drawn from real courses, not invented — to ground what good looks like.

**What to produce.** A catalog with one entry per course:

- Course name, level, subject area
- The opinion (1–2 sentences a student would say at dinner)
- Source (course page, syllabus, professor interview, student review)
- How the course delivered on it (what specific assignments, lectures, or framings made the opinion present in student experience)
- What it cost (was the opinion expensive to maintain? Did later units drift from it?)

**Where to look.** MIT OCW, Stanford courses, popular Coursera/edX courses with strong reviews, professors known for distinctive voices (e.g., Hal Abelson, Jonathan Haidt, Richard Feynman's published lectures), independent online courses with cult followings (3blue1brown, Khan Academy, Andrej Karpathy's courses).

**Output.** `research/course-opinions-catalog.md`. Calibrates the course-designer prompt and the persona filter's opinion-drift sensibility.

---

## 2. Three-act structures that held

**Why.** v4 §3.2 derives scaffolding tier from the act a unit belongs to (Act 1 → Tier 1, Act 2 → Tier 2, Act 3 → Tier 3). For that to work, the acts have to be *pedagogically distinct* in a way a student would feel — not just three roughly equal chunks of the syllabus. We need real examples of courses where the three-act structure was load-bearing, not decorative.

**What to produce.** A short set of case studies (5–10):

- The course
- What changed between Act 1 and Act 2 (and Act 2 and Act 3)
- How the change manifested in the work the student did (assignment shape, scaffolding, scenario diversity)
- Whether the student could feel the act change

**Output.** `research/three-act-structures.md`. Calibrates the three-act portion of the course-designer prompt and the per-act build process.

---

## 3. Persona-based review literature

**Why.** v4 §2.7 commits to named personas with specific failure modes. The exemplar narrows this to one canonical filter. Either way, persona-based review has decades of practice in UX research, instructional design, and software usability testing. The v4 spec doesn't cite any of it. A short literature pass would tell us:

- What makes a persona card actually drive specific findings rather than generic ones
- Known failure modes of persona-based review (e.g., persona ossification, generic responses, reviewer-capture)
- How findings should be scored when reviewers conflict
- Whether one rich persona outperforms several thin ones, or vice versa

**What to produce.** A short summary doc with citations (Cooper's *About Face*, Pruitt & Adlin's persona work, Marr's research on "data personas," any HCI literature on adversarial usability review). Note what transfers to AI-driven personas vs. what's human-specific.

**Output.** `research/persona-review-literature.md`. Calibrates the persona filter prompt structure.

---

## 4. Greg's existing learning design standards

**Why.** Greg has codified standards. The framework must not violate them. Before we design how standards integrate with templates / scripts / prompts, we need to read what he has and understand the shape:

- How are the standards structured today? (Single document? Multiple? Wiki?)
- What's the scope? (Course design? Assignment design? Question-level? All three?)
- Which standards are mechanically checkable? Which require judgment?
- Which ones have already been violated by AI-drafted content in past builds, and how?

**What to produce.** A summary of the existing standards corpus, sectioned by the level it operates at (course / unit / assignment / question / prose), with each section flagged as machine-checkable or judgment-required.

**Output.** `standards/standards-survey.md`. Direct input to the standards architecture decision in `open-questions.md`.

---

## 5. The "typical research plan" the AI defaults to

**Why.** The exemplar has the AI offer "the typical research plan" with optional ID steering. We need to specify what "typical" means before the research agent prompt can be written. Working hypothesis: subject overview, comparable courses, novel approaches, common student difficulties, citation-worthy source list. Validate by trying it on 2–3 different subjects.

**What to produce.** A document specifying the default research plan, with worked examples of how it executes for three different subjects (e.g., intro stats, intro psych, intro programming). Note what the AI should produce, what tools it needs, what the artifact looks like.

**Output.** `research/research-planner-spec.md`. Calibrates the research agent prompt and artifact format.

---

## 6. Cowork capability validation

**Why.** Already in `open-questions.md`. Repeated here because it's a research/testing front, not a design decision. The 10-test protocol needs to run.

**What to produce.** Test results document with pass/fail for each of the 10 capabilities, plus implications for the platform decision.

**Output.** `research/cowork-capability-tests.md`. Decides where Phase 0/A actually run.

---

## 7. Sanity content schema reference

**Why.** Already in `open-questions.md`. The framework's output schemas need to map to Sanity's content model. Without the Sanity schema in hand, output schemas drift and will require re-mapping later.

**What to produce.** A copy of (or pointer to) the Sanity schemas the framework writes against, with notes on which fields the framework is responsible for vs. which are populated by humans or by the upload script.

**Output.** `research/sanity-schema-reference.md`. Constrains output schema design across the framework.

---

## 8. Longitudinal signal — closing the feedback loop

**Why.** v3 ID feedback (see `v3-id-feedback-signal.md`) surfaced *"I still don't know if my units are actually good. They seem OK?"* — a feedback-loop problem that no framework fully solves through pre-release review alone. The persona filter is an LLM proxy for student experience; the SM validator is a correctness check; neither tells us whether students are actually learning, completing, or finding the course memorable.

**What to produce.** A short specification for how the framework eventually captures and feeds back longitudinal signal:

- What signals can we get? (Sanity has completion data; quizzes have score distributions; problem sets have time-on-task in some LMS configurations; final exam outcomes; student survey responses.)
- What signals would tell us whether the *opinion* landed? (Distinct from whether the content was technically correct.)
- How does signal flow back into the framework? (Per-course post-mortem files, cross-course pattern detection, prompt updates derived from systematic findings.)
- Where does signal live in the repo structure? (Probably `feedback/{course-id}/` with per-cohort data.)

**Output.** `research/longitudinal-signal-spec.md`. Probably out of scope for v4's first cut but worth scoping so output schemas accommodate it. Defers to pilot for actual implementation.

---

## 9. Innovative pedagogical approaches in scaffolded practice

**Why.** The exemplar's stats course conversation specifically calls out wanting to research "innovative websites or YouTube channels with a novel approach to scaffolding practice problems." This isn't course-specific — it's a research front the AI does *for every course it builds*. We can prime the well by gathering what "innovative scaffolding" looks like across subjects so the research agent has a calibrated bar.

**What to produce.** A catalog of approaches that genuinely move the needle (e.g., Khan Academy's mastery model, 3Blue1Brown's visual intuition, Brilliant.org's interactive scaffolding, Stack Overflow-style peer practice, Codecademy's inline feedback, Duolingo's spaced repetition, Anki for retention). Per entry: the approach, why it works, what conditions it needs, what subjects it transfers to.

**Output.** `research/innovative-scaffolding-approaches.md`. Calibrates the research agent's "novel approaches" search and the build-notes format.

---

## Sequencing

Some of this is parallelizable; some isn't.

**Independent and parallelizable:** 1, 2, 3, 6, 9.

**Sequenced:** 4 (review existing standards) → standards architecture decision → relevant prompt-engineering work. 5 (research planner spec) depends on 9 (innovative scaffolding) being underway so the planner has a concrete example.

**Recommended first move:** 4, 6, 7. Greg's standards (4) unblock the standards architecture decision; Cowork tests (6) unblock the platform decision; Sanity schemas (7) unblock the output schema design. Those three are the highest-leverage early research because they unblock other work.

Then 1, 2, 3, 9 in any order. Then 5 once 9 is far enough along to have examples. 8 (longitudinal signal) can defer until a pilot is queued.
