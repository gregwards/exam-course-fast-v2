# Pillars Learning Design Standards (Pillars-specific layer)

**Status:** Canonical. Imported 2026-04-25 from `source/Pillars_Learning_Design_Standards.docx` (Last updated April 2026).
**Scope:** Pillars courses (undergraduate 100–400 level). Pathways and other course types may follow different standards.
**Parent document:** `rize-lds.md` (the general Rize Learning Design & Formatting Standards). This Pillars document **adds to or replaces** individual standards in the parent. Together they form the full standards corpus the framework enforces.

This document is the canonical Pillars-specific source of truth. **Templates, scripts, prompt fragments, and persona filter context are derived from `standards.md` + `rize-lds.md` via `scripts/compile_standards.py`. Do not edit derived artifacts; edit the source documents and regenerate.**

## Conflict resolution (LD-GEN-04)

Per the parent standard `LD-GEN-04`, apply standards in this priority order:

1. **Accessibility requirements (`ACC-*`)** — Non-negotiable for legal/ethical compliance.
2. **Foundational Principles (`LD-GEN-*`)** — The "why" overrides the "what."
3. **Course-level templates** — Program-specific requirements override general standards.
4. **Specific standards** — The most specific applicable rule wins.
5. **Online Skeptic test** — When truly ambiguous, optimize for LD-GEN-01.

**For the framework, this means:** Pillars-specific standards in this document supersede the parent rules in `rize-lds.md` where they conflict, except for accessibility (parent `ACC-*` standards are non-negotiable). Where a Pillars standard explicitly says "replaces LD-XXX-NN", treat the parent as overridden. Where it adds requirements without saying "replaces", treat both as in force, with the Pillars version more specific.

---

## Standard naming convention

- `LD-GEN-NN` — General. Applies to all Rize courses.
- `LD-PIL-NN` — Pillars-specific. Adds to or replaces the equivalent general standard.
- `LD-ACT-NN`, `LD-DSC-NN`, `LD-GRD-NN`, `LD-QZ-NN`, `LD-ARC-NN`, `ACC-NN`, `QA-NN` — domain-scoped (activity, discussion, grading, quiz, architecture, accessibility, QA).

References to the broader Rize Learning Design Standards use `[See Rize LDS: LD-XXX-NN]`. Those standards are not yet in this repo — see `standards/standards-architecture.md` for the gap.

---

## Part 1 — Design Principles

These principles inform all decisions. When standards conflict or situations aren't covered, return here.

### LD-GEN-01: The Online Skeptic

**Standard.** Design for the "Online Skeptic" — the student who wants to succeed but is time-poor, hates ambiguity, and needs to be convinced that their effort is worthwhile. Every activity, resource, and assessment must pass these tests:

- **Clarity test.** Would a frustrated student know exactly what to do first?
- **Path test.** Can a student complete this successfully using only provided resources and instructions?
- **Purpose test.** Can you state why this matters in one sentence?

**Rationale.** By focusing on this persona, we create courses that are clear, purposeful, and respectful of every student's time.

> **Framework note.** The Online Skeptic *is* the canonical persona filter. See `personas/online-skeptic.md`. The three tests above are the persona filter's evaluation criteria.

### LD-PIL-01: The Pillars Student as the Online Skeptic [PIL]

**Standard.** Pillars students are super-online skeptics. They may not have chosen an online course, may be more skeptical of the format, and are quicker to disengage when things feel unclear or arbitrary. Many students need this course to continue the sequence at their home institution. In addition to LD-GEN-01:

- **Sequence test.** Does this task prepare a student for what comes next in the course, and the next course in the program?

### LD-GEN-02: The Hierarchy of Needs

**Standard.** Design priorities follow a strict hierarchy: **Clarity > Relevance > Fun.**

- **Clarity** refers to both clear directions (LD-GEN-01) and clear written language in instructions and resources.
- **Relevance** clearly connects design to learning outcomes and course objectives.
- **Fun** is self-explanatory: make assignments as engaging as possible, while not sacrificing clarity or relevance.

**Rationale.** An engaging assignment is worthless if students don't understand what to do. Ensure the foundation is solid before adding creative flourishes.

### LD-GEN-03: Every Action is Earned

**Standard.** Every task, resource, and activity must have a clear purpose that is communicated to the student.

**Rationale.** The Online Skeptic will disengage if they feel their time is being wasted. Justify every action by connecting it to a valuable skill or a clear step toward the final.

---

## Part 2 — Syllabus Standards

The standards in this section govern the work done before the course plan is built: classifying the course and writing learning outcomes. Complete these before opening the course plan template.

