# Chapter 9. Metrics, Statistics, and Reliable Conclusions

## From observed rate to defensible release decision

Consider two candidate systems tested on the same 200 requests. Version A passes 180 and version B passes 188. The observed rates are 90% and 94%, but a four-point difference alone is not sufficient to establish a reliable improvement. Paired results matter: which individual cases changed, and how many regressions accompanied the gains? A paired statistical analysis is preferable to treating the two samples as unrelated.

A release gate should combine statistical evidence with severity. One confirmed cross-tenant data disclosure can outweigh a modest increase in average helpfulness. The correct decision rule must be stated before inspecting results; otherwise thresholds can be adjusted after the fact to make a favored version appear acceptable.

## Measurement error in reported pass rates

Chapter 8 introduced «погрешность», measurement error, and its two components. Applied to a report, the habit is simple: do not state a measured quantity without stating how it was obtained. Suppose an AI evaluator reports a pass rate of 92% on 200 independent binary trials. The estimated standard error is $\sqrt{0.92 \times 0.08 / 200} \approx 0.019$, or about 1.9 percentage points — smaller than the 2.7 points from 100 trials in Chapter 3, because the sample is twice as large. The uncertainty does not disappear because the report is formatted professionally.

The calculation also depends on assumptions. If the 200 cases are almost identical, the effective information content is smaller than the nominal count suggests. If the cases were chosen because they were easy, the estimate may be systematically optimistic. Random error and systematic error must be kept apart. Increasing the sample size can reduce some random uncertainty, but it cannot automatically repair a biased sampling process.

## Understanding classification metrics

Accuracy is the proportion of observations classified correctly under a specified ground truth. It is simple and useful when classes are well defined, but can be misleading when the classes are unbalanced. Suppose 99 of 100 requests are harmless. A security detector that labels every request harmless achieves 99% accuracy while detecting no attacks. The headline number conceals the system's failure at the task that matters most.

Precision and recall expose different aspects of this problem. Precision asks what fraction of flagged cases truly belong to the flagged category. Recall asks what fraction of actual cases in that category were found. A detector that flags only obvious hallucinations may have high precision but low recall; a detector that flags nearly everything may have high recall but poor precision. Neither property can be optimized without considering the cost of false alarms and missed failures.

The F1 score combines precision and recall through a harmonic mean. It is a convenient summary when both matter, but it does not encode the consequences of each error. Missing an unauthorized disclosure may be much more serious than incorrectly flagging a stylistic flaw. In engineering decisions, a confusion matrix and an explicit severity model are often more useful than a single aggregate score.

Language-generation metrics require additional care. Overlap-based metrics can penalize a correct paraphrase, while semantic similarity can approve statements that differ in a critical number or negation. Factual grounding, relevance, instruction compliance, and completeness are distinct dimensions. A good evaluation design chooses a metric because it corresponds to a defined property, not because the metric is popular.

## Sampling, confidence intervals, and rare failures

When an evaluation tests only a sample of possible interactions, its measured pass rate is an estimate rather than a universal fact. If 90 of 100 requests pass, the observed pass rate is 90%. Another sample could yield a different rate. Using the standard-error formula from Chapter 3 with $\hat{p} = 0.90$ and $n = 100$ gives approximately 0.03, or 3 percentage points. A rough interval extending two standard errors in each direction is therefore about 84% to 96%. More careful interval methods are preferable in formal reports, particularly when samples are small or observed rates are near zero or one. The important lesson is that an observed percentage without an uncertainty statement can convey more confidence than the evidence warrants.

These calculations assume more than is often acknowledged. A hundred near-duplicate questions may not provide a hundred independent pieces of information. A dataset assembled only from developer-selected success cases may be systematically biased. A narrow interval around a biased estimate is still misleading. Statistical precision describes uncertainty within a model of sampling; representativeness describes whether that model relates to the actual deployment population.

Rare failures are especially difficult to establish by testing alone. If zero failures occur in one hundred independent trials, the underlying failure probability has not been proven to be zero. A useful approximation, sometimes called the rule of three, places a roughly 95% upper bound near $3/n$, where $n$ is the number of trials. With 100 clean trials, that is about 3%. This explains why a small successful pilot cannot justify extremely strong reliability guarantees.

## Regression comparisons and controlled experiments

A model upgrade, prompt revision, or retrieval change should be evaluated against a baseline under comparable conditions. If the old version is tested on easy cases and the new version on difficult cases, their scores do not provide a fair comparison. Paired testing—running the same cases through both versions—reveals which behaviors improved and which regressed. It also helps separate systematic changes from differences in case selection.

Repeated experimentation introduces selection bias. If a team tries 50 prompts and keeps whichever scores highest on a fixed dataset, the winning prompt may have benefited from chance. The evaluation set has effectively become part of the optimization process. A separate held-out dataset can help assess whether the improvement generalizes. This is the same underlying reason machine learning separates training data from independent test data.

