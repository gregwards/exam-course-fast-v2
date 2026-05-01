# Persona Filter Finding Format

This is the canonical format for persona filter outputs. The persona filter prompt produces files matching this structure; the conductor reads them; the ID reviews aggregated findings.

Findings without a severity are invalid — the prompt enforces this explicitly.

---

## File structure

A complete persona filter review is one markdown file with this shape:

```markdown
# {Persona Name} — Review of {Artifact Name}

**Reviewer:** {Persona name and source path, e.g., "Online Skeptic (personas/online-skeptic.md)"}
**Artifact:** {What was reviewed, e.g., "design/course-plan-recommendation.md (locked 2026-04-25)"}
**Date:** YYYY-MM-DD
**Context the reviewer saw:** {Brief description: persona card + standards Part 1 + design lock + the artifact. Confirms isolation.}

## Summary

{2–3 sentence narrative read in the persona's voice. What did this artifact feel like to read as this persona? Did it land? What was the dominant impression?}

## Findings

### Finding 1 — {BLOCKER | WARNING | NOTE}

**Test failed:** {Clarity test | Path test | Purpose test | Sequence test | Hierarchy of needs | Every action earned | Other named test from standards Part 1}

**Location:** {Specific quote, section reference, or cell coordinate from the artifact}

**Observation:** {What the persona noticed, in the persona's voice. Concrete, not generic. "I read the second sentence twice and I still don't know whether 'briefly' means 100 words or 500" — not "this could be clearer."}

**Why it matters:** {Consequence for the student / course opinion / workload / engagement. One or two sentences.}

**Fix hint (optional):** {If the reviewer has a specific suggestion. Often left empty — the reviewer's job is to falsify, not to fix.}

### Finding 2 — {...}

...

## Adjustments the AI made before surfacing to the ID

{If the AI applied light iterations based on filter findings before showing the artifact + this review to the ID, list them here. Each adjustment names the finding it responded to and what changed. Example:

- *Finding 3 (WARNING — clarity test):* Revised the second sentence of the Course Opinion from "Chemistry is more than just a set of formulas" to "Chemistry is a set of models you build, layer by layer, until you can predict what matter will do." Removed the negation; foregrounded the positive claim.

If no adjustments were made — either because findings were all NOTE-severity or all escalated to ID — say so explicitly: "No adjustments applied; all findings surfaced to the ID for judgment."}
```

---

## Severity definitions

**BLOCKER** — The artifact fails the persona's experience in a way that will lose the student. Cannot ship without addressing. Escalates to ID rather than the AI attempting auto-iteration. Examples: instructions are not followable; the artifact's stated purpose contradicts what the work actually asks for; the path to completion is broken.

**WARNING** — The artifact has a real problem the persona would notice and that affects engagement or clarity, but doesn't prevent the student from completing. AI may apply light iterations to address before surfacing; if iterations don't resolve, escalates to ID. Examples: a context section restates instructions; a step references a resource by ambiguous name; the cognitive shift between two units isn't named so the student can't see the through-line.

**NOTE** — The artifact has something the persona observed that's worth flagging but doesn't require action. Often a quality observation rather than a defect. Surfaced to ID as informational. Examples: a unit description does the persona-engagement work especially well; a transition between acts is more abrupt than ideal but acceptable; a phrasing choice is fine but a sharper alternative exists.

---

## What the format prevents

The four-field structure (severity, location, observation, why-it-matters) is deliberately constrained:

- **Severity** prevents findings without weight ("this could be clearer" with no scale).
- **Location** prevents abstract critique that can't be acted on.
- **Observation in the persona's voice** prevents AI-rubric language ("the artifact would benefit from") that doesn't help the ID understand what the persona experienced.
- **Why it matters** prevents pedantic findings that catch the letter without naming the consequence.
- **Fix hint optional** prevents the reviewer from drifting into generator role. The reviewer's job is to falsify; the ID decides whether to accept findings.

---

## What this format is NOT for

This is the persona filter output format. Other discriminators have their own:

- **D1 mechanical scripts** — produce JSON pass/fail per check; not this format.
- **D2 standards auditor** — produces findings referencing specific standard IDs (LD-PIL-04, LD-ACT-05, etc.) in a similar but standards-anchored shape; see `templates/audit-finding.md` (TBD).
- **D4 SM validator** — produces AGREE / REASONING DIVERGENCE / SUBSTANTIVE DISAGREEMENT classifications per question; see `templates/validation-finding.md` (TBD).

Different discriminators, different findings formats, different files in `reviews/`. Each is structurally distinct so that aggregating them at gates produces clear signal stratified by discriminator type.