### LD-PIL-02: Pillars Course Classification [PIL]

**Standard.** All Pillars courses are classified as either **Exam-Primary** or **Project-Primary** before course plan work begins. Classification determines which template, build sequence, and assessment standards apply.

| Course Type | Primary Summative Assessment |
|---|---|
| Project-Primary | A project, paper, presentation, or portfolio deliverable |
| Exam-Primary | A timed exam (midterm and/or final) |

To classify a course, apply these rules in order:

1. **Rule 1.** What do the majority of universities running this course use as the primary summative assessment? If the answer is clear, that is the classification.
2. **Rule 2 (tiebreaker).** Would a student walk away with something to put in a portfolio, or proof that they passed? Portfolio piece → Project-Primary. Proof of passing → Exam-Primary.

When rules conflict: For Pillars, where seamless integration with partner institutions is the top priority, default to Rule 1 and adjust learning outcomes to match.

**Rationale.** Classification drives every downstream decision. Rule 1 prioritizes partner institution alignment; Rule 2 is the pedagogical tiebreaker when university norms are genuinely mixed.

### LD-PIL-03: Learning Outcomes — PLOs [PIL]

**Standard.** Every Pillars course has exactly one PLO. It defines the course's primary skill — what a student can do with everything they've learned in a professional or real-world context.

**PLO Bloom's level by course type:**

- **Project-Primary.** Create or Evaluate level. Avoid "Demonstrate command of...", "Explore / Examine...", and references to specific tools or platforms that may go out of date.
- **Exam-Primary.** Apply level or higher. In math and science courses where students perform calculations across scenarios, Apply is appropriate.

**PLO formula by course type:**

- **Project-Primary.** [Creative skill centered on a deliverable] + [standard or methodology]. Example: "Create an evidence-based plan to improve organizational performance by applying frameworks for leadership, motivation, team dynamics, and change management."
- **Exam-Primary.** [Evaluative or applied skill] + [content domain] + [method or application context]. Example: "Evaluate the spontaneity, rate, and equilibrium behavior of chemical reactions by applying thermodynamic, kinetic, and electrochemical principles to predict and explain the outcomes of dynamic chemical systems."

**Rationale.** The PLO is the anchor for the entire course. A well-formed PLO makes CLO writing straightforward and ensures the final assessment has a clear, defensible purpose.

### LD-PIL-04: Learning Outcomes — CLOs [PIL]

**Standard.** CLOs are the discrete, observable skills that add up to the PLO.

- Maximum **8 CLOs per course**.
- CLOs start with **Apply, Analyze, Evaluate, or Create** level Bloom's verbs.
- Every topic should connect to at least one CLO. If a topic has no CLO, either the topic shouldn't be there or a CLO is missing.
- Every CLO must visibly contribute to the PLO. If you can't explain how a CLO helps a student achieve the PLO, cut it.

**Rationale.** CLOs define the observable skills the course is building. Requiring them to roll up to the PLO prevents CLO proliferation and keeps the course focused.

### LD-PIL-05: Exam-Assessable CLOs [PIL]

**Standard.** In Exam-Primary courses, all CLOs must be assessable by a timed exam unless a supplemental assignment is explicitly included to cover the exception. Three CLO types that are not exam-assessable and require a supplemental assignment:

- Lab technique CLOs — require a lab add-on or science interactive component.
- Communication CLOs — require a written assignment or presentation.
- Computational tool CLOs — require a software-based assignment.

If a CLO cannot be made exam-assessable and no supplemental assignment is feasible, **flag to Laura and Greg before proceeding.**

**Rationale.** Exam-Primary courses cannot backwards-design from CLOs that the exam cannot assess.

### LD-PIL-06: LO Assessment Frequency [PIL]

**Standard.** The 2x assessment requirement [See Rize LDS: LD-ARC-02] applies to Pillars courses with one modification: in Exam-Primary courses, timed exams count toward the 2x requirement in addition to assignments. **Every PLO and CLO must be assessed in at least two separate, graded activities** via assignments or timed exams.

To count toward the 2x requirement, the assessment must use a 6-level rubric (for assignments) or a documented question mapped to the CLO (for exams).

**Rationale.** Restricting the 2x requirement to assignments alone would make it impossible to fulfill in exam courses without adding unnecessary assignments.

---

## Part 3 — Course Plan Standards

The Course Plan is the high-level blueprint created before unit-level building begins. Complete syllabus design (Part 2) before opening the course plan template.

### LD-PIL-07: Three-Act Structure [PIL]

