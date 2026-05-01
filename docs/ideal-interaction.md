# Ideal Course Builder Interaction — Source of Truth

**Status:** Canonical. Supersedes `system-spec-v4.md` where they conflict.
**Captured:** 2026-04-24, from a working session with Greg.

This document describes the intended shape of the conversation between the instructional designer (ID) and the course-builder AI. It is the document every other artifact in this framework defers to. When the v4 spec and this exemplar disagree, this exemplar wins.

The principle the exemplar embodies: **when possible, the AI builds an example of the next step in the process for the ID to give feedback on.** That makes the conversation as efficient as possible turn-by-turn — the AI ships work, the ID reacts, neither of them spends turns on questions that a draft would answer faster.

---

## The exemplar conversation

**ID:** Let's build this course.

**AI:** Great, I see you've provided the course syllabus and a copy of the textbook. Are there any other materials or context you'd like to provide before we get started? We'll begin by collecting your vision for the course, then building the course plan.

**ID:** No, that's all I have to start with.

**AI:** OK, let's start with the course vision. Give me a description, as detailed as you can, of what you think a great version of this course looks like or does. If you're unsure at this stage, we can leave this for now and come back to it at a later stage.

**ID:** OK, this is intro to statistics. I'm fairly familiar with the subject, and the reality is there aren't that many ways to teach it — the key is making sure the problem sets scaffold out concepts really clearly, we provide lots of practice problems should students choose to engage with them, as seeing and solving a broad diversity in problems is the best way for students to make connections, and then ensuring the lecture content is as high quality as possible.

**AI:** Great, that all makes sense. One clarifying question — when you talk about practice problems, are you talking about those within the standard walkthrough videos or are you envisioning additional practice problem resources in this course?

**ID:** I'm envisioning an additional central repository of practice problems that map to textbook chapters for students to practice — we'll link the relevant section in each unit on the resource page.

**AI:** Got it. The next step is the research phase to inform the course plan — do you have any pointers on specific sources or angles to research or should I progress with the typical research plan?

**ID:** I think normal is probably fine, but because it's such a common topic I would love to see if there's any particularly innovative websites or YouTube channels out there with a novel approach to scaffolding practice problems. If we find great YouTube videos that may change the scope of our lecture videos.

**AI:** OK, starting research now, with a focus on innovative video channels and online courses… [creates research artifact / file and gives a high-level summary]

Given this research, I would make the following recommendations for the course plan:

- **Course Opinion:** "If you torture the data, it will say anything" — Statistics is a tool, and like any tool it can be used for good and bad. Good = uncovering real truths through large data analysis, bad = misconstruing the truth by distorting statistical methods.
- **3-act structure:**
  - **Act 1, Units 2–5: Foundational concepts.** [AI explains heavily scaffolded assignments that anchor in real-world intuition, with ideas for making each distinct unit interesting and not repetitive]
  - **Act 2, Units 6–10: Analysis practice.** [AI explains progressively lighter scaffolded problem sets that all connect directly to exam preparation, including the midterm]
  - **Act 3:** [etc... this one would focus on the final]
- **What to avoid:** the risk with intro to statistics is that students get lost trying to wrap their heads around really abstract concepts, and so just revert to learning pattern-matching approaches to problem set and exam questions. We will avoid that through good problem set scaffolding that connects to high-value lectures.
- **What success looks like:** if this course is truly successful, students will get "lightbulb" moments just from completing the problem sets. Concepts like correlation vs. causation, statistical significance will "click" and they'll want to bring them up in conversation — this is the aim of Act 1 and part of Act 2, before the course laser-focuses on final exam prep.

How does that sound? Any adjustments or recommendations before I draft the course plan?

**ID:** [provides feedback and some steering]

**AI:** Great, I'll make those adjustments. Building the course plan now… [generates the course plan as two CSVs — `lo-map.csv` and `assessment-map.csv` — then runs the standard student persona filter on the course plan to validate perceived workload and course flow, makes light iterations based on that, and then presents both files to the user in the Cowork interface for review and iteration]

