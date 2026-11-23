# Appendix A. Mathematical Reference

## Mathematics makes evaluation claims auditable

This appendix collects the formulas used in the book, with a short statement of what each one measures and what it does not. A mathematical formula should be read as a sentence about a system. The quantities it names, and its denominators in particular, show which errors a measurement counts and which it ignores. The Russian term «вывод» can mean both a derivation and a conclusion. For every important equation, seek both: follow the derivation when appropriate, and state the engineering conclusion in ordinary language.

## Learning and prediction

Mean squared error for $n$ predictions $\hat{y}_i$ of observed values $y_i$ (Chapter 2):

$$\mathrm{MSE} = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2$$

The logistic function, which converts a score $z$ into a probability (Chapter 2):

$$p = \frac{1}{1 + e^{-z}}$$

Cross-entropy loss over $N$ examples, where $P_\theta(y_i \mid x_i)$ is the probability the model with parameters $\theta$ assigns to the correct output (Chapters 2 and 4):

$$L = -\frac{1}{N}\sum_{i=1}^{N}\log P_\theta(y_i \mid x_i)$$

For a single example with correct-class probability $p$, the contribution is $-\ln p$: about 0.105 at $p = 0.9$, 0.223 at $p = 0.8$, and 2.303 at $p = 0.1$. The loss penalizes confident wrong predictions heavily. It measures predictive fit, not truth or safety.

Perplexity, when natural logarithms are used:

$$\mathrm{PPL} = e^{L}$$

The gradient descent update, with learning rate $\eta$ (Chapter 4):

$$\theta_{t+1} = \theta_t - \eta\,\nabla_\theta L$$

## Probabilities over tokens

Softmax, which converts scores $z_i$ into a probability distribution (Chapter 5):

$$p_i = \frac{e^{z_i}}{\sum_j e^{z_j}}$$

The values are nonnegative and sum to one. They describe a distribution within the model and its context; they are not calibrated statements about factual truth.

Softmax with temperature $T$ (Chapter 6):

$$p_i = \frac{e^{z_i/T}}{\sum_j e^{z_j/T}}$$

Lower temperature concentrates probability; higher temperature spreads it. Temperature controls diversity, not correctness.

Scaled dot-product attention (Chapter 5):

$$\mathrm{Attention}(Q,K,V) = \mathrm{softmax}\!\left(\frac{QK^{\top}}{\sqrt{d_k}}\right)V$$

Read as a sentence: compare each query with every key, scale the scores by the square root of the key dimension, normalize them into weights, and use the weights to combine the values. It produces weighted representations, not an independent test of truth.

## Similarity

Cosine similarity between vectors $a$ and $b$ (Chapters 8 and 12):

$$\cos(a,b) = \frac{a \cdot b}{\lVert a \rVert\,\lVert b \rVert}$$

For $a = (1, 0)$ and $b = (0, 1)$ it is 0; for $a = (1, 0)$ and $b = (0.6, 0.8)$ it is 0.6; for identical directions it is 1. High similarity is useful for retrieval ranking but cannot establish that two statements agree about numbers, dates, or negation. Similarity describes geometry, not entailment.

## Classification metrics

With true positives TP, false positives FP, and false negatives FN (Chapter 9):

$$\text{Precision} = \frac{TP}{TP + FP} \qquad \text{Recall} = \frac{TP}{TP + FN} \qquad F_1 = \frac{2PR}{P + R}$$

Precision ignores missed cases; recall ignores false alarms. Which matters more depends on the cost of each error.

Bayes' rule for the credibility of an alert (Chapter 3):

$$P(\text{defect} \mid \text{alert}) = \frac{P(\text{alert} \mid \text{defect})\,P(\text{defect})}{P(\text{alert})}$$

## Estimating rates and their uncertainty

Observed pass rate for $k$ passes in $n$ independent trials, and its approximate standard error (Chapters 3 and 9):

$$\hat{p} = \frac{k}{n} \qquad \mathrm{SE} = \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$

If 90 of 100 cases pass, the standard error is 0.03 and a rough two-standard-error interval is 84% to 96%. The formula assumes independent, representative trials. It does not repair selection bias or dependence between cases.

The Wilson interval, preferred for formal reports, with $z = 1.96$ for 95% confidence (Chapter 9):

$$\frac{\hat{p} + \frac{z^2}{2n} \pm z\sqrt{\frac{\hat{p}(1-\hat{p})}{n} + \frac{z^2}{4n^2}}}{1 + \frac{z^2}{n}}$$

Paired difference between two versions run on the same $n$ cases, where $D_i$ is $+1$, $-1$, or $0$ for each case (Chapter 9):

$$\bar{D} = \frac{1}{n}\sum_i D_i \qquad \mathrm{SE}(\bar{D}) = \sqrt{\frac{\overline{D^2} - \bar{D}^2}{n}}$$

## Rare failures

Probability of observing at least one failure in $n$ independent trials with per-trial failure probability $p$ (Chapter 8):

$$1 - (1 - p)^n$$

The rule of three: after zero failures in $n$ independent trials, an approximate 95% upper bound on the failure rate is $3/n$ (Chapters 8 and 9).

Success of a task requiring $k$ independent steps, each succeeding with probability $s$ (Chapter 20):

$$P(\text{task succeeds}) = s^{k}$$

## Performance and cost

Generation time for $N$ output tokens (Chapter 15):

$$T \approx \mathrm{TTFT} + (N - 1) \times \mathrm{TPOT}$$

Lower bound on weight memory for $P$ parameters at $b$ bits each (Chapter 15):

$$M \approx \frac{P \times b}{8} \text{ bytes}$$

Model calls for an evaluation suite of $n$ cases, $r$ repetitions, and $j$ judge calls per trial (Chapter 15):

$$C = n \times r \times (1 + j)$$

Return on investment (Chapter 19):

$$\mathrm{ROI} = \frac{\text{benefit} - \text{cost}}{\text{cost}}$$

---

[← Previous](21-synthesis-becoming-an-ai-engineering-specialist.md) · [Contents](../README.md#contents) · [Next →](references.md)