**Standard.** All Pillars courses use a three-act structure governing unit sequencing and assessment pacing. The act structure is fixed; unit-by-unit content decisions belong to the ID and PC.

| Act | Units | Name | Purpose |
|---|---|---|---|
| 1 | 1–3 | Onboarding | Orient students to the course, tools, and expectations. Low-stakes early actions build confidence. |
| 2 | 4–11 (Project) / 4–12 (Exam) | Core | Build skills progressively. Contains midterm checkpoint(s), Career Milestones (100/200-level), and — for Project-Primary — final project proposal and draft. |
| 3 | 12–14 (Project) / 13–14 (Exam) | Final | Demonstrate cumulative mastery through project + Async Fair (Project) or cumulative review + final exam (Exam). |

> Act 2 ends at Unit 11 for Project-Primary courses and Unit 12 for Exam-Primary courses.

**Fixed elements — require Laura/Greg sign-off to change:**

- Onboarding assignments in Unit 1.
- MyCareer Onboarding CM in Unit 3 (100/200-level only).
- Midterm checkpoint in Act 2 (default Unit 7; ±1 unit with sign-off).
- ROI Journey CM in Units 7–9 window (100/200-level only).
- Final Project Proposal (default Unit 10) + Draft (Unit 11) + Submission (Unit 13) + Async Fair (Unit 14) — Project-Primary.
- Cumulative Review (Unit 13) + Final Exam (Unit 14) — Exam-Primary.

**Flexible elements — ID and PC own these:**

- Topic sequence within each act.
- Assessment types per unit.
- Precise midterm location within Act 2 (with sign-off).
- Whether to include a second midterm.
- Exam length, question types, and open/closed notes.

**Rationale.** The act structure provides consistent scaffolding across Rize's Pillars catalog while preserving disciplinary flexibility.

### LD-PIL-07A: Act 1 Onboarding Requirements [PIL]

**Standard.** Before asking students to do any real work, Act 1 must accomplish three things:

- Show students where to find everything they need — syllabus, resources, submission process, and support.
- Set honest expectations about weekly workload and what the course will ask of them.
- Walk students through at least one of each core action — submitting work, joining a discussion, taking a quiz — through low-stakes activities before those actions carry real grade weight.

**Rationale.** For many Pillars students, this is their first online course. Act 1 is their entire impression of what online learning feels like.

### LD-PIL-08: Point Budgets by Act [PIL]

**Standard.** All Pillars courses are worth **1000 points**. Point budgets by act:

| Act | Units | Project-Primary | Exam-Primary |
|---|---|---|---|
| Act 1: Onboarding | 1–3 | 100–250 pts | 100–250 pts |
| Act 2: Core | 4–11/12 | ~350–550 pts | ~300–550 pts (includes midterm) |
| Act 3: Final | 12–14 / 13–14 | 150–300 pts | 350–450 pts (35–45% of course grade) |

**Additional constraints:**

- **No more than 300 points due in a single unit.** Exception: the final exam unit (standalone proctored exam, no workload-stacking risk).
- **Project-Primary.** Final project arc components (proposal + draft + submission + async fair) must not exceed 400 points total (40% of course grade). Point values must scale within the arc — submission > draft > proposal.
- **Exam-Primary.** Final exam is 350–450 points (35–45% of course grade). The final exam is the only HonorLock-proctored assessment.
- Point values should scale with effort and complexity within each act.

**Rationale.** Explicit act-level budgets give IDs guardrails while leaving individual unit weights to their discretion.

### LD-PIL-09: Activity Mix [PIL]

> **This standard replaces LD-ACT-01 for Pillars courses.**

**For Project-Primary courses:**

- Final project arc (Proposal, Draft, Submission, Async Fair) is required. All exceptions must be approved during course review.
- If the course PLO cannot be authentically assessed through a single culminating project, an alternative final arc may be approved (must include scoping/proposal stage, at least one feedback cycle, and a sharing/reflection component). Requires Laura/Greg sign-off.
- Midterm checkpoint is required; format is flexible.
- Discussions are recommended where they would generate genuine discourse — history, psychology, literature, sociology. Not mandated.
- Quizzes are optional but appropriate where there is significant conceptual content.
- Exams are permitted at the midterm checkpoint where conceptual knowledge needs to be verified.

**For Exam-Primary courses:**

- Midterm exam and final exam are required. Only final exam is proctored.
- Problem sets are a required assignment type for most exam-primary courses (see LD-PIL-19).
- **No more than 2 units may have a primary assessment that is not a problem set** (i.e., an assessment that directly practices exam question types).
- **No more than 5 units may include a secondary assessment** (quiz or discussion).
- Discussions are permitted where they would generate genuine discourse; not required.
- An applied project is permitted if it reinforces procedural skills, has one feedback cycle only, point value does not exceed the midterm, and is placed in Act 2.

