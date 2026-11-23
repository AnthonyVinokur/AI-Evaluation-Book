# Chapter 3. Probability, Statistics, and Uncertainty

## Uncertainty is not an optional footnote

A test result is a measurement with uncertainty («погрешность»). If 92 of 100 independently sampled requests satisfy a requirement, the observed pass rate is $\hat{p} = 92/100 = 0.92$. A rough standard error is

$$\mathrm{SE} = \sqrt{\frac{\hat{p}(1-\hat{p})}{n}} = \sqrt{\frac{0.92 \times 0.08}{100}} \approx 0.027$$

or about 2.7 percentage points. The result is not proof that exactly 92% of future requests will pass.

The computation assumes independent, representative trials. If the 100 questions are minor paraphrases of five underlying cases, the effective information is smaller. Good evaluation reports identify the sampling population and the dependence structure, and avoid presenting precision unsupported by the experimental design.

## Why probability matters

A conventional program may apply exact rules to uncertain information. An AI model goes further: it learns relationships from observations and estimates what may happen when presented with new information. A model that assigns an 80% probability to a customer cancellation does not know the future. It expresses an estimate conditional on its training data, inputs, and assumptions. The distinction matters because an apparently precise score can conceal incomplete evidence, biased sampling, or a changing environment.

Two kinds of uncertainty are especially useful to distinguish. Aleatoric uncertainty describes variation that may remain even if the underlying process is well understood. Epistemic uncertainty reflects gaps in what the model has learned. More representative evidence can sometimes reduce epistemic uncertainty; it cannot eliminate genuine randomness. A hosted foundation model rarely provides enough internal information to separate these categories reliably, which makes direct evaluation of observed behavior essential.

## Base rates and false alarms

Imagine a detector that identifies 90% of genuinely unsafe responses but incorrectly flags 5% of safe responses. If unsafe responses account for 1% of traffic, then among 10,000 responses approximately 100 are unsafe. The detector catches 90 of them but also flags 495 of the 9,900 safe responses. Only 90 of 585 alerts are genuine — about 15%. This is the base-rate problem: a seemingly accurate detector can be dominated by false positives when the condition is rare.

A commercial evaluation system must therefore report more than accuracy. Precision asks what proportion of alerts are genuine; recall asks what proportion of genuine problems were found. Thresholds change the balance between missed defects and unnecessary escalations. The appropriate balance depends on the cost of each mistake, which differs dramatically between a harmless formatting defect and an unauthorized financial transaction.

## Sampling and confidence

The standard error computed at the start of this chapter feeds directly into a confidence interval: a range of values consistent with the observation under stated sampling assumptions. A confidence interval expresses sampling uncertainty; it does not compensate for a test set composed only of easy examples.

Repeated paraphrases of one question are not necessarily independent observations. Hundreds of similar prompts may create an illusion of precision while leaving entire classes of failures unexplored. Reliable evaluation combines representative sampling with deliberately chosen boundary, adversarial, and high-consequence cases. In AI Test Lab, a tamper-evident result can establish record integrity; it cannot by itself establish that the sample or metric was scientifically valid.

## Worked explanation — How the base rate changes alert credibility

Bayes' rule makes the base-rate effect precise:

$$P(\text{defect} \mid \text{alert}) = \frac{P(\text{alert} \mid \text{defect})\,P(\text{defect})}{P(\text{alert})}$$

Keep the same detector — 90% sensitivity and a 5% false-alarm rate — and compare two deployments.

| Defect rate | Defects caught (per 10,000) | False alarms | Share of alerts that are genuine |
|---|---|---|---|
| 1% | 90 | 495 | 90 / 585 ≈ 15.4% |
| 10% | 900 | 450 | 900 / 1,350 ≈ 66.7% |

The detector has not changed, yet the credibility of an alert rises more than fourfold. Detector sensitivity and alert credibility are not interchangeable, and a precision figure measured in one deployment does not transfer to another with a different defect rate.

---

[← Previous](02-learning-from-data.md) · [Contents](../README.md#contents) · [Next →](04-neural-networks-and-deep-learning.md)
