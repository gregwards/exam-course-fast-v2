# Unit 2 Problem Set: Answer Key (Instructor-Facing)

**Per LD-PIL-19:** This is the grading guide, not a rubric. Problem sets use answer keys with point breakdowns; partial credit is determined by work shown, not holistic judgment.

This problem set follows the OpenIntro Section 3.2 pedagogy: contingency table → tree diagram → Bayes' Theorem as the formal generalization. Problem 1 is structurally identical to OpenIntro Example 3.42 (page 105) with substituted numbers; Problem 2 reuses the contingency-table tool at large scale.

---

## Problem 1: The COVID test (16 pts)

### Part (a): Contingency table (3 pts)

For 10,000 people in the area:

- Has COVID: 10,000 × 0.02 = 200 (column total provided to students).
- Does NOT have COVID: 10,000 × 0.98 = 9,800 (column total provided to students).

Of the 200 with COVID:
- Test positive: 200 × 0.95 = 190
- Test negative: 200 × 0.05 = 10

Of the 9,800 without COVID:
- Test positive: 9,800 × 0.02 = 196
- Test negative: 9,800 × 0.98 = 9,604

| | Has COVID | Does NOT have COVID | Row total |
|---|---|---|---|
| **Test positive** | 190 | 196 | 386 |
| **Test negative** | 10 | 9,604 | 9,614 |
| **Column total** | 200 | 9,800 | 10,000 |

**Point breakdown:** 1 pt for correct application of sensitivity (190 / 10). 1 pt for correct application of specificity (196 / 9,604). 1 pt for correct row totals (386 / 9,614). Award full credit if all four interior cells are correct, even if intermediate work is incomplete. Deduct 0.5 pt for each cell off by more than rounding.

### Part (b): Tree diagram (3 pts)

Students should produce a tree with:

- Primary branches: "Has COVID" (0.02) and "Does not have COVID" (0.98).
- Secondary branches under each: "Test positive" and "Test negative" with conditional probabilities.

Joint probabilities (products along each path):
- Has COVID and test positive: 0.02 × 0.95 = 0.0190
- Has COVID and test negative: 0.02 × 0.05 = 0.0010
- Does not have COVID and test positive: 0.98 × 0.02 = 0.0196
- Does not have COVID and test negative: 0.98 × 0.98 = 0.9604

Sum: 0.0190 + 0.0010 + 0.0196 + 0.9604 = 1.0000. ✓

**Point breakdown:** 1 pt for correct tree structure (two primary, four secondary, conditional probabilities labeled). 1 pt for correct joint probabilities at the leaves. 1 pt for verifying the four joint probabilities sum to 1.00. Sketches are fine; precision in the structure matters more than aesthetic quality.

### Part (c): P(COVID | test positive) from the contingency table (3 pts)

P(COVID | test positive) = (people with COVID who test positive) / (people who test positive) = 190 / 386 ≈ **0.4922 ≈ 49.2%**

**Point breakdown:** 2 pts for correct setup (recognizing that the denominator is the row total of "test positive," not the population total). 1 pt for correct numerical answer. Accept any answer between 49% and 50% as correct.

### Part (d): Verify with tree diagram (3 pts)

P(COVID | test positive) = P(COVID and test positive) / P(test positive) = 0.0190 / (0.0190 + 0.0196) = 0.0190 / 0.0386 ≈ **0.4922 ≈ 49.2%**

This should match part (c) to three decimal places.

**Point breakdown:** 1 pt for correct setup (joint over marginal). 1 pt for correct denominator (sum of two "test positive" branch joint probabilities). 1 pt for correct answer matching part (c). If answers differ by more than rounding, the student likely made a mistake in the tree structure; refer back to part (b).

### Part (e): Connect to Bayes' Theorem (2 pts)