**For both course types:**

- **Maximum two activities per unit** (progress quizzes are exempt from the two-activity cap).
- Every item on a unit page needs a reason to be there — Pillars course topics and LOs are dense.

**Rationale.** Pillars courses are content-dense and serve students who are skeptical of busywork. The two-activity cap enforces intentionality.

### LD-PIL-10: Quizzes vs. Exams [PIL]

**Standard.** Pillars courses distinguish between two assessment types that use similar question formats but serve different purposes:

**Quizzes** [See Rize LDS: LD-QZ-01] are low-stakes, formative, and untimed. Designed for knowledge checks and retrieval practice. Point values should be low relative to assignments and exams.

**Exams** (midterm and final) are summative, higher-stakes assessments built in Sanity. Rules:

- Timed.
- Final exam only: HonorLock proctored. Confirm activation with Greg before publishing.
- Midterm exam: Unproctored. Treat as a high-stakes practice run, not a security checkpoint.
- Open or closed note — must be documented in the course plan before unit build begins.
- Must be cumulative or cover a defined, substantial content window — not a single-unit knowledge check.
- Point value must be significantly higher than any individual quiz in the course.

**Rationale.** Quizzes build confidence and provide low-stakes retrieval practice. The final exam is the summative proof of mastery — and the only assessment with HonorLock enforcement.

### LD-PIL-11: Career Milestones [PIL]

**Standard.** Career Milestones are plug-and-play activities created by the software product team. IDs place them according to LD-PIL-07; they do not write or modify CM content.

- CMs must not have learning outcomes attached in the course plan. They are course-agnostic.
- For Fall 2026: CMs run in 100/200-level courses only. Two Pillars CMs: MyCareer Onboarding (Unit 3) and ROI Journey (Units 7–9 window). 300-level courses are exempt.

**Rationale.** CMs deliver Rize's career promise without burdening IDs with content creation or LO mapping.

### LD-PIL-12: Primary Instructional Resource Identification [PIL]

**Standard.** Before unit build begins, the ID and PC must identify a primary instructional resource — a textbook, OER, or video series — and map it to the course topic sequence. This is a course plan stage requirement, not a unit build decision.

If no suitable primary resource exists, or there are concerns about instructional gaps, **flag to Laura and Greg at course plan review.**

**Rationale.** You cannot scaffold problem sets, design exam coverage, or sequence assignments without knowing what students are learning from.

---

## Part 4 — Unit Build Standards

The standards in this section govern individual unit construction. For all standards not listed here, see the Rize LDS Part 3 [See Rize LDS: LD-ACT-02 through LD-GRD-06].

### LD-PIL-13: Exam-Primary Build Sequence [PIL]

**Standard.** Exam-Primary courses follow a different build sequence than Project-Primary courses. Because content must follow strict conceptual progression rather than thematic organization, the sequence is:

| Step | Stage | What Happens |
|---|---|---|
| 1 | Course Topics | Define what the course covers in disciplinary sequence. Topics are not Bloom's outcomes; they are the content areas a student needs to master. |
| 2 | PLOs / CLOs | Derive learning outcomes from topics. CLOs must be exam-assessable (LD-PIL-05). |
| 3 | Exam Generation | AI-assisted tool generates exam questions from course text and CLOs, human-checked by SME. Surfaces content gaps and sets the assessment target before unit build begins. |
| 4 | Worked Examples | AI-generated worked examples created for each distinct exam question type, human-checked. 4–5 examples per question type. |
| 5 | Unit Build | Back-solve unit structure from exam and worked examples. Unit sequence follows content progression. |

**Rationale.** Standard backwards design from CLOs produces thematically organized units — the wrong structure for exam courses that follow strict conceptual progressions.

### LD-PIL-14: Walkthroughs, Exemplars, and Templates [PIL]

**Standard.**

**For Exam-Primary courses:** For each distinct question type in the AI-generated exam, provide 4–5 worked examples. One per question type is designated for video walkthrough (P1 courses only); the remainder are annotated written examples paired with at least one practice problem. Worked examples must be reviewed by an SME or PC before publication.

**For Project-Primary courses:** Follow [See Rize LDS: LD-ACT-07] with two additions:

- Exemplars are required for all final project arc milestones (proposal, draft, submission).
- Templates should be provided wherever students are producing a structured deliverable.

