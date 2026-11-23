# Chapter 4. Neural Networks and Deep Learning

## A low loss is not a guarantee of safe behavior

Neural networks optimize a mathematical objective such as cross-entropy. During training, the gradient indicates how parameters should change to reduce that objective on observed examples. Yet a small average loss does not establish correctness on every rare or consequential case. Optimization is about fitting a chosen objective; evaluation is about deciding whether that objective and the resulting behavior satisfy real requirements.

This distinction is especially important when a model is embedded in a workflow. The neural network may predict likely text successfully while the application violates an authorization rule. The authorization requirement needs its own deterministic test; it cannot be inferred from a good language-model training score.

## A historical connection: Kolmogorov and approximation

Russian and Soviet mathematical traditions contributed important foundations to the study of functions, probability, and computation. Andrey Kolmogorov is especially relevant to the broader intellectual history of approximation and probability. His 1957 representation theorem, later refined by Vladimir Arnold and now known as the Kolmogorov–Arnold representation theorem, shows that any continuous function of several variables can be written using sums and compositions of continuous functions of one variable. It is sometimes mentioned in discussions of neural-network expressiveness. The historical result should not be confused with a proof that a practical neural network will train successfully or generalize reliably.

This distinction is instructive. A theorem may establish that a function can be represented under specified conditions, while engineering asks whether a finite model can learn a useful approximation from limited data with limited computation. Existence, efficient construction, and reliable generalization are three different questions. Neural networks force us to keep them separate.

## Learning representations

A linear model combines inputs with learned coefficients. It can be useful and interpretable, but its basic form represents a limited class of relationships. Neural networks compose many transformations: each layer combines numerical inputs, applies a nonlinear function, and passes a representation to the next layer. Without nonlinearity, stacking linear layers would still produce a linear model. Nonlinear transformations make it possible to approximate complex relationships among features.

Artificial neurons are mathematical operations, not literal copies of biological neurons. A deep network learns distributed representations across many parameters. Early stages may capture local regularities; later stages may combine them into more abstract patterns. What a network actually learns depends on the data, architecture, optimization objective, and training process, not on the names engineers assign to its layers.

## Loss functions and backpropagation

Training requires a measurable objective. A loss function assigns a penalty to undesirable predictions. Cross-entropy loss, for example, penalizes a classifier for assigning low probability to the correct class. Backpropagation applies the chain rule to calculate how changing each parameter would change the loss. An optimizer uses those gradients to adjust parameters across many batches of examples.

The learning rate controls the scale of parameter updates. If it is too high, training can become unstable; if too low, progress may be slow. The model is not instructed explicitly to understand a concept: it is optimized to reduce loss. If an irrelevant watermark predicts the correct label in the training data, the network may learn to rely on that shortcut. Good training performance alone cannot prove that the intended relationship was learned.

## Generalization and overfitting

Overfitting occurs when a model performs well on familiar examples but poorly on unseen cases. Regularization methods such as weight decay, dropout, and early stopping can discourage brittle solutions. They do not substitute for carefully designed held-out data or tests of distribution shift.

Independent AI testing often has only black-box access to a model. The evaluator cannot inspect weights or retrain it, but can still measure observable behavior. Gray-box access to retrieval traces, tool calls, and application logs can improve diagnosis. The evaluation claim must match the evidence available: an external test can establish observed behavior under tested conditions, not a universal guarantee about internal reasoning.

## Worked explanation — The chain rule in backpropagation

Take a one-weight model $\hat{y} = wx$ with loss $L = \tfrac{1}{2}(wx - y)^2$. The chain rule gives

$$\frac{\partial L}{\partial w} = (wx - y)\,x$$

With $x = 2$, $y = 5$, and $w = 1$, the gradient is $(2 - 5) \times 2 = -6$. Gradient descent with learning rate $\eta = 0.1$ gives $w_{\text{new}} = 1 - 0.1 \times (-6) = 1.6$, moving the prediction from 2 to 3.2, closer to the target of 5. Deep networks apply the same chain rule through many layers.

---

[← Previous](03-probability-statistics-and-uncertainty.md) · [Contents](../README.md#contents) · [Next →](05-transformers-tokens-and-attention.md)
