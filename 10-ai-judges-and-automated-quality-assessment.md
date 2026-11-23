# Chapter 10. AI Judges and Automated Quality Assessment

## The evaluator must itself be evaluated

An AI judge can mistake eloquence for correctness. Suppose it approves 90 of 100 genuinely supported answers but also approves 20 of 40 unsupported answers. Its sensitivity to supported answers is high, yet its ability to reject unsupported claims is inadequate for a high-stakes factuality gate. The confusion matrix is more informative than a single agreement percentage.

Judge calibration requires independently adjudicated examples, stable rubrics, and revalidation after model or prompt changes. Where evidence is incomplete, an "inconclusive" verdict is often more scientifically defensible than a forced binary judgment.

## Why a model can judge another model

Human evaluation is expensive when applications generate thousands of distinct answers. A second model can review an answer against instructions, a rubric, and supporting evidence, then produce a structured assessment. This approach is commonly called AI-as-a-judge. Its advantage is scalability: it can assist with repetitive reviews that would otherwise consume substantial expert time. Its limitation is that the judge is itself a fallible AI system.

A judge may favor fluent prose over correct facts, overlook a negation, invent an explanation, or interpret an ambiguous rubric inconsistently. In pairwise comparisons it may prefer the first or second answer because of presentation order. It may favor longer answers even when the extra content contains unsupported claims. It may also favor writing styles similar to its own. None of these weaknesses disappear merely because the judge produces a confident numerical score.

Different judging formats ask different questions. Pointwise evaluation scores one answer against a rubric. Pairwise evaluation asks which of two answers better meets the same criteria. Claim-level evaluation decomposes an answer into propositions and examines whether each is supported. Pairwise comparison can be easier than absolute scoring; claim-level checks can expose the precise source of a factual problem. The right choice depends on the property being measured.

## Rubrics, decomposition, and evidence

A useful rubric replaces vague adjectives with observable criteria. "Good answer" is too broad. A more precise rubric might ask whether the answer addresses the request, whether its material claims are supported by the provided source, whether missing information is acknowledged, and whether explicit constraints are obeyed. Each criterion should have a defined meaning, including how to handle partial success and conflicting evidence.

An answer can be relevant but ungrounded, accurate but incomplete, or polite but unauthorized. A multidimensional verdict preserves these differences. Where a single operational decision is required, the aggregation rule should be explicit. For example, a confirmed unauthorized disclosure may override a high helpfulness score. This prevents strong performance in one dimension from concealing a critical failure in another.

The evidence supplied to the judge is not necessarily trustworthy. Retrieved pages and tool outputs can contain misleading statements or embedded instructions intended to redirect the evaluator. The judge must treat such material as evidence to analyze, not authority to obey. Strong boundaries require application-level controls and adversarial testing in addition to carefully written instructions.

A judge should be able to abstain. If a policy excerpt does not mention the requested refund deadline, the judge cannot establish that an asserted deadline is true merely by finding the sentence plausible. "Unsupported by the available evidence" is different from "proven false." Maintaining this distinction improves the usefulness of diagnostic reports and avoids overstating the evaluator's knowledge.

## Calibration and agreement

Before using an automated judge in a release gate, engineers need a calibration set with independently reviewed outcomes. The set should include obvious successes, obvious failures, borderline cases, and adversarial examples. Comparing the judge's decisions with reviewed labels reveals its error patterns. A confusion matrix can show whether the judge is prone to approving unsupported claims or rejecting valid paraphrases.

Human agreement is also imperfect. Reviewers may interpret ambiguous rules differently, and a majority vote does not automatically establish factual truth. Disagreements should be adjudicated against authoritative evidence where possible. Calibration is not a one-time ceremony: changes to the judge model, its instructions, the rubric, or the supporting documents can change the meaning of its scores.

Multiple judges can provide useful independent signals, but agreement between models is not proof of correctness. Judges may share training data, biases, or reasoning errors. Their decisions can be correlated even when they are implemented separately. A reliable evaluation system therefore uses model judgments as evidence to be calibrated, not as unquestionable ground truth.

## Building a trustworthy judging workflow

A practical judging workflow starts by defining the property to assess and the authoritative evidence available. It then prepares a stable rubric, records the judge configuration, runs the assessment, and preserves both the verdict and the supporting rationale. Borderline or high-severity cases can be routed to human review. The goal is not to eliminate judgment, but to make the process systematic, inspectable, and proportionate to risk.

Judge quality must be monitored just like application quality. A judge that performs well on short policy questions may struggle with long documents, multilingual content, or complex agent trajectories. Its calibration set should reflect the actual cases it will assess. When the judge is updated, prior scores may cease to be directly comparable; versioning and re-evaluation are therefore essential.

For AI Test Lab, AI judges are best understood as one component in a layered evaluation architecture. Deterministic checks verify schema, authorization, and evidence integrity. Evidence-grounded checks examine factual claims. Calibrated model judges assess properties that resist exact comparison. Human adjudication resolves consequential uncertainty. The platform becomes credible not by claiming that its judges are always correct, but by showing how their limitations are measured and controlled.

## Worked explanation — Calibrating a model judge

A judge must be calibrated against independently adjudicated cases. Suppose 40 of 100 answers contain unsupported claims. The judge flags 34 of those 40, misses 6, and incorrectly flags 9 of the 60 supported answers. Treating "unsupported" as the positive class:

$$\text{Precision} = \frac{34}{34 + 9} \approx 79.1\% \qquad \text{Recall} = \frac{34}{40} = 85\%$$

These numbers expose two operational costs. About one alert in five sends a reviewer to investigate an answer that was actually supported, and 15% of unsupported claims pass unnoticed. A single overall agreement figure would hide this distinction: the judge agrees with the reviewers on 85 of 100 cases, which sounds reassuring until the two kinds of error are separated. A fluent rationale from the judge does not improve these measured error rates, and any change to the judge model, prompt, or rubric requires renewed calibration.

---

[← Previous](09-metrics-statistics-and-reliable-conclusions.md) · [Contents](../README.md#contents) · [Next →](11-hallucinations-robustness-and-behavioral-testing.md)
