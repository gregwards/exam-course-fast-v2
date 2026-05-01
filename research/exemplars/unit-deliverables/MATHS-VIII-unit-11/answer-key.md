# Unit 11 Problem Set: Answer Key (Instructor-Facing)

**Per LD-PIL-19:** This is the grading guide, not a rubric. Problem sets use answer keys with point breakdowns; partial credit is determined by work shown, not holistic judgment.

This problem set assesses CLO 4 (statistical inference) primarily, with CLO 6 (statistical software) on the StatKey-bearing parts. Per the locked Phase 5 misconception-remediation table, the Unit 11 misconception targets are (a) p-value as P(null is true) and (b) bootstrap as data-creating / assumption-free. Both are surfaced in this problem set; specifically, Problem 1 (a) and Problem 4 (d) target the p-value misconception; Problems 2 (c) and 3 (c) target the bootstrap-assumptions misconception.

---

## Problem 1: Short Response (5 pts), SA-001

**KLOs:** KLO-29, KLO-44  
**CLO:** CLO-4  
**Scenario tag:** `lady-tasting-tea-fisher-1935`

### Model answer

(a) C(8, 4) = 8! / (4! · 4!) = 70 splittings of 8 cups into two groups of 4. Exactly 1 of those splittings has all 4 tea-first cups in the tea-first group (Bristol's perfect identification). Ratio: 1/70 ≈ 0.0143.

(b) The p-value 0.014 is the probability that someone with no real ability, just a random guesser, would get all 8 cups right purely by luck. Out of every 1,000 random guessers, only about 14 would match Bristol's perfect score by chance. The number is not the probability that Bristol's ability is real; it's the probability that random luck alone would produce data this extreme. The smaller the p-value, the harder it is to explain Bristol's result by chance.

(c) StatKey simulation with null p = 0.5, observed = 8/8, 10,000 trials, right tail: the proportion of simulations producing 8 successes is approximately 0.0035 to 0.005 (the analytical answer for 8 successes in 8 Bernoulli(0.5) trials is (0.5)⁸ = 0.0039). This differs from 0.014 because StatKey's "Test for Single Proportion" assumes 8 independent Bernoulli trials, while Fisher's tea-tasting design constrains Bristol to identify exactly 4 of each type, a hypergeometric structure with 70 total arrangements (which gives 1/70 ≈ 0.014). The two simulations agree on the qualitative answer ("very small probability of all-8 by chance") but encode different null models. A student who explains either consistency or the model-mismatch earns full credit.

### Point breakdown

(a, 1 pt): 0.5 pt for correct C(8, 4) = 70 with formula shown. 0.5 pt for identifying that exactly 1 splitting matches and computing 1/70. Accept hand-arithmetic or formula notation.

(b, 2 pts): 1 pt for "probability of getting all 8 right by chance / luck alone." 0.5 pt for explicitly NOT framing it as P(null is true). 0.5 pt for the "smaller the p-value, the harder to explain by chance" framing or equivalent. A student who only writes "probability of data given H₀" earns at most 1 of 2.

(c, 2 pts): 1 pt for correct StatKey setup with screenshot/transcript. 0.5 pt for reporting the simulation proportion. 0.5 pt for either (i) noting consistency with 0.014 or (ii) catching the Bernoulli-vs-hypergeometric model mismatch and briefly explaining.

### Instructor explanation

This problem is the unit's introduction to the p-value as a concept. The narrative is the prompt's setup; the question is the cognitive engagement. Fisher's tea-tasting experiment is the canonical "first p-value" anchor in intro stats, and the small numbers (8 cups, 70 arrangements) make the calculation tractable to inspect even though the student isn't asked to derive it.

The pedagogical move in (a) is to force a plain-language interpretation BEFORE students reach for the textbook phrasing "probability of the data given H₀." That phrasing is technically correct but routinely produces the misconception that the p-value is a probability OF the null hypothesis. Per the locked Phase 5 build notes' misconception-remediation table, this is the single most-documented misinterpretation in the Garfield/delMas/Chance literature; surfacing it in the first problem of the inference unit, and explicitly forbidding the textbook phrasing, is the calibration move.

The simulation in (b) is a deliberate friction point. StatKey's "Test for Single Proportion" treats Bristol's task as 8 independent Bernoulli(0.5) trials, which is a defensible but DIFFERENT null model than Fisher's hypergeometric one (where Bristol is told there are exactly 4 of each type). A student who runs the simulation and reports ~0.0039 has done correct work; a student who reports it WITHOUT noting the discrepancy has missed an opportunity. Either reasoning earns full credit; the model-comparison observation is bonus rigor, not required.

The Lady Tasting Tea anchor is per the locked Phase 5 decision ("Default: Tea as worked example, A/B as problem set anchor"). This problem makes Tea the assigned work, with the A/B framing showing up in Problem 4.

### Annotations

Section 2 mirror (exam SA shape). Compressed from Phase 5's 5-part Lady Tasting Tea LA per LD-PIL-19A and the 'exam-mirror format from Unit 8 onward' course principle. Hand-combinatorics derivation (1/70) is given in the prompt; conceptual core (interpret p-value, verify with simulation) preserved. Part (b)'s StatKey-vs-Fisher null-model mismatch is a bonus rigor moment; full credit for either reasoning.

---

## Problem 2: Short Response (5 pts), SA-002

**KLOs:** KLO-25  
**CLO:** CLO-4  
**Scenario tag:** `4day-workweek-bootstrap-ci-interpretation`

### Model answer

(a) The bootstrap CI [3.1, 11.6] is a range of plausible values for the TRUE productivity effect of the 4-day schedule on this team's population, based on the sampling variability we observed. The point estimate (7.4%) is our single best guess; the interval acknowledges that with 100 employees, that estimate could plausibly understate the effect (true value as high as ~11.6%) or overstate it (true value as low as ~3.1%). 95% confidence means: if we ran this trial design many times, about 95 in 100 of the resulting intervals would contain the true effect.

(b) First problem: the CI [3.1, 11.6] does NOT tell us we'll see "at least 7%." The CI says the true effect could plausibly be as low as 3.1%. The CEO's "at least 7%" floor is actually below the CI's lower bound. The CEO is confusing the point estimate (7.4%) with the lower confidence bound (3.1%). A correctly-stated CEO claim would be: "we have reasonable evidence the effect is somewhere between 3% and 12%, with our best guess around 7%."

Second problem: even taking the CI at face value, generalizing from a 100-employee software team to the full company is a separate issue. A software team isn't a representative sample of all departments; the bootstrap can quantify sampling variability within the studied population but cannot extrapolate to populations the sample didn't draw from. Sales, customer service, manufacturing, and other functions may respond differently, or not at all, to a schedule change.

(c) Examples (any one suffices): (i) software developers may have unusually high baseline workplace flexibility and concentration-driven workflow that benefits more from a compressed schedule than other roles; (ii) the team is a single tech function in one company's culture, not a cross-section of jobs; (iii) employees who joined the trial may have selected in (people who liked the idea of a 4-day week); (iv) the 100-person sample may overrepresent senior or independent contributors. The failure mode: the bootstrap CI quantifies sampling variability assuming the sample IS representative. If it is not, the CI is anchored on a biased point estimate, and its 3.1–11.6 range just reflects the uncertainty around that biased number, not the true uncertainty about the broader workforce.

### Point breakdown

(a, 2 pts): 1 pt for stating the CI is a range of plausible values for the true productivity effect (not the data, not P(true value in interval) for a frequentist interpretation). 1 pt for the "95% confidence ≈ 95% of intervals would contain the true value if we repeated the procedure" framing or equivalent. Accept either the frequentist or repeated-procedure framing; reject only "the true value is in this interval with 95% probability" without further qualification (Bayesian misreading).

(b, 3 pts): 1 pt for catching the CI-reading error: the CI lower bound (3.1) is below the CEO's "at least 7%" claim. 1 pt for explicitly distinguishing the point estimate from the lower confidence bound. 1 pt for the second-level critique: bootstrap quantifies sampling variability in the studied population only; can't extrapolate to other populations. A student who only addresses one of the two errors earns 1 or 2 pts depending on quality.

(c, 1 pt): 1 pt for any defensible specific failure mode (sample is one team / one function / one company / self-selected) AND a brief connection back to "what the bootstrap can't fix." Accept any of the listed failure modes or others that name a real selection mechanism.

### Instructor explanation

This problem is the unit's primary practice surface for KLO 25 (CI conceptual) without requiring students to construct a CI from scratch (that's Problem 3). Pure interpretation; pure transfer. Section 2 mirror.