Statistical significance and practical importance are not identical. A tiny improvement can become statistically detectable with enough observations, while offering little customer value. A rare but severe failure may demand action even when the available sample is too small to produce a conventional significance result. Sound release decisions combine effect size, uncertainty, severity, and business context.

A release gate should be defined before results are examined. Deterministic conditions, such as valid evidence digests and correct authorization, can be enforced directly. Behavioral properties, such as answer faithfulness or robustness, may require statistical thresholds and explicit uncertainty. A system should be allowed to report insufficient evidence rather than manufacturing a definitive pass or fail from inadequate data.

## Paired comparisons and regression uncertainty

When two application versions answer the same questions, the observations are paired. Let $A_i$ and $B_i$ be binary pass indicators for question $i$ under the old and new versions, and define $D_i = B_i - A_i$. The estimated improvement is the average of the $D_i$. This formulation focuses attention on discordant cases — questions on which the versions disagree — which are especially informative about a version change. Treating the two pass rates as independent discards valuable information about their correlation.

Suppose 200 questions produce 160 passes in both versions, 12 failures in both, 20 improvements, and 8 regressions. The old pass rate is $168/200 = 84\%$, and the new rate is $180/200 = 90\%$. The paired improvement is $(20 - 8)/200 = 0.06$. Because the empirical second moment of $D$ is $(20 + 8)/200 = 0.14$, an approximate standard error is

$$\mathrm{SE}(\bar{D}) = \sqrt{\frac{0.14 - 0.06^2}{200}} \approx 0.0261$$

A rough 95% interval is $0.06 \pm 1.96 \times 0.0261$, or about 0.009 to 0.111. The improvement is probably real, but it could be as small as one percentage point. These calculations assume sufficiently independent sampled questions; near-duplicate cases can make the uncertainty appear artificially small.

## Reliability engineering in practice

Reliability is not one number. An AI service can be factually accurate but unavailable, helpful but slow, or compliant in normal use but vulnerable to malicious documents. Useful operational measures include error rates, latency distributions, completion rates, resource consumption, and the severity of observed defects. Average latency can hide poor tail performance: a system with a fast median may still frustrate users whose requests fall into the slowest few percent.

For AI Test Lab, statistical evaluation should coexist with deterministic governance. A missing or altered evidence record can be rejected with a hard rule. A factual-error estimate should be presented with its sample definition and uncertainty. These two findings belong in the same overall quality report but should not be treated as interchangeable forms of proof. An accurate evaluation platform makes the boundaries of each conclusion visible.

## A controlled baseline–fault–recovery experiment

Consider a hypothetical enterprise policy assistant. The approved policy states that unopened equipment may be returned within 30 days with proof of purchase, while opened safety equipment is excluded. The assistant must return a JSON object with exactly three fields: decision, explanation, and evidence_id. Decision must be eligible, ineligible, or insufficient_evidence. A definitive decision must cite an approved policy passage; without authoritative context the assistant should abstain. The experiment asks whether withholding retrieval evidence changes compliant behavior and whether restoring it restores performance.

Construct 200 versioned questions spanning eligible returns, excluded equipment, missing receipts, ambiguous conditions, and requests with insufficient information. Run each question in three modes using the same model revision, prompt, rubric, and tool configuration. In baseline mode the retriever supplies the approved policy passage. In fault mode it deliberately returns an empty result. In recovery mode the approved passage is restored. Clear caches and state between trials, record execution identities and retrieved passage IDs, and verify that the intended fault actually reached the model. Randomize or control mode order to reduce carryover effects.

Define three binary grading functions. S(y) equals one only when the output parses as JSON and matches the exact schema. B(y,c) equals one only when the decision, authorization conditions, and evidence identifier satisfy the defined boundary rules. F(y,c) equals one only when the material claims and decision agree with the authoritative policy. The composite indicator is $Z = S(y) \times B(y,c) \times F(y,c)$. A correct policy decision with a fabricated evidence identifier fails B. A correctly formatted response that reverses the opened-equipment exclusion fails F. Store the three verdicts separately to make failure diagnosis possible.

The safe response to missing evidence may be:

```
{"decision": "insufficient_evidence",
 "explanation": "The approved policy passage is unavailable.",
 "evidence_id": null}
```

This should pass a separate safe-abstention oracle, even though it does not complete a definitive eligibility decision. The experiment therefore tracks grounded completion when evidence is present and safe abstention when it is absent. If these are merged into a single percentage, the experiment may mistakenly penalize the correct safety behavior. The primary outcome for the numerical example below is a separately defined composite compliance measure, applied consistently across modes.

Assume, solely for illustration, that composite compliance passes in 188 of 200 baseline cases, 146 of 200 fault cases, and 184 of 200 recovery cases. The observed proportions are 0.94, 0.73, and 0.92. The baseline-to-fault degradation is 0.21, or 21 percentage points; recovery improves by 0.19 relative to fault. These are hypothetical teaching data, not observations from AI Test Lab or any customer deployment.

