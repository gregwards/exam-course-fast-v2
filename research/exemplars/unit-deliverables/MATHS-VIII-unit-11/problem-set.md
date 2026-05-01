# Unit 11 Problem Set: Inference via Simulation. Bootstrap CIs and Permutation Tests

**Resources**

- OpenIntro Statistics 4e, Sections 5.2 (confidence intervals via simulation) and 5.3 (hypothesis testing via simulation)
- StatKey: <https://www.lock5stat.com/StatKey/>
- A standard normal probability table or calculator for the formula-based portion of Problem 4

**Questions**

- 2 short-response problems
- 2 long-response problems

**Showing your work**

- Label each part clearly (a, b, c, …)
- **StatKey simulations:** include a screenshot or transcript showing your input values, the number of trials, and the reported result
- **Hand calculations:** show the formula, the substituted values, and the arithmetic
- **Interpretation parts:** write complete sentences in plain language; do not just rewrite a formula in words
- **Hypotheses:** state in both plain language and symbols

---

## Problem 1: Short Response

In a Cambridge garden in the 1920s, a colleague of Ronald Fisher's named Muriel Bristol claimed she could taste the difference between tea poured into milk and milk poured into tea. Fisher, a statistician with a sense of humor, designed an experiment. He prepared 8 cups (4 with tea-first, 4 with milk-first), randomized the order, and asked Bristol to identify which was which. She got all 8 right.

Fisher's question was: how impressed should I be? The probability of getting all 8 right by random guessing alone is 1/70 ≈ 0.014. The null hypothesis here is that Bristol is guessing; the alternative is that she can really taste the difference. The 0.014 number is your first p-value.

(a) Confirm 1/70 by hand. How many ways are there to split 8 cups into two groups of 4 (compute C(8, 4))? Of those splittings, how many produce Bristol's perfect identification (all 4 tea-first cups in the tea-first group)? Verify the ratio is 1/70.

(b) The p-value 0.014 is a number. In two to three sentences, explain in plain language what this number means in the context of Bristol's experiment. Avoid the textbook phrasing "probability of the data given the null"; write it the way you'd explain it to a friend who hasn't taken statistics.

(c) Verify the analytical result with simulation. In StatKey, open "Test for Single Proportion." Set the null proportion to 0.5 and observed count to 8 successes out of 8 trials. Run 10,000 simulated guesses with the "Right Tail" option and report the proportion of simulations that produced 8 successes by chance. State whether your simulation result is consistent with 0.014, and if your number is different, briefly explain why.

---

## Problem 2: Short Response

In late 2024, a randomized trial of a 4-day workweek on a 100-employee software team reported the following result: "Mean weekly productivity output increased by 7.4 percentage points (95% bootstrap confidence interval: [3.1, 11.6]) among employees on the 4-day schedule, compared to the 5-day control group."

The CEO reads this and tells the board: "Our productivity went up 7.4%. If we roll this out company-wide, we'll see at least a 7% gain across the company."

(a) State in plain language, in one or two sentences, what the bootstrap CI [3.1, 11.6] tells you about the productivity effect. What is the interval describing?

(b) The CEO's "at least 7% gain" claim is wrong on two distinct levels. Identify both. The first is a problem with how the CEO is reading the CI itself (what does the CI actually allow them to conclude about the gain in the trial population?). The second is about generalizing from the trial to a company-wide rollout (what could a 100-person trial fail to capture about the company at scale?).

(c) The bootstrap method requires the original sample to be representative of the population the study wants to generalize to. State one specific way this 100-employee software-team sample might fail to represent the broader workforce, and what that failure would do to the trustworthiness of the reported CI.

---

## Problem 3: Long Response

In 1979, a Stanford statistician named Bradley Efron published a paper that solved a problem statisticians had wrestled with since the 1900s: how do you build a confidence interval for a parameter when you don't know the underlying population's distribution? Efron's answer was so simple it sounded like a joke. Take your sample. Resample from it, with replacement. Do it a thousand times. The middle 95% of those resampled estimates IS your 95% confidence interval. He called it the bootstrap. By 2026 it's the default tool when standard normal-approximation assumptions are uncomfortable.

You're going to use it on a poll. The polling thread you've been following since Unit 9: a survey reports 47% support for Candidate A from a sample of 1,200 likely voters in a swing state. The pollster reports a margin of error of ±3 percentage points based on the standard normal-approximation formula.

(a) Run the bootstrap. In StatKey, open "CI for Single Proportion." Enter the sample proportion 0.47 and sample size 1,200. Generate 1,000 bootstrap samples. Report the 2.5th and 97.5th percentiles of the bootstrap distribution; those two values bound your 95% bootstrap CI.

(b) Compute the analytical standard error by hand using the formula SE = √(p̂(1 − p̂) / n). Plug in p̂ = 0.47 and n = 1,200. Compare your computed SE to the standard deviation of the bootstrap distribution from (a). They should be close.

(c) Compare your bootstrap CI to the pollster's reported ±3% (i.e., a 95% CI of [44%, 50%] from the normal-approximation formula). How close are the two? Which one would you trust more in this scenario, and why?

(d) The bootstrap doesn't assume normality. It doesn't assume σ is known. It doesn't require the Central Limit Theorem to apply. State, in one sentence, what the bootstrap DOES require for the resulting CI to be trustworthy.

(e) Imagine a friend reads the headline "Candidate A leads with 47% support" and concludes the candidate is going to win. Use your bootstrap CI to push back. Write two to three sentences in plain language explaining what the headline gets wrong and what the CI actually tells us about the race.

---

## Problem 4: Long Response

A small randomized trial tests a new drug against placebo for a chronic condition. 50 patients receive the drug; 50 receive placebo. The outcome is binary: at the 12-week follow-up, did the patient meet the pre-defined response criterion?

Results: 35 of 50 in the treatment arm responded (70%). 25 of 50 in the placebo arm responded (50%). The observed difference in response rates is 0.70 − 0.50 = 0.20.

The trial team wants to know: is this difference real, or could it be chance? You're going to answer it twice, once by simulation and once by formula.

(a) State the null hypothesis and alternative hypothesis for whether the treatment works, in plain language and in symbols (using p_T for the treatment response rate and p_P for the placebo response rate).

(b) Run the permutation test in StatKey. Open "Test for Difference in Proportions." Enter the data: treatment 35 successes out of 50, placebo 25 successes out of 50. Click "Generate 1000 Samples." StatKey will randomly permute the treatment-vs-placebo labels 1,000 times under the null and compute the difference in response rates each time. What proportion of the 1,000 permutations produced a difference at least as extreme as the observed 0.20? Use the right-tail option since the alternative is one-sided.

(c) Compute the formula-based version of the same test by hand. The pooled proportion is p̂_pooled = (35 + 25) / (50 + 50) = 0.60. Compute the two-sample z-statistic using z = (p̂_T − p̂_P) / √(p̂_pooled(1 − p̂_pooled)(1/n_T + 1/n_P)). Look up the corresponding one-sided p-value from a standard normal table or calculator. Compare to your permutation p-value from (b).

(d) State the p-value from your permutation test (or take it as approximately the same as the formula-based answer if they agree). Interpret it in the context of the trial. What does it tell the trial team about whether the treatment works? Avoid the textbook phrase "probability of the data given the null"; write it as you'd explain it to a clinician.

(e) The American Statistical Association's 2016 statement on p-values names six common misinterpretations of p-values. One says: "A p-value below 0.05 means the result is true." State why this misinterpretation is wrong, and explain in two to three sentences what a small p-value actually licenses the trial team to conclude (and what it does NOT license them to conclude).

---

**End of problem set.**
