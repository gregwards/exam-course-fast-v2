# Unit-deliverable exemplars

These are real, locked, ID-approved deliverables from completed unit builds. The conductor reads them as **in-context calibration** during Phase 6/7/8 rendering — they are the templates. The framework intentionally does NOT synthesize generic markdown templates from rules; templates derived from rules in the absence of real exemplars produce output that drifts from house style. Real exemplars beat any rule-derived template.

## What's in here

```
unit-deliverables/
├── README.md                           ← this file
├── MATHS-VIII-unit-02/                 ← early-unit example (locked, "really accessible for unit 2")
│   ├── problem-set.md                    student-facing pset, 2 problems with heavy scaffolding
│   ├── learn-page.md                     Sanity learn page (Turn In / Context / Steps)
│   └── answer-key.md                     instructor-facing key with model answers + point breakdowns
├── MATHS-VIII-unit-11/                 ← late-unit example (locked, exam-mirror shape)
│   ├── problem-set.md                    student-facing pset, exam-mirror format
│   ├── learn-page.md                     Sanity learn page
│   └── answer-key.md                     instructor-facing key
├── MATHS-VIII-exemplar-annotations.md  ← Greg's per-course operationalization of the standards
│                                          for MATHS-VIII (course-specific norms emerged in build)
├── resource-pages/                     ← Tier 3 Rize Remix resource pages (locked)
│   ├── learning-from-failures.md         food-service course outbreak case studies
│   └── training-posters-that-work.md     visual-design guidance for poster assignment
└── lecture-notes/                      ← lecture instructor-reference docs (locked)
    ├── anatomy-unit-04-tissues.docx      anatomy lecture: "Two Questions That Identify Any Tissue"
    ├── anatomy-unit-04-tissues.md        markdown extract for conductor reading
    ├── HISTY-III-unit-9-video-1.docx     history lecture: "The 30-Year Window"
    └── HISTY-III-unit-9-video-1.md       markdown extract
```

## How the conductor uses these

When rendering a unit's deliverables in Phase 6/7/8:

1. **Read both unit exemplars** (Unit 02 + Unit 11). Together they bound the framework's scaffolding range — Unit 02 shows early-unit heavy scaffolding; Unit 11 shows late-unit exam-mirror shape. Whatever unit the conductor is building falls somewhere on that arc.
2. **Read the per-course `exemplar-annotations.md`** at `{course_dir}/design/exemplar-annotations.md` (if it exists). This captures course-specific norms emerged during Phase 6 — voice anchors, citation patterns, forbidden phrasings, course-specific shape rules. Greg's MATHS-VIII version (`MATHS-VIII-exemplar-annotations.md` here) is the working example of what this file looks like populated.
3. **Read the resource page exemplars** when generating Tier 3 (Rize Remix) resources per LD-RES-04. Both shown examples are tied to a specific assignment ("For Assignment X..."), use H2-organized sections per QA-07, end with Sources per QA-11.

## What each exemplar exemplifies

### Unit 02 (early-unit pattern)