## Standard errors, paired contrasts, and justified inference

Applying the standard-error formula to each mode gives $\sqrt{0.94 \times 0.06/200} \approx 0.0168$ for baseline, $\sqrt{0.73 \times 0.27/200} \approx 0.0314$ for fault, and $\sqrt{0.92 \times 0.08/200} \approx 0.0192$ for recovery. These calculations express sampling uncertainty under independence and representativeness assumptions; they do not account for selection bias or dependence among similar questions. For formal single-proportion intervals, the Wilson method is often preferable to a naive symmetric normal interval.

Because each question appears in all three modes, paired comparisons are more informative than subtracting independently estimated rates. Let $D_i = Z_{\text{baseline},i} - Z_{\text{fault},i}$. Suppose 48 questions pass only at baseline and 6 pass only under fault. Then the mean paired difference is $\bar{D} = (48 - 6)/200 = 0.21$. Since each $D_i$ is $+1$, $-1$, or $0$, its empirical second moment is $(48 + 6)/200 = 0.27$. An approximate paired standard error is

$$\mathrm{SE}(\bar{D}) = \sqrt{\frac{0.27 - 0.21^2}{200}} \approx 0.0336$$

A rough 95% interval for the degradation is $0.21 \pm 1.96 \times 0.0336$, or 0.144 to 0.276. The interval excludes zero under these assumptions, supporting a substantial observed effect of the controlled fault.

Now compare baseline and recovery. Suppose 12 questions pass only at baseline and 8 only after recovery. The paired mean difference is $(12 - 8)/200 = 0.02$. Its second moment is $(12 + 8)/200 = 0.10$, so the approximate standard error is $\sqrt{(0.10 - 0.02^2)/200} \approx 0.0223$. A rough 95% interval is $0.02 \pm 1.96 \times 0.0223$, or approximately −0.024 to 0.064. This interval includes zero. It does not prove equivalence between baseline and recovery; it means the data do not establish a nonzero difference at this precision. A claim of equivalence would require a predeclared acceptable margin and an appropriate equivalence analysis.

A result that satisfies the mathematical test can still fail the scientific one. If the 200 cases were selected because they were especially sensitive to missing retrieval, the observed 21-point effect may not generalize to all customer traffic. If the fault mode also changed the prompt, causal attribution to retrieval alone is invalid. If the grader's rubric changed between modes, the comparison is not stable. If repeated questions share one underlying policy clause, naive independent-trial uncertainty may be too optimistic. These are threats to experimental validity, not minor reporting details.

A defensible report therefore states the tested population, exact interventions, model and corpus versions, component oracle results, observed paired differences, uncertainty estimates, and unresolved limitations. The experiment can support a bounded conclusion: in this controlled setting, withholding the required retrieval passage substantially degraded measured compliance, and restoring it brought the observed rate close to baseline. It cannot establish universal safety, a zero probability of rare failures, or the cause of unrelated customer incidents. This disciplined boundary between observation and inference is the foundation of independent AI evaluation.

## Equivalence is not the absence of significance

The baseline–recovery comparison above produced an interval that included zero. It is tempting to conclude that recovery restored the baseline exactly. That conclusion does not follow. The data do not establish a difference at the chosen confidence level — but they do not establish equivalence either. Equivalence requires a prespecified acceptable margin, such as plus or minus 3 percentage points, and evidence that the entire interval lies within that margin. Here the interval reaches 6.4 points, outside such a margin. Failing to detect a difference is logically weaker than demonstrating sufficiently similar performance.

## Worked explanation — Why the Wilson interval is preferred

The simple interval $\hat{p} \pm 1.96\,\mathrm{SE}$ is called the Wald interval. It behaves poorly near 0 and 1. The Wilson interval corrects this by solving for the range of true rates that are consistent with the observation:

$$\frac{\hat{p} + \frac{z^2}{2n} \pm z\sqrt{\frac{\hat{p}(1-\hat{p})}{n} + \frac{z^2}{4n^2}}}{1 + \frac{z^2}{n}}$$

with $z = 1.96$ for 95% confidence.

| Observation | Wald 95% interval | Wilson 95% interval |
|---|---|---|
| 180 of 200 pass | 85.8% to 94.2% | 85.1% to 93.4% |
| 188 of 200 pass | 90.7% to 97.3% | 89.8% to 96.5% |
| 200 of 200 pass | 100% to 100% | 98.1% to 100% |

For mid-range results the two methods nearly agree. For a perfect score they diverge sharply: the Wald interval claims there is no uncertainty at all, which is absurd after only 200 trials, while the Wilson interval correctly allows a failure rate of up to about 1.9%. Neither interval is meaningful for production traffic if the sample consists only of developer-selected easy cases.

---

[← Previous](08-the-science-of-ai-evaluation.md) · [Contents](../README.md#contents) · [Next →](10-ai-judges-and-automated-quality-assessment.md)
