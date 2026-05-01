# Exam Fast Mode — Orientation

A quick orientation for instructional designers using this plugin for the first time. If you're already mid-build and want to skim this for a refresher, the phases section is the part to scan.

---

## What this does

The plugin builds a Rize Pillars course alongside you, end-to-end. You bring the syllabus and the textbook reference. It drives the build conversation through every phase — from initial vision through to final release-ready content. The framework's architecture and the learning design standards stay invisible to you. Your time goes on judgment and voice — the choices that make this course memorable rather than just compliant.

## How the conversation works

You don't run commands. You don't fill out forms. You talk.

You provide source material (syllabus, textbook). The build assistant produces work — a research summary, a course plan recommendation, exemplar assignments, populated units. You react: confirm what's right, redirect what isn't, ask questions. At the end of each phase there's a checkpoint where the work gets saved before the build moves on. You can pause at any checkpoint and come back days later.

If something the assistant produces is wrong — a bad question, a missed standard, a weird voice — just say so in plain language. The assistant edits the framework's instructions based on your feedback so future builds catch the same issue automatically. (This is mostly Greg's mode during framework iteration; most IDs won't be editing the framework itself.)

## The phases

A complete build moves through ten phases. You'll see a checkpoint at the end of each one.

| Phase | What happens | What gets produced |
|---|---|---|
| **0. Kickoff** | Confirm classification (Exam-Primary or Project-Primary) and what materials you've provided. | Course folder created. |
| **1. Vision** | You describe in your own words what a great version of this course looks like. Skip if you don't have one yet. | `course-vision.md` |
| **2. Research** | The assistant researches the subject, comparable courses, and pedagogical approaches relevant to this course's level. | `research-summary.md` |
| **3. Course plan recommendation** | A unified course opinion (one paragraph), three-act narrative, what to avoid, and what success looks like. You react and lock. | Course Narrative tab in the course plan xlsx |
| **4. Course plan build** | The LO Map (PLO and CLOs from your syllabus verbatim, KLOs derived) and Assessment Map (14 units with topics, points, assessments, resources, descriptions) populate. | `course-plan-exam.xlsx` (or project version) |
| **5. Build notes** | Insights from research and conversation that didn't fit the plan — specific case studies, scenarios, unit-level guidance. | `build-notes.md` |
| **6. Exemplar assignments** | Two assignments at different points in the course's trajectory, built fully and reviewed before any other unit work begins. | Two locked exemplar assignments |
| **7. Exemplar units (optional)** | If useful, expand one or both exemplar assignments into full units (problem set, walkthrough, exam-bank rows, quick-reference resources). | Locked exemplar unit(s) |
| **8. Per-act build** | You ask for "all of Act 1" (or 2, or 3); the assistant builds the units in sequence and runs reviews at the end of each act. | All units for the act |
| **9. Release review** | A final pass reads everything against the locked course opinion to flag any drift before upload. | Release review document |
| **10. Sanity upload** | Out of scope for this plugin. You invoke the upload script separately when you're ready to ship. | Course in Sanity |

You can pause at any checkpoint. To come back to a course later, just say *"continue {course-name}"* and the build picks up where you left off.

## What you get

By the end of a full build, in your course's folder:

- A populated course plan xlsx (Course Narrative + LO Map + Assessment Map).
- Per-unit problem sets with answer keys.
- A question bank for midterms and the final exam.
- Walkthrough scripts and worked examples.
- Resource pages and quick-reference materials.
- Review records — what the student-experience and standards reviews caught at each phase.

Everything lives at `~/Documents/Rize-Courses/{course-name}/` (or wherever you chose to keep your courses). The folder structure is consistent across courses, so you'll always know where to look.

## How to start

Just say:

> Let's build a course.

(Or, if you have one in flight: *"continue {course-name}"*.)

The build assistant will ask which course you're starting on, confirm the basics, and walk you through from there. There's no command syntax to memorize.

## If you need help mid-build

Ask in plain language. Say *"I'm not sure what to do here,"* or *"what does this step produce,"* or *"skip this step,"* or *"go back to the previous step"* — the assistant will adapt. The flow is designed to bend around what you actually need, not force you through a script.

## What the framework does behind the scenes

Three things happen automatically that you don't have to manage:

**Standards adherence.** Rize's learning design standards (the parent Rize LDS plus the Pillars-specific layer) are baked into the templates the assistant fills in and the prompts it uses. Where the standards admit a range or judgment, the assistant chooses based on course level and material.

**Quality reviews.** At checkpoints, the work passes through reviews — a student-experience read (the "Online Skeptic" persona, modeled on a real first-or-second-year undergraduate at a partner institution who's time-poor and skeptical of online courses), a standards check, and (for question-bank content) a subject-matter review. You see what each review caught, what the assistant revised, and what's still open for your call.

**Locked artifacts.** Once you confirm a phase's output, it's locked — downstream work treats it as fixed. If you later want to revise something locked, just say so; the assistant will confirm the unlock before redrafting.

These mechanics keep the build compliant and rigorous without requiring you to think about them. Your work stays at the level of *"is this the right opinion for this course"* and *"does this assignment work for the kind of student we're building for"* — not *"did I miss a standard"* or *"is this rubric the right size."*

---

*If you have specific questions about how a phase works, or you're stuck and the assistant isn't catching it, ping Greg. The framework is supposed to evolve through real ID use; your friction is the input.*