P(COVID | positive) = (0.95 × 0.02) / [(0.95 × 0.02) + (0.02 × 0.98)] = 0.0190 / (0.0190 + 0.0196) = 0.0190 / 0.0386 ≈ **0.4922 ≈ 49.2%**

Same answer as parts (c) and (d). The Bayes formula is the formal version of the tree-diagram ratio.

**Point breakdown:** 1 pt for correct substitution into the formula (correct identification of P(B|A), P(A), P(B|A^c), P(A^c)). 1 pt for correct numerical answer matching parts (c) and (d).

### Part (f): Plain-language interpretation (2 pts)

Acceptable answer should:
- State that the probability is around 49%, not the 90%+ most people guess (1 pt).
- Reference the base rate (low prevalence ~2%) as the reason: false positives outnumber true positives because there are so many more uninfected people being tested (1 pt).

A student who writes formulas without plain-English explanation should receive at most 1 of 2 points.

---

## Problem 2: DNA evidence (9 pts)

### Part (a): Contingency table for 5,000,000 people (3 pts)

| | Is the perpetrator | Is NOT the perpetrator | Row total |
|---|---|---|---|
| **DNA matches** | 1 | ~5 | ~6 |
| **DNA doesn't match** | 0 | ~4,999,994 | ~4,999,994 |
| **Column total** | 1 | 4,999,999 | 5,000,000 |

Reasoning:
- Perpetrator (1 person) matches with probability 1.00 → 1.
- Innocent people (4,999,999) match with probability 0.000001 → expected ~5 random matches.
- Perpetrator can't be in the "doesn't match" row.
- Innocents who don't match: 4,999,999 - 5 ≈ 4,999,994.

**Point breakdown:** 1 pt for the "DNA matches" row count of perpetrator (1) and innocent (~5). 1 pt for the row total of ~6. 1 pt for clear setup showing the multiplication 4,999,999 × 0.000001. Accept "5" or "4-6" as correct for innocent matches.

### Part (b): P(is the perpetrator | DNA matches) (4 pts)

P(is perpetrator | matches) = (people who match AND are the perpetrator) / (people who match) = 1 / 6 ≈ **0.167 ≈ 16.7%**

**Point breakdown:** 1 pt for recognizing that "matches" includes both the perpetrator and the random matches. 1 pt for adding the 1 (perpetrator) to the ~5 (random matches). 2 pts for correct final answer. Accept any reasoning that yields 14% to 20%.

A more sophisticated student might note: this assumes equal prior probability across all city residents (no other evidence), which is the assumption stated in the problem. Acknowledge this caveat if they raise it.

### Part (c): Plain-language interpretation (2 pts)

Acceptable answer should:
- Identify the "one in a million" probability as P(match | innocent person) — the probability OF a match GIVEN someone is innocent (1 pt).
- Identify the jury's question as P(is the perpetrator | match) or equivalently P(innocent | match) — and explain that confusing these two is the prosecutor's fallacy (1 pt).

A student who writes formulas without plain-English explanation should receive at most 1 of 2 points.

---

## Misconception remediation summary

This problem set targets two misconceptions documented in the Garfield/delMas/Chance literature:

- **Base-rate neglect** (primary): Problem 1 forces students to confront the role of low prevalence in producing false positives that outnumber true positives. The contingency table in part (a) and the tree diagram in part (b) are the remediation devices. Plain-language interpretation in part (f) confirms the conceptual transfer.
- **Prosecutor's fallacy / P(A|B) ≠ P(B|A)** (secondary): Problem 2 forces students to articulate the difference between two conditional probabilities that the prosecutor's argument conflates. The contingency table at population scale (5,000,000) makes the conflation visible.

If a student gets parts (a), (b), (c), (d) of Problem 1 correct but the interpretation parts (e), (f) and Problem 2 part (c) incorrect, that's a signal the procedural skill is in place but the conceptual one isn't. Recommend follow-up practice in the unit's discussion forum or via the optional practice items.

## Total points: 25
