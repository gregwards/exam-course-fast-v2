# Standards Auditor Finding Format

This is the canonical format for D2 (standards auditor) outputs. The auditor reads an artifact section-by-section against the codified standards, in the role of a compliance reviewer. Different from the persona filter (which reads as a real student); different from the SM validator (which reads as a textbook expert checking facts).

Findings reference specific standard IDs. The voice is precise and standards-anchored, not student-experiential.

---

## File structure

```markdown
# Standards Audit — {Artifact Name}

**Reviewer:** Standards Auditor (D2)
**Artifact:** {Artifact name and source path}
**Audit scope:** {Course Plan | Assignment | Unit Batch (Act N) | Release}
**Standards consulted:** {Which Parts of standards.md and rize-lds.md were the evaluation reference}
**Date:** YYYY-MM-DD
**Context the reviewer saw:** {One sentence: standards subset + design lock + the artifact. Confirms isolation.}

## Summary

{2–3 sentences. How many findings, what severity distribution, the dominant compliance gap if any. Compliance reviewer voice — precise, evaluative, not experiential.}

## Findings

### Finding 1 — {BLOCKER | WARNING | NOTE}

**Standard:** `LD-PIL-04` (CLOs: max 8, Bloom's verbs, PLO roll-up)

**Citation:** {Quote the exact standard text the artifact violates or under-serves. Multiple citations if more than one standard applies.}

**Location:** {Specific cell coordinate, section reference, or row in the artifact}

**Observation:** {What the auditor identified. Standards-anchored language. Example: "CLO #3 'Examine the policy landscape' opens with 'Examine,' which falls below the Apply / Analyze / Evaluate / Create Bloom's threshold the standard requires."}

**Why it matters:** {The downstream consequence. Compliance / pedagogical / structural. One or two sentences.}

**Fix hint (optional):** {If the auditor sees a specific compliant alternative. Example: "Recast as 'Analyze the policy landscape...' to meet the Bloom's threshold."}

### Finding 2 — {...}

...

## Cross-cutting observations (optional)

{If the auditor notices systemic patterns across multiple findings — e.g., "three CLOs use sub-Apply Bloom's verbs, suggesting the original draft predates the LD-PIL-04 update" — surface them here. Distinct from per-finding observations because they're patterns rather than instances.}
```

---

## Severity definitions

**BLOCKER** — The artifact fails a hard standard. Cannot ship without addressing. Examples: PLO doesn't follow the LD-PIL-03 formula; CLO count exceeds LD-PIL-04 maximum; required component missing from a problem set per LD-PIL-19.

**WARNING** — The artifact under-serves a standard or sits in a grey area. Could ship with documented exception, but better to address. Examples: a unit's `Steps` section contains explanatory teaching content that LD-ACT-06 says belongs in Resources; a Tier 2 resource lacks the brief framing LD-RES-04 requires.

**NOTE** — The artifact is technically compliant but the auditor surfaces a quality observation. Examples: the PLO is well-formed but reads more like a 200-level outcome than the 300-level the course is at; CLOs are within count but two could be combined to tighten the course.

---

## Audit scopes

The standards auditor runs in different scopes at different gates. Each scope determines which subset of the standards is the evaluation reference.

| Scope | When | Standards subset | Artifact under review |
|---|---|---|---|
| Course Plan | Phase 4, once | rize-lds.md Parts 1–2 + standards.md Parts 1–3 | course-plan-exam.xlsx (LO Map + Assessment Map + Course Narrative) |
| Assignment | Phase 6, per assignment | rize-lds.md Part 1 + Part 3 (relevant to assignments) + standards.md Parts 1, 4 | An assignment artifact |
| Unit Batch | Phase 8, once per act, sampled within the act | rize-lds.md Parts 1, 3 + standards.md Parts 1, 4 | Sampled unit artifacts from the act |
| Release | Phase 9, once | All of rize-lds.md + standards.md, plus Course Opinion alignment | All units + Course Narrative |

The scope is set by the runner script via `--scope` and determines which standards get loaded into the prompt's `<standards>` block. Other scopes' standards are excluded so the auditor stays single-objective.

---

## Voice and constraints

The auditor is a compliance reviewer, not a student. Findings should:

- Reference specific standard IDs (LD-PIL-04, LD-ACT-05, ACC-03, etc.).
- Quote the standard text being violated or under-served.
- Identify the specific location in the artifact.
- Use precise, standards-anchored language — not experiential or persona voice.

The auditor should NOT:

- Generate or rewrite the artifact (the auditor's job is to falsify; the generator decides what to change).
- Read for "student experience" — that's the persona filter's job.
- Read for factual correctness — that's the SM validator's job.
- Manufacture findings to seem thorough; if the artifact is compliant, say so explicitly.

---

## What this format is NOT for

This is the standards auditor's output format. Other discriminators have their own:

- **D1 mechanical scripts** — produce JSON pass/fail per check.
- **D3 persona filter** — `templates/persona-finding.md`.
- **D4 SM validator** — `templates/validation-finding.md` (TBD).

When findings from multiple discriminators converge on the same artifact, the conductor aggregates them severity-stratified for the ID — preserving the source discriminator so the ID can see which lens caught what.