The 4-day workweek scenario is plausible without being a known real study (real 4-day-week studies exist but their reported effect sizes vary widely, so this stylized version avoids inadvertently modeling on any specific paper's exact methodology). The CEO-misreading frame is realistic. It is exactly the kind of overstatement that appears regularly in business press coverage of organizational research.

Part (b) targets two distinct conceptual errors in one problem. The first (CI-reading) is the misconception inside the data: confusing "best guess" with "guaranteed minimum." The second (extrapolation) is the misconception OUTSIDE the data: thinking statistical inference handles selection bias. These are routinely conflated in real-world misreadings; pulling them apart in a 3-pt problem builds the discrimination skill.

Part (c) is intentionally short (1 pt) because the goal is to surface ONE concrete failure mode, not enumerate all possible ones. Students who write a paragraph here are over-investing; the answer should be a sentence or two naming a specific selection issue.

This problem also rehearses the KLO 25 transfer that the more constructive Problem 3 will demand. A student who can articulate what a CI means in this scenario is positioned to actually build one in the next problem.

### Annotations

Section 2 mirror. Pure interpretation problem (no construction). Pairs with Problem 3 (constructive bootstrap CI), interpret first, then construct. The 4-day workweek scenario is stylized to avoid modeling on any specific real published study's methodology. Part (b) targets two distinct misconceptions in one problem (CI-reading error + extrapolation error).

---

## Problem 3: Long Response (10 pts), LA-003

**KLOs:** KLO-25, KLO-43  
**CLO:** CLO-4, CLO-6  
**Scenario tag:** `bootstrap-1200-voter-poll-47pct`

### Model answer

(a) Running 1,000 bootstrap samples in StatKey at p̂ = 0.47, n = 1,200 produces a bootstrap distribution centered near 0.47 with standard error around 0.014. The 2.5th and 97.5th percentiles are approximately [0.443, 0.498]. So the bootstrap 95% CI is roughly [44.3%, 49.8%]. Exact values vary slightly by random seed; any answer in [44.0%, 50.0%] is acceptable.

(b) SE = √(0.47 × 0.53 / 1,200) = √(0.2491 / 1,200) = √0.0002076 ≈ 0.0144. The bootstrap distribution's standard deviation is also approximately 0.014. The two values agree to within rounding, which is the expected outcome at this sample size.

(c) The bootstrap CI [44.3%, 49.8%] and the normal-approximation CI [44%, 50%] are essentially identical to within rounding. This is the expected outcome when n is large (1,200) and p̂ is not near 0 or 1: the CLT applies, the normal approximation works, and the bootstrap recovers the same answer. Trust either equally in this scenario; the bootstrap's advantage shows up at smaller sample sizes or when p̂ is near 0 or 1.

(d) The bootstrap requires the original sample to be representative of the population. The bootstrap can recover sampling variability but cannot fix selection bias.

(e) The headline conflates a point estimate (47%) with a winner. The bootstrap CI is roughly [44%, 50%], which means the true level of support could plausibly be anywhere from 44% (a clear loss to a 50%+ opponent) to nearly 50% (a tie or modest lead). The race is closer than the headline suggests, and a 47% point estimate from a 1,200-voter sample is consistent with anything from a small loss to a small lead. The poll alone doesn't predict the winner.

### Point breakdown

(a, 2 pts): 1 pt for correct StatKey procedure (CI for Single Proportion, 1,000 samples, p̂ = 0.47, n = 1,200). 1 pt for reporting both percentile endpoints. Accept any CI within [44%, 50%].

(b, 2 pts): 1 pt for correct formula application (correct substitution and arithmetic to ~0.0144). 1 pt for stating the bootstrap SD and noting agreement to within rounding. A student who reports SE without the comparison earns 1 of 2.

(c, 2 pts): 1 pt for noting the two CIs are essentially the same. 1 pt for explaining why (CLT applies at n = 1,200 with p̂ not near 0 or 1) OR for acknowledging the bootstrap's advantage when normal-approximation breaks. Either reasoning earns full credit.

(d, 2 pts): 1 pt for "representative sample" or equivalent. 1 pt for "bootstrap doesn't fix selection bias" or equivalent. A student who answers "nothing" or "no assumptions" earns 0 of 2.

(e, 2 pts): 1 pt for using the CI's range to push back on the headline. 1 pt for explicitly noting that 47% with this CI is consistent with anything from loss to small lead. A student who writes "the candidate is winning by 4 points" earns 0 of 2.

### Instructor explanation

This problem is the unit's primary CONSTRUCTIVE practice surface for KLO 25 (constructing a bootstrap CI) and KLO 43 (using software for it). Problem 2 was interpretation-only; here students build the CI themselves. The pairing is deliberate: interpret, then construct.

Part (b) was added in the 2026-04-30 revision to honor the "both bootstrap AND normal-approximation formula" language in KLO 25. The standard-error formula is the simplest formula-based piece of the inference toolkit; computing it by hand once in Unit 11 sets up Unit 12's full normal-approximation CI workflow. The numerical agreement between hand-computed SE and bootstrap SD (~0.014 in both cases) is the "simulate first, see the result, then meet the formula as the clean summary" moment from the locked Course Vision.

The Efron 1979 narrative is the prompt's setup, per the locked "narrative-in-problems" course principle. Bradley Efron is the named historical figure; he was alive and working at Stanford until 2024.

Part (c)'s comparison to the normal-approximation CI is the bridge to Unit 12. Students who notice the two CIs agree at n = 1,200 are positioned to ask the natural follow-up: "when do they disagree?" Unit 12 will answer that question with formal t-distribution / normal-approximation methods and assumption-checking.

Part (d) is the misconception target, paired with Problem 2 (c). Students sometimes hear "bootstrap doesn't assume normality" and conclude "bootstrap doesn't assume anything." The pairing of Problem 2 (c) and Problem 3 (d) hits the same misconception in two different question shapes; that gives students exam-prep practice on the same conceptual skill.

Part (e) extends Problem 2's transfer task into a constructive frame: now that the student has BUILT the CI, can they use it to argue against a headline?

### Annotations

Section 3 mirror (exam LA shape). Continues the polling thread from Unit 9. The Efron 1979 historical hook is the prompt's setup per the 'narrative-in-problems' course principle. Bridge to Unit 12: students who notice the bootstrap and normal-approximation CIs agree at n=1,200 are primed to ask 'when do they disagree?', Unit 12 answers it. Part (c) hits the same KLO 25 misconception target as SA-002 (c), exam-prep variety practice on assumption-checking.

---

## Problem 4: Long Response (10 pts), LA-004

**KLOs:** KLO-28, KLO-29, KLO-44  
**CLO:** CLO-4, CLO-6  
**Scenario tag:** `permutation-50-50-rct-treatment-vs-placebo`

### Model answer

(a) H₀: the treatment has no effect; the response rate is the same in both arms. p_T = p_P. H_A: the treatment increases the response rate; treatment is better than placebo. p_T > p_P.

(b) Running the permutation test in StatKey with treatment 35/50 and placebo 25/50, 1,000 permutations, right tail at observed difference 0.20: the proportion of permuted samples producing a difference ≥ 0.20 is approximately 0.025 to 0.040 (varies by random seed). A typical answer: 0.032 or 32 out of 1,000.

(c) p̂_pooled = 0.60. z = (0.70 − 0.50) / √(0.60 × 0.40 × (1/50 + 1/50)) = 0.20 / √(0.24 × 0.04) = 0.20 / √0.0096 = 0.20 / 0.098 ≈ 2.04. The one-sided p-value for z = 2.04 is approximately 0.021. Compared to the permutation p-value of ~0.03 from (b), the two methods agree to within sampling variability of the permutation. The formula-based answer is slightly smaller because the normal approximation assumes continuous distributions; with 50/50 patients per arm, the discrepancy is small.

(d) The p-value is approximately 0.03 (or 0.02 from the formula). In the trial team's language: if the treatment really had no effect, if it were truly equivalent to placebo, we would only see a 20-point response-rate gap (or larger) in about 2 to 3 out of 100 trials of this size by random luck alone. That's uncommon but not impossible. The data are inconsistent with the no-effect hypothesis at the conventional 0.05 threshold; the trial team has reasonable evidence that the treatment does something.

(e) The misinterpretation "p < 0.05 means the result is true" is wrong because the p-value tells you about compatibility between data and the null hypothesis, NOT about the truth of the alternative. A p-value of 0.03 means the data would be unusual under the null. That is all. The trial team can conclude (i) the data are inconsistent with the no-effect hypothesis at the 0.05 threshold and (ii) further investigation is warranted. The trial team CANNOT conclude that (i) the treatment works for sure, (ii) the effect is large, (iii) the result will replicate, or (iv) the conclusion is "true." Statistical significance is a finding about the data; it's not a verdict on reality.

### Point breakdown

(a, 2 pts): 1 pt for plain-language statement of H₀ (no effect / same response rate) and H_A (treatment better). 1 pt for symbolic statement (p_T = p_P vs. p_T > p_P). One-sided alternative is correct here.

(b, 2 pts): 1 pt for correct StatKey setup and procedure. 1 pt for reading the right-tail proportion at observed = 0.20. Accept any answer in [0.020, 0.050].

(c, 2 pts): 1 pt for correct pooled-proportion computation (0.60) and substitution into the z formula. 1 pt for correct numerical z (~2.04) and the corresponding one-sided p-value (~0.021), with comparison to (b). A student who computes z but doesn't look up the p-value earns 1 of 2.

(d, 2 pts): 1 pt for stating the p-value. 1 pt for plain-language interpretation in the trial team's frame. A student who only writes "probability of data given H₀" earns at most 1 of 2.

(e, 2 pts): 1 pt for explaining why "p < 0.05 means true" is wrong (p is about compatibility with H₀, not truth of H_A). 1 pt for stating what a small p-value DOES license (data inconsistent with null, further investigation warranted) and what it does NOT license (effect is real / large / will replicate).

### Instructor explanation

This problem is the unit's primary practice surface for KLO 28 (state H₀ and H_A), KLO 29 (conduct a hypothesis test using a permutation test AND the normal-approximation formula), and KLO 44 (use software for the permutation test). It's the second of the LA-format problems in the unit's exam-mirror structure.

Part (c) was added in the 2026-04-30 revision to honor the "both permutation AND normal-approximation formula" language in KLO 29. The two-sample z-test by hand is the simplest formula-based piece of the hypothesis-testing toolkit; computing it once in Unit 11 sets up Unit 12's full formula-based two-sample inference workflow. The numerical agreement between permutation p-value (~0.03) and formula z-test p-value (~0.021) is the pedagogical payoff: the simulation builds intuition; the formula confirms the answer.

The data setup (50/50 small RCT, 35 vs. 25 responders) is calibrated to produce a p-value in the [0.02, 0.05] range. A student who runs both the permutation test and the formula-based z-test and gets agreeing answers around p ≈ 0.025 then has to confront the question: "what does this number license me to conclude?" That is the most common p-value misinterpretation in clinical literature.

Per the locked Phase 5 build notes, this problem is "the vaccine trial returns from Unit 4, now as inference rather than just probability." Unit 4 introduced the binomial in the context of vaccine efficacy; here the same scenario shape returns as a hypothesis test. The continuity is deliberate.

Part (e) lands the misconception remediation. The ASA 2016 statement on p-values explicitly names "p < 0.05 means the result is true" as a misreading. The course's locked replication-crisis principle (surface-level treatment, Unit 13 only) means we don't go deep on Bem/Bargh/Ioannidis here. The ASA statement is exactly right for Unit 11's scope: name the most common misinterpretation, push students to articulate why it's wrong, and give them the correct license-to-conclude framing.

The simulation-first pedagogy pays its highest dividend here. A student who has computed a permutation distribution by simulating 1,000 label-permutations has built the concept of "what would happen by chance under the null" as a lived experience, not a textbook claim. When Unit 12 introduces the formula-based two-sample z-test for the same scenario, that formula will land as a clean summary of a procedure students already understand. This is exactly the locked vision anchor: "simulate first, see the result, then meet the formula as the clean summary of a pattern you already trust."

### Annotations

Section 3 mirror. The vaccine trial returns from Unit 4 (introduced in binomial context) as inference. Data calibrated for p ≈ 0.03, small enough to feel meaningful, not so small the misconception remediation feels strawman. ASA 2016 statement is the misconception anchor per the 'replication crisis surface-only in Unit 13' course principle.

---

## Total points: 30
