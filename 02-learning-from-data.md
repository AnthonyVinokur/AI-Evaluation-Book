# Chapter 2. Learning from Data

## Generalization is the reason evaluation exists

Training error tells us how well a learner fits the observations used to develop it. The central question, however, is how it performs on new observations. This is generalization, or «обобщение». A classifier that memorizes its training set can appear perfect while failing on unfamiliar cases. Independent evaluation data is necessary to distinguish genuine learning from memorization.

For example, suppose a classifier makes 2 errors among 100 training examples but 18 errors among 100 independent test examples. The observed error rates are 2% and 18%. The difference does not by itself diagnose the cause, but it warns that performance on development data is not a reliable estimate of deployment performance. An AI evaluation suite must preserve this separation when comparing prompts, models, and application versions.

## From a mathematical problem to a learned rule

A useful distinction in the Russian mathematical classroom is between «задача» (the problem to be solved) and «метод» (the method chosen to solve it). Machine learning begins with a precisely stated task and a measurable criterion of success. The learning algorithm is the method; the trained model is its result. Confusing these levels leads to a common error: assuming that a sophisticated algorithm guarantees a well-posed problem.

Consider fitting a straight line to observed pairs of numbers. We seek parameters $a$ and $b$ so that the prediction $\hat{y} = ax + b$ is close to the observed value $y$. With $n$ examples, a common objective is mean squared error:

$$\mathrm{MSE} = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y}_i)^2$$

This equation does not say the line is the truth. It says we have chosen a family of possible functions and a rule for penalizing disagreement. The fitted line can still fail outside the range of the training examples. In Russian terminology, this is the problem of «обобщение»—generalization beyond the sample.

The habit worth developing is to ask three questions of every learning system: what is being optimized, on which observations, and under which assumptions will the result generalize? Those questions remain useful when the model has billions of parameters rather than two.

## The learning objective

To learn from data, a model needs a family of possible functions, examples, and a way to measure improvement. Imagine predicting a used car's price from its age and mileage. A simple model combines the inputs with learned coefficients. Training adjusts those coefficients to reduce the difference between predicted and observed prices. A neural network can express much more complex relationships, but the core pattern remains: predict, measure error, and adjust.

The loss function expresses error numerically. Mean squared error strongly penalizes large numerical mistakes. Cross-entropy loss is commonly used when the model assigns probabilities to categories or tokens. The choice of objective matters because a model can optimize its loss while failing to satisfy the real business objective. A fraud detector with excellent average accuracy can still miss rare, expensive fraud cases.

Gradient descent is a family of optimization methods that changes parameters in a direction expected to reduce loss. Training proceeds over batches, often for many iterations. The result depends on the data, the model architecture, initialization, and learning settings. Successful optimization does not guarantee that the model has learned the intended relationship.

## Generalization and the unseen world

Performance on examples used for training is not sufficient evidence of useful learning. A model may memorize incidental details that do not recur in deployment. For this reason, practitioners distinguish training data, validation data used for model selection, and a held-out test set used for final assessment. This separation protects the credibility of the final estimate.

The separation can erode when developers repeatedly inspect test results and adapt the model or prompts to improve them. The test set then influences development even if its examples never update model weights. In foundation-model evaluation, public benchmarks may also appear in web-scale training data. A high score is most informative when the test is genuinely independent and representative of the intended task.

Distribution shift occurs when the real world differs from the conditions represented in development data. Policies change, users phrase requests differently, and new document formats appear. A model that was reliable last month may behave differently after an application or data update. Evaluation must therefore be repeated, and the versions of data, prompts, models, and tools must be recorded.

## Uncertainty in evaluation

Every evaluation score is computed on a sample, so it is an estimate rather than a fact about all future requests. A different representative sample could yield a different result, even if each individual test is deterministic, and probabilistic generation adds a further source of variability. Chapter 3 quantifies this uncertainty; here the point is simply that a score without a description of its sample is incomplete.

A release decision should therefore consider confidence, the representativeness of examples, and the severity of failures. A single dangerous unauthorized action may matter more than dozens of harmless wording differences. Statistical intervals and paired comparisons can help determine whether observed changes are meaningful, but they cannot compensate for poorly chosen evaluation cases.

This provides a second useful distinction for AI Test Lab. An integrity-protected result can establish what was observed in a controlled run; the validity of broader reliability claims depends on the experimental design. Good governance preserves evidence, while good evaluation science determines what that evidence means.

## Worked explanation — A logistic regression calculation

A logistic model computes $p = 1/(1+e^{-z})$ with $z = wx + b$. Let $w = 2$, $x = 2$, and $b = -3$. Then $z = 1$ and $p \approx 0.731$. If the correct label is 1, the binary cross-entropy loss is $-\ln(0.731) \approx 0.313$. A model assigning probability 0.1 to the same label incurs $-\ln(0.1) \approx 2.303$. Optimization rewards better predictions, but the observed probability still requires calibration.

---

[← Previous](01-the-nature-of-artificial-intelligence.md) · [Contents](../README.md#contents) · [Next →](03-probability-statistics-and-uncertainty.md)