**For both course types:** Quick-reference resources are required in units introducing complex new concepts.

- Format: key terms glossary, concept overview, or formula reference page.
- Placed first in the unit, before other resources and problem sets.
- Required for: units introducing a major new topic, and units building on prerequisite knowledge students may not have retained.

**Rationale.** Students need to know what success looks like before they try to produce it. Worked examples replace the scaffolding function of lectures.

### LD-PIL-15: Quizzes as Retrieval Practice [PIL]

**Standard.** Quizzes in Exam-Primary courses must include retrieval practice from prior units, not only current unit content.

- At minimum, every third unit must include a progress quiz drawing 1–2 questions from prior units alongside current unit content.
- More frequent interleaving is encouraged where the content sequence supports it.
- Progress quizzes are exempt from the two-activity-per-unit cap (LD-PIL-09).

**Rationale.** Interleaved practice from prior units replicates the pacing and accountability function of lectures across the 14-unit course structure without adding student workload.

### LD-PIL-16: Quiz and Exam Question Types [PIL]

**Standard.** Pillars courses use two different question type sets depending on the assessment type. **Quizzes are restricted to auto-gradable types only.** Exams may use all available types.

**Quiz question types (auto-gradable only — no file upload or essay):**

| Question Type | Best For | Notes |
|---|---|---|
| Multiple Choice | Conceptual understanding, definitions, identifying correct processes | |
| True / False | Misconception checks, fact verification | |
| Multiple Answers | Identifying all correct items from a set | |
| Matching | Pairing terms with definitions, reactions with types | |
| Number Entry | Integer answers, exact unit conversions, rounding-sensitive answers | Set acceptable range based on sig figs and rounding conventions where needed. |
| Essay | Brief written responses requiring human review | Not auto-graded. Use sparingly in quizzes. |

**Exam question types — three sections:**

**Section 1 — Auto-Gradable** (assesses KLO-level knowledge):

| Question Type | Best For | Notes |
|---|---|---|
| Multiple Choice | Conceptual understanding, definitions, identifying correct processes | |
| True / False | Misconception checks, fact verification | |
| Multiple Answers | Identifying all correct examples from a set | |
| Matching | Pairing terms with definitions, reactions with types | |
| Numerical Answer — Exact | Integer answers, exact unit conversions | No tolerance. |
| Numerical Answer — Precision | Sig fig-sensitive answers | Sanity checks significant figures. |
| Numerical Answer — Range | Answers where rounding variation is expected | Set acceptable range. |
| Text (no question) | Section headers, instructions, formula sheets (if open note) | |

**Section 2 — Short Answer** (assesses CLOs and KLOs):

| Question Type | Best For | Notes |
|---|---|---|
| File Upload | Multi-step problems requiring shown work, diagrams, lab data interpretation | Must be used for showing work/process. |
| Essay | Written justification, mechanism explanation, data interpretation narrative | |
| Text (no question) | Section headers, instructions, formula sheets (if open note) | |

**Section 3 — Long Answer** (maps to the PLO):

| Question Type | Best For | Notes |
|---|---|---|
| File Upload | Multi-step problems requiring shown work, diagrams, lab data interpretation | Must be used for showing work/process. |
| Essay | Written justification, mechanism explanation, data interpretation narrative | |
| Text (no question) | Section headers, instructions, formula sheets (if open note) | |

**Rationale.** Quizzes are low-stakes and auto-graded. The three-section exam structure scales with cognitive demand.

### LD-PIL-17: Peer Review in Pillars Courses [PIL]

**Standard.** Peer review in Pillars courses is conducted through discussion forums where students share work and respond to peers. When using discussion forums for peer review:

- Peer response is required (exception to [See Rize LDS: LD-DSC-02] default of optional responses).
- Provide a structured response prompt with specific criteria — not "respond to two peers."
- Grade as a separate, low-stakes, effort-based task ([See Rize LDS: LD-GRD-06] applies).
- Stagger due dates: initial post by end of Unit N, peer responses by end of Unit N+1.
- A student's grade cannot depend on receiving a peer response — if a peer does not respond, the student is not penalized.

**Rationale.** Peer review supports learning in project-primary courses but requires structured scaffolding to function well asynchronously.

### LD-PIL-18: Word Count and Length Guidance [PIL]

**Standard.** Less is almost always better. Bloated writing and dense unit structure are a direct impediment to learning for the Online Skeptic.