[ID provides feedback and adjustments, AI iterates, ID confirms it's good]

**AI:** Great, beyond the course plan, I also have the following build notes: [provides additional notes from the ID and research relevant to the course build but not fully captured in the course plan itself, like specific case studies to use, particular unit notes, etc.]

I recommend building out the problem sets for Unit 2 and Unit 10 as exemplar assignments to show two assignments at different stages of the course trajectory. Does that work for you?

**ID:** Yes

**AI:** [produces assignments for each unit, following the chain-of-thought assignment-building process that adheres to prior context, course plan, and learning design standards, then applies the user persona filter, iterates based on its feedback, and presents the ID with the iterated version with notes on the adjustments made from the user persona filter]

[ID provides feedback on these and the AI iterates until the ID confirms]

**AI:** Would you like me to build out one or both of these units as exemplar units?

[ID provides a selection of unit or units as the exemplar unit]

[ID then asks for all of Act 1 to be built, with subject matter validation — this is the validation step — then does the same for the full course]

[the ID reviews, iterates, and then invokes the Sanity upload script which populates the course in Sanity]

The Sanity upload script is a script that uploads the full course content into the content management system that we use — this will be developed separately.

---

## Annotations and clarifications

A handful of clarifications captured in the same working session that refine the exemplar:

**Exemplar count is reasoned, not fixed.** The default is two exemplar assignments at different points in the course trajectory. The AI chooses three when the course's diversity warrants it (e.g., a third act that's pedagogically distinct from the second). The reasoning is surfaced to the ID so they can override.

**Course plan output is two CSVs, not an xlsx.** `lo-map.csv` and `assessment-map.csv` — the same schemas the v3 framework already uses. Cowork's UI presents them side-by-side or tabbed; the file format is CSV.

**Per-act validation samples within the act.** Validation runs once per act on a sampled subset of that act's questions. Not every question in every unit. Same blind-context machinery as v3, different cadence.

**The persona filter is one canonical asset, applied by the AI before the ID sees the work.** The AI runs the filter, makes light iterations, then surfaces the iterated version to the ID with notes on what changed. There aren't separate flatness, monotony, and opinion-audit agents — those sensibilities are baked into the persona filter itself. Different personas catch different things; the library may grow over time.

**Learning design standards are not negotiable.** Greg has a body of codified standards. The agent must not violate them. The architecture for enforcement is open but the working assumption is: standards baked into templates the agent works from, the full standards available as reference, and light scripted checks during build to catch the structural violations.

---

## What this exemplar tells us about the framework

A few load-bearing implications:

The agent is a **single conversational entity** from kickoff through release, not a roster of named specialized agents. The persona filter is an internal pass the agent runs; the subject matter validator is an internal pass the agent runs. The ID only sees one voice.

The agent **ships work, then asks for feedback** — almost never asks open-ended questions when a draft would substitute. The one exception is the vision elicitation step, where the ID has knowledge the AI doesn't and where a draft would anchor poorly.

The course plan, build notes, and exemplar assignments form a **tightly coupled trio** that anchors everything downstream. The plan tells the AI what to build; the build notes tell it the texture; the exemplar assignments show it the bar. Once those three are locked, the rest of the course is in-context replication of that bar against the plan.

**Sanity is the terminus.** Every artifact this framework produces eventually maps to Sanity's content model. The Sanity upload script is out of scope here, but the schemas this framework writes need to be Sanity-compatible.

---

## What this exemplar deliberately leaves open

The exemplar does not specify the shape of:

- The research artifact's internal structure
- The standard student persona filter's internal prompt
- The course plan recommendation's internal structure (opinion + acts + what to avoid + success)
- The build notes document's structure
- The chain-of-thought assignment-building process
- The standards reference structure
- The Sanity upload schema

Each of those is downstream design work that defers to this exemplar's conversational shape. The shape is the constraint; the implementations are the work.
