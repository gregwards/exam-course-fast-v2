# Unit 2 Problem Set: Conditional Probability and Bayes' Theorem

**Total points: 25**
**Time estimate: ~3 hours of work on the problems (this is included in the 5-hour total estimate on the Learn page, not extra).**

Show all work. Partial credit is awarded for clear setup. Where the problem asks for plain-language interpretation, write complete sentences.

This problem set follows the exact pedagogy of OpenIntro Section 3.2: contingency table → tree diagram → conditional probability by counting. Problem 1 is structurally identical to the worked mammogram example in the textbook (Example 3.42, page 105). Refer to that worked example whenever you get stuck.

---

## Problem 1: The COVID test that breaks intuition (16 points)

You take a rapid COVID test. Here's what we know about the test and the population:

- About **2% of people in your area currently have COVID**. This is the **marginal probability** of infection: P(COVID) = 0.02.
- The test gives a **false negative in 5% of infected people**. So if you do have COVID, the probability the test correctly says you do is 1 − 0.05 = 0.95.
- The test gives a **false positive in 2% of healthy people**. So if you don't have COVID, the probability the test correctly says you don't is 1 − 0.02 = 0.98.

Your test comes back positive. Most people, given numbers like these, guess the probability of actual infection at 90% or higher. In 1982, the researcher David Eddy gave a similar problem to 100 physicians; 95 of them estimated the probability of disease at around 75%. The correct answer is much lower. You're going to find it.

This problem follows the same approach OpenIntro uses in Example 3.42 (page 105). Refer back to that worked example as needed.

### Part (a): Build a contingency table for a population of 10,000 (3 points)

Imagine 10,000 people in your area. Fill in this contingency table with the count in each cell. Show your reasoning for at least one cell.

| | Has COVID | Does NOT have COVID | Row total |
|---|---|---|---|
| **Test positive** | _____ | _____ | _____ |
| **Test negative** | _____ | _____ | _____ |
| **Column total** | **200** | **9,800** | **10,000** |

(The column totals are filled in for you: 2% of 10,000 is 200, so 200 people actually have COVID and 9,800 don't.)

To fill the cells:
- Of the 200 with COVID, 95% test positive and 5% test negative.
- Of the 9,800 without COVID, 2% test positive and 98% test negative.

### Part (b): Build the same scenario as a tree diagram (3 points)

OpenIntro Example 3.42 represents this kind of problem with a **tree diagram**. Build one for the COVID scenario. Your tree should have:

- Two primary branches: "Has COVID" (probability 0.02) and "Does not have COVID" (probability 0.98).
- Two secondary branches under each primary branch: "Test positive" and "Test negative," with the appropriate conditional probabilities.
- The joint probability at the end of each path (the product of the probabilities along that path).

The four joint probabilities should sum to 1.00. Verify this. (See the textbook's Figure on page 105 for the layout.)

### Part (c): Compute P(COVID | tested positive) using the contingency table (3 points)

Looking at your contingency table from part (a), focus only on the row "Test positive." What fraction of the people in that row actually have COVID?

P(COVID | test positive) = (number who have COVID and test positive) / (number who test positive)

Compute this from your table. Express your answer as a decimal and as a percentage.

### Part (d): Verify the answer using the tree diagram (3 points)

OpenIntro's tree-diagram approach computes the same probability as a ratio of joint probabilities:

P(COVID | test positive) = P(COVID and test positive) / P(test positive)

where P(test positive) is the sum of the two "test positive" branches' joint probabilities (one through "has COVID," one through "doesn't have COVID").

Use your tree diagram from part (b) to compute this ratio. Confirm that you get the same answer as part (c). (Round both to three decimal places to compare.)

### Part (e): Connect to Bayes' Theorem (2 points)

The OpenIntro callout box on page 106 names the formal version of what you just did:

P(A|B) = [P(B|A) × P(A)] / [P(B|A) × P(A) + P(B|A^c) × P(A^c)]

For our COVID problem, A is "has COVID" and B is "test positive."

Substitute the four probabilities (P(test positive | COVID) = 0.95, P(COVID) = 0.02, P(test positive | no COVID) = 0.02, P(no COVID) = 0.98) directly into this formula. Compute the result. Confirm that it matches your answers in parts (c) and (d).

You've now solved the same problem three ways: by counting from a contingency table, by reading from a tree diagram, and by Bayes' Theorem formula. They all give the same answer.

### Part (f): Interpret in plain language (2 points)

Imagine you have a friend who is panicking after a positive rapid test. In two to three sentences, explain in plain English (no formulas) what the probability you computed actually means and why a positive test alone doesn't mean someone has COVID. Reference the **base rate** (the 2% prevalence) explicitly. The base rate is what makes the answer counterintuitive.

---

## Problem 2: The DNA evidence reversal (9 points)

A criminal investigation finds DNA at the scene. The forensic lab reports a 1-in-1,000,000 random match probability, meaning a random person from the general population has a 0.0001% chance of matching the DNA profile. The defendant's DNA matches. The prosecutor argues in court that this all but proves guilt: "the chance of a random match is one in a million."

You're a juror. The city has a population of 5,000,000. Use the same contingency-table approach you used for Problem 1.

### Part (a): Build a contingency table for the 5,000,000-person city (3 points)

Assume the actual perpetrator is one specific person in the city, and they are guaranteed to match the DNA (P(match | guilty) = 1.00). Everyone else has a 1-in-1,000,000 chance of matching.

Fill in the contingency table:

| | Is the perpetrator | Is NOT the perpetrator | Row total |
|---|---|---|---|
| **DNA matches** | _____ | _____ | _____ |
| **DNA doesn't match** | _____ | _____ | _____ |
| **Column total** | **1** | **4,999,999** | **5,000,000** |

To fill the "DNA matches" row:
- The perpetrator matches with probability 1.00, so the count is 1.
- Each of the 4,999,999 innocent people matches with probability 0.000001, so the expected count is 4,999,999 × 0.000001 ≈ 5.

### Part (b): Compute P(is the perpetrator | DNA matches) (4 points)

Looking at your contingency table, focus on the row "DNA matches." What fraction of those people are the actual perpetrator?

P(is the perpetrator | DNA matches) = (number who match AND are the perpetrator) / (number who match)

Compute this from your table. Express your answer as a decimal and as a percentage.

The defendant in our case is one of the people in the "DNA matches" row, but the police pulled them in randomly with no other evidence. Your answer above is the probability that this defendant is actually the perpetrator.

### Part (c): Identify the prosecutor's error in plain language (2 points)

In two to three sentences, explain in plain English what the prosecutor got wrong. Specifically:

- The "one in a million" probability is a conditional probability. *Which* conditional probability? (Hint: P(match | innocent) or P(innocent | match)?)
- The probability the jury actually wants is the *other* conditional probability. Which one?
- Why is treating these two probabilities as the same number a serious mistake?

---

**End of problem set.**
