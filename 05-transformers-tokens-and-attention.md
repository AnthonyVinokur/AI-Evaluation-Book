# Chapter 5. Transformers, Tokens, and Attention

## Why transformer evaluation must look beyond fluent text

Self-attention enables tokens to condition on other tokens in a sequence. For a single attention head, the standard expression is

$$\mathrm{Attention}(Q,K,V) = \mathrm{softmax}\!\left(\frac{QK^{\top}}{\sqrt{d_k}}\right)V$$

 The calculation determines weighted combinations of representations, not whether the represented propositions are true. A transformer can therefore produce a fluent but unsupported answer even when its internal computation is functioning as designed.

Evaluation must separate language fluency, factual grounding, context utilization, and task success. A benchmark that rewards plausible continuation may be poorly aligned with an application that must cite current policies or refuse unauthorized requests. The architecture explains what the model computes; evaluation determines whether that computation serves the application.

## From recurrent networks to transformers

Earlier sequence models often processed tokens one after another, carrying a hidden state forward. This made long-distance dependencies difficult to preserve and restricted parallel processing during training. The transformer architecture, introduced in 2017, made attention central to the computation. Rather than forcing all previous information into one evolving summary, attention allows positions in a sequence to combine information dynamically.

Modern language models generally use transformer variants that combine attention, feed-forward networks, normalization, and residual connections. The transformer did not invent attention, but showed how attention could underpin an effective sequence architecture without relying on recurrence for input processing.

## Tokenization and embeddings

A model receives tokens, which can represent words, word fragments, punctuation, or other units. Tokenization maps text to identifiers; an embedding layer maps those identifiers into learned numerical vectors. Embeddings are not dictionary definitions. They are representations shaped by training to support useful predictions.

Tokenization affects context length, inference cost, and performance across languages. Equivalent meanings may require different token counts in different languages. Long context increases the amount of potentially relevant material, but it does not guarantee that the model will find or correctly use a fact buried within it.

## Queries, keys, and values

Self-attention derives query, key, and value vectors from the current token representations. Similarities between queries and keys determine weights used to combine value vectors. Multiple attention heads allow different relationships to be represented in parallel. Decoder-only language models apply a causal mask so a position cannot access future generated tokens.

During inference, a model first processes the supplied context, then generates tokens sequentially. Sampling controls which token is selected from the predicted distribution. Attention weights should not be treated as complete explanations of why an answer was generated or whether it is correct. For evaluation, the important distinction is between evidence being present, evidence being attended to effectively, and evidence being used accurately.

## Compute and context

Standard full attention considers pairwise interactions between positions, which can become expensive for long sequences. Serving systems use optimized attention kernels and key-value caches to manage computation. Prefill, the processing of the input, and decode, the production of new tokens, have different performance characteristics.

An independent evaluator should therefore measure time to first token separately from overall response latency. A long document can increase both cost and failure opportunities. The engineering objective is not the longest possible context but the smallest sufficient context that supports a correct and well-grounded answer.

## Worked explanation — A two-token attention example

Suppose one query produces attention scores of 2 and 0 for two tokens. The softmax weights are $e^2/(e^2 + e^0) \approx 0.881$ and $1/(e^2 + 1) \approx 0.119$. If the corresponding values are 10 and 2, the weighted result is $0.881 \times 10 + 0.119 \times 2 \approx 9.05$. Real attention operates on vectors with multiple heads; the simplified arithmetic shows how context-dependent weights combine information.

---

[← Previous](04-neural-networks-and-deep-learning.md) · [Contents](../README.md#contents) · [Next →](06-training-and-adapting-foundation-models.md)