Demonstrates how scaffolding lengthens individual question scope in early units. Lower question count, more guidance per problem, textbook references woven through prompts (per Operating Principle #11). The "learn page" is concise — Context section in instructor-natural voice, Steps action-oriented and numbered with explicit titles.

### Unit 11 (late-unit / exam-mirror pattern)

Demonstrates the exam-mirror format the framework adopts from Unit 8 onward (per MATHS-VIII annotations). Problem set top-section structure: Title → Resources → Question count summary → Showing-your-work block → per-problem sections. Per-problem prose embeds narrative inside the prompt rather than quarantining it in a separate Context section. Answer key has metadata block (Q-IDs, KLOs, CLOs, scenario tag), model answers per part, deterministic point breakdowns (per LD-PIL-19, no rubrics).

### `exemplar-annotations.md` (per-course operationalization)

Greg wrote this for MATHS-VIII at end of Phase 6. It bridges the standards (universal) to the course (specific): how LD-PIL-19A's SA:LA ratio shows up in this course's problem sets, what voice the Context sections use, what real-world citations are in scope, what forbidden phrasings the prompts call out. Phase 7 (exemplar units) and Phase 8 (per-act build) read it as binding course-level guidance.

When the conductor builds a new course, it writes the equivalent file at `{course_dir}/design/exemplar-annotations.md` during Phase 6 lock — capturing the course-specific norms that emerged during the exemplar iterations.

### Resource pages (Tier 3 / Rize Remix shape)

Both examples are food-service course resources. Patterns to notice:

- **Assignment-tied framing.** Both lead with *"For Assignment X..."* — they exist to support a specific assignment, not as standalone reading.
- **Concrete examples over generic guidance.** Outbreak page uses Chipotle 2015 + Wendy's 2022 with specific facts, dates, financial consequences. Training-posters page uses specific poster examples (chopping board chart, FDA handwashing, "Would You Want These People In Your Kitchen") and walks through what each does well.
- **Plain-language analysis.** Both end with an actionable framework: "When you analyze your chosen outbreak, search for..."; "Before you open a design tool, answer these questions..." — operationalizing the reading for the assignment.
- **H2 sections with descriptive titles** per QA-07.
- **Sources at the bottom** per QA-11. Mix of CDC, FDA, trade press, and academic sources.

### Lecture notes (instructor reference docs)

The two lecture exemplars demonstrate the format target for any lecture-notes deliverable:

- **Title** that pairs a concept handle with a hook (*"Two Questions That Identify Any Tissue,"* *"The 30-Year Window"*).
- **"Instructor Reference"** subtitle — the doc is for the SME delivering the lecture, not the student.
- **Ordered sections**: Hook → Concept 1 → Concept 2 → ... (1–4 concepts) → Close.
- **Each concept section has three bordered tables**: Key points (3–5 bullets, the load-bearing claims), Key terms to hit (specific vocabulary the lecture must name), Illustration (description of visual to display, not a drawn image).
- **Hook and Close sections** have Key points + Illustration but typically no Key terms.
- **"Detailed Example"** at the end with **Say/Show two-column tables** walking through delivery — one paragraph or two of actual instructor language per row, with the corresponding visual cue. Per the doc framing: *"One way to deliver each section. Reference, not script. If you hit every key point and every key term, the lecture works regardless of how you deliver it."*
- **No marketing language, no theatrical flourishes.** The voice is concrete, grounded, evidence-anchored. The HISTY exemplar opens with three specific dates (*"In 619, a Sasanian Persian army occupies Alexandria..."*); the anatomy exemplar opens with a real student frustration (*"Your body has over 200 cell types. But you only need two questions..."*).
- **Format: .docx** (the pedagogically-relevant Sanity-and-print artifact). The framework also keeps a .md extract for the conductor's reading; the .docx is the deliverable.

When the conductor generates lecture notes, it reads both exemplars to calibrate against the form, then drafts a unit-specific lecture matching the structure with course-specific content. Anatomy and history are deliberately different domains in the exemplar set — the format generalizes; the content doesn't.

## What's not here yet

- **Walkthrough reference exemplars** (Phase 7 calibration). When a course gets to Phase 7 and Greg locks a walkthrough reference he considers strong, duplicate it here.
- **Quick-reference resources** (per LD-PIL-14, formula sheets / glossaries / concept overviews). The framework needs an exemplar to calibrate against; first one to get locked goes here.
- **Answer key exemplars beyond MATHS-VIII**. Two answer keys from one course doesn't show variation across course types; over time, add more from chemistry, polisci, etc.

## How to add new exemplars

When a unit's deliverables get locked with positive ID review during a real build:

1. Duplicate the relevant files into a new folder here (e.g., `CHEMY-I-unit-04/problem-set.md` etc.).
2. Update this README's "What's in here" tree to include them.
3. Add a brief paragraph in "What each exemplar exemplifies" naming what new pattern this exemplar shows that the existing set doesn't.

The exemplar set should grow over time as the framework runs against more courses.