| Element | Guidance |
|---|---|
| Context sections | 200 words max. Engage students quickly, frame the ask, and let them know what they're making. |
| Resource intro framing | Under 100 words. Orient the student to the resource — don't summarize it. |
| Resource core content | No hard limit, but over 800 words is a signal the resource may need to be split into two. |
| Resource core content — external | Replace the core content section with a link. No additional text or summary needed. |
| Resource closing / source list | Optional. Under 50 words, excluding links. |

Every sentence should pass the purpose test: if removing it wouldn't confuse a student, remove it.

**Rationale.** Pillars students are managing dense content across multiple courses at their home institution. Respecting their reading load is a design requirement.

### LD-PIL-19: Problem Set Structure [PIL]

**Standard.** Problem sets are a Pillars-specific assignment subtype built in Sanity requiring three components:

- Learn assignment page (instruction layer) — built in Sanity.
- Downloadable problem set file (work product layer) — Google Doc or Word, with make-a-copy link.
- Grading guide / answer key — instructor-facing only.

The purpose of Steps in problem sets is to orient students to the format and requirements of the problem set, integrate resources at point of need, and remind students to show their work. **Steps do not restate or preview the problems.**

**Rationale.** Problem sets need to be downloadable documents so students can work offline, print, or annotate on iPad. Answer keys with specific point breakdowns are required because rubrics are not appropriate for this assignment type — correct answers are verifiable and partial credit is determined by work shown, not by holistic judgment.

#### LD-PIL-19A: Problem Set Sizing [PIL]

**Standard.** Problem sets in *exam-primary* courses are faithful representations of the exam question types students will see on midterms and the final, sized to give them practice in the same formats and at proportionally similar workload depth.

**SA-to-LA workload ratio (~1:2.5).** The LD-PIL-21 exam point weighting (SA = 6 pts / 5 min; LA = 15 pts / 12 min; ratio ≈ 1:2.5) reflects depth of work, and that ratio holds in problem sets too. **LA questions in problem sets are roughly 2.5× the work of SA questions.** That's the point of the SA-vs-LA distinction — short-answer items assess discrete CLO-spanning skills; long-answer items assess deeper integrative reasoning in novel scenarios. Problem sets that flatten this ratio (treating SA and LA as equal-effort) misrepresent exam difficulty.

**First-encounter scaling.** Problem-set versions of SA and LA questions may run somewhat longer than their exam equivalents — students are encountering the material for the first time and benefit from slightly more scaffolding inside the problem prompt. Rough targets:

- Problem-set SA: ~10–15 minutes each (vs. exam's 5 min)
- Problem-set LA: ~25–30 minutes each (vs. exam's 12 min)
- The ~1:2.5 ratio between SA and LA still holds at the problem-set scale.

**Mix of SA and LA is required.** Problem sets must include both SA and LA items in any unit where both are warranted by the unit's CLO/KLO coverage. A problem set composed entirely of LA items (the MATHS-VIII Unit 11 failure pattern) does not give students the practice variety they need and consumes the unit's workload budget on too few problems.

**Minimum question count per problem set:**

- **Unit 3 onward:** at least 4 questions per problem set, with at least one SA and at least one LA when both are pedagogically appropriate. Late-course units (post-midterm) particularly benefit from 4–6 questions to mirror the breadth of final-exam practice.
- **Units 1 and 2:** may have fewer where heavy scaffolding lengthens individual question scope; surface a rationale in the unit's design lock entry.

**Total problem-set time** matches the unit's workload contract — typically 60–120 minutes of student work for a 30-pt primary problem set, scaling with point value for heavier units.

**Rationale.** Problem sets serve as practice surfaces for the exam. Preserving the exam's SA:LA workload ratio gives students faithful preparation; flattening it (or collapsing the problem set to a few long items) breaks the practice-to-exam fidelity and exhausts the unit's workload budget. The ~1:2.5 ratio is what makes SA "short" and LA "long" — keep it.

### LD-PIL-20: KLOs (Exam-Primary) [PIL]

**Standard.** KLOs (Knowledge-Level Objectives) are discrete, observable units of knowledge derived from CLOs.

- Format: `[Bloom's verb] + [object] + [scope]`.
- Each KLO must be independently teachable and independently assessable.
- All Pillars exam-primary courses must define **35–55 KLOs per course**.
- KLOs are documented in the LO Map sheet of the course plan.
- KLOs map to exam question types: each KLO is assessed by at least one Section 1 (auto-gradable) question. Short response and long response questions assess one CLO and 2+ KLOs each.

**Rationale.** KLOs are the unit of instructional design in exam-primary courses. They derive bottom-up from exam question types and surface coverage gaps before unit build begins.

### LD-PIL-21: Standard Exam Structure (Exam-Primary) [PIL]

**Standard.** All final exams in exam-primary Pillars courses use a three-section structure with this fixed question distribution:

| Section | Type | Count | Points each | Section pts | Time est. | Purpose |
|---|---|---|---|---|---|---|
| 1: Auto-gradable | MCQ / TF / Multi-Answer / Matching / Numerical | 15 | 2 | 30 | 22.5 min | KLO breadth coverage |
| 2: Short Answer | Essay or File Upload | 5 | 6 | 30 | 25 min | CLO standard assessment |
| 3: Long Answer (CLO) | Essay or File Upload | 3 | 15 | 45 | 36 min | CLO deep assessment |
| 3: Long Answer (PLO / behavioral) | Essay or File Upload | 1 | 15 | 15 | 12 min | Behavioral PLO assessment |
| **Total** | | **24** | | **120 raw** | **95.5 min** + **24.5 min buffer** | |

The 120 raw points scale to the LD-PIL-08 final-exam range (350–450 pts) at upload time; the raw distribution above is what the framework generates against.

**Per-type LO-mapping rules:**

- **Section 1 (Auto-gradable, all subtypes):** one KLO in isolation.
  - *Exception:* Matching pairs 2–3 KLOs within one CLO.
- **Section 2 (Short Answer):** one CLO + 2+ KLOs.
- **Section 3 Long Answer (CLO):** one CLO + 2+ KLOs in a novel scenario.
- **Section 3 Long Answer (PLO / behavioral):** the integrative PLO skill, drawing across the course's CLOs.

**Final long answer maps to the PLO.** The 1 PLO-LA assesses the integrative skill the PLO defines and is structurally distinct from the 3 CLO-LAs. This is the course's culminating question.

**Midterm structure.** Midterms use the same three-section structure and roughly the same per-section proportions as the final, scaled to a smaller question count (typically 100–200 raw points per LD-PIL-21 / LD-PIL-08). The midterm functions as a dress rehearsal — same format, same conditions, unproctored. Students who have taken the midterm should have no surprises when they sit the final.

**Rationale.** A fixed distribution makes exam construction consistent across courses, supports question-bank reuse, and gives the build framework a target to generate against. Midterm alignment ensures students have accurate calibration on their own performance before the proctored final.

> **Framework implication.** The course's question bank must, by end of build, contain at minimum: 15 AG, 5 SA, 3 LA-CLO, 1 LA-PLO rows allocated to `Exam Bank`, with appropriate KLO/CLO mappings per the rules above. Generation across units accumulates toward this target. The framework's per-act and release-gate checks verify the running totals approach this distribution.

#### LD-PIL-21A: Multi-Semester Exam Pool [PIL]

**Standard.** The single-administration target above (24 items / 120 raw points) is the *minimum*. Courses that run multiple cohorts per academic year (typically Fall + Spring) require a **pool** sized for that rotation, so each cohort sees a different exam and no question repeats across the same student's midterm and final.

**Default multiplier:** **2.5×** the single-administration target. This supports two distinct semester exams plus a partial reserve for makeups, item retirement, and rotation. For a standard Pillars course:

| Section | Per-administration | 2.5× pool |
|---|---|---|
| Section 1 (Auto-gradable) | 15 | 37–38 |
| Section 2 (SA) | 5 | 12–13 |
| Section 3 (LA — CLO) | 3 | 7–8 |
| Section 3 (LA — PLO/behavioral) | 1 | 2–3 |
| **Pool total** | **24** | **~60** |

**The same logic applies to midterms.** Per LD-PIL-21, midterms use a scaled-down version of the same three-section structure (typically 100–200 raw points). The midterm pool should also be 2.5× a single midterm administration.

**Course-template override.** Courses with one cohort per year (rare for Pillars) may operate at 1× target with explicit Laura/Greg sign-off in the design lock. Courses with three cohorts per year scale up to 3.5×.

**Rationale.** Without a pool, the same exam runs every semester — answer-sharing across cohorts erodes assessment validity within one or two terms. A pool sized for two distinct administrations plus reserves gives the course a sustainable rotation. The 2.5× multiplier is a working default; calibrate against the program's actual cadence.

#### LD-PIL-21B: Final ≤ Hardest Pset Difficulty [PIL]

**Standard.** Exam items targeting a given KLO must not exceed the difficulty of the hardest problem-set item targeting the same KLO in the unit that introduces that KLO. **Students should encounter every concept at exam-or-greater depth in problem sets *before* the exam tests it.** The exam is a representative sample of work students have already done in scaffolded form; it is not a step-up in challenge.

**Generation-side rule.** When drafting an Exam Bank row for KLO X, the conductor reads the existing Problem Set rows for KLO X in the unit that owns it. The exam item should match the difficulty of one of those problem-set items — typically the harder applied-tier ones — but not exceed them. If no Problem Set row exists for KLO X yet, the conductor pauses and surfaces the gap rather than drafting an exam item that students will encounter for the first time on the exam.

**Audit-side rule (D2 standards auditor, scope = unit-batch and release).** For each Exam Bank row, the auditor verifies that at least one Problem Set row in the same KLO's owning unit assesses the same KLO at equal or greater scaffolding depth. Findings:

- **PASS** — at least one matching pset row at ≥ exam difficulty exists.
- **WARNING** — pset row exists but at lower depth. Surface to ID for review.
- **BLOCKER** — no pset row exists for this KLO at all. Students would meet this concept first on the exam.

**Rationale.** Students should feel the exam is a fair sample of work they've already done. Problem sets are practice surfaces; exams are graded performance. If students hit problem-set items easier than the final, they feel sandbagged; if they hit problem-set items harder than the final, they feel prepared and the exam validates their preparation. The framework's calibration target is the second condition, every time.

---

## Part 5 — Accessibility

[See Rize LDS: ACC-01 through ACC-06] — All accessibility standards apply to Pillars courses in full with no modifications. IDs are responsible for heading structure, link text, alt text, emphasis, PDF avoidance, and image quality as defined in the Rize LDS.

> **Framework note.** Rize LDS standards (`ACC-*`, `LD-ACT-*`, `LD-DSC-*`, etc.) referenced throughout this document are not yet captured in this repo. Tracked as a gap in `standards-architecture.md`.

## Part 6 — QA & Formatting

[See Rize LDS: QA-01 through QA-12] — All QA and formatting standards apply to Pillars courses in full with no modifications. See course-level templates for Pillars-specific QA checklists.

---

## Part 7 — Appendices

### Appendix A — Pending Decisions

The following areas require additional decisions before standards can be written. **Do not build against these until they are resolved.**

| Area | Open Question | Owner |
|---|---|---|
| Final exam scaling | Build guidance for final exams given it's worth 350–450 points | Greg |
| Skills framework in Pillars | Role of job skills in Pillars courses; how to capture skill signal in exam-based courses without rubric criteria | Laura + Greg + Zak |
| Spaced practice for language / fluency courses | Distinct guidance needed for vocab acquisition and sequential language courses | Laura |
| AI honesty policy / stoplight system | How the stoplight system applies in Pillars courses, including English/writing edge cases | Greg |
| Office hours reframe | Reframing weekly sessions as exam prep rather than office hours requires ops team alignment | Laura + Ops |
| Course Topics standard | Topics as a distinct layer between classification and LOs. Skipped for now; revisit once core build is complete | Laura |

### Appendix B — Controlled Vocabulary: Assignment Types

The following labels are used in the course plan. Every activity must use the correct Sanity type and label. **Spelling, spacing, and capitalization must match exactly.**

| Label | Sanity Type | Course Type | Required? | Unit Placement |
|---|---|---|---|---|
| Onboarding Tasks | Assignment | Both | Yes | Unit 1 |
| Career Milestone: MyCareer Onboarding | Career Milestone | Both | 100/200-level only | Unit 3 |
| Career Milestone: ROI Journey | Career Milestone | Both | 100/200-level only | Units 7–9 |
| Midterm Exam | Exam | Exam-Primary | Yes | Act 2 (default Unit 7) |
| Final Exam | Exam | Exam-Primary | Yes | Unit 14 |
| Midterm Checkpoint | Assignment | Project-Primary | Yes | Act 2 (default Unit 7) |
| Final Project Proposal | Assignment | Project-Primary | Yes | Act 2 (default Unit 10) |
| Final Project Draft | Assignment | Project-Primary | Yes | Act 2 (default Unit 11) |
| Final Project Submission | Assignment | Project-Primary | Yes | Unit 13 |
| Async Fair | Discussion | Project-Primary | Yes | Unit 14 |
| Problem Set | Assignment | Exam-Primary | Recommended | Act 2 |
| Applied Project | Assignment | Exam-Primary | Optional | Act 2 only |
| Quiz | Quiz | Both | Varies | Any |
| Discussion | Discussion | Both | Varies | Any |
| Assignment | Assignment | Both | Varies | Any |

---

*This document is the source of truth. Edit here; regenerate derivatives via `scripts/compile_standards.py`. See `standards-architecture.md` for how each standard maps into the framework's enforcement layers.*
