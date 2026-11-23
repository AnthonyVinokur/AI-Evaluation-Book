# Chapter 6. Training and Adapting Foundation Models

## Every adaptation creates a new evaluation obligation

Pretraining, supervised fine-tuning, preference optimization, retrieval, and prompting alter behavior through different mechanisms. Each can improve some tasks and degrade others. Fine-tuning may increase domain fluency while weakening general performance; a prompt revision may improve format compliance while causing more abstentions. The only defensible way to compare these changes is to test against stable requirements with appropriate held-out cases.

The practical implication is that an application version is more than a model name. Its behavior depends on weights, prompts, tools, retrieval data, decoding settings, and deployment configuration. A meaningful regression test records these identities so that a changed result can be investigated rather than attributed vaguely to "the AI."

## Pretraining

A foundation model is trained on broad data so that it can support many downstream tasks. In autoregressive language-model pretraining, the model repeatedly predicts the next token from previous tokens. This objective encourages it to learn grammar, coding patterns, factual associations, and other statistical regularities. It does not directly require the model to verify claims or follow an organization's rules.

Training data may contain inaccuracies, stereotypes, outdated information, and contradictory statements. A fluent answer is therefore not evidence of a reliable source. The distribution of training examples influences strengths and weaknesses, particularly for uncommon languages and specialized domains.

## Scale and mixtures of experts

Model capability depends on architecture, data quality, compute, and parameter count. Larger models can have more capacity, but size alone does not guarantee better task performance. The optimal choice for a product also depends on inference cost and latency. A smaller model may outperform a larger one on a specialized task when properly adapted.

Mixture-of-experts architectures activate only selected groups of parameters for each token. This can increase total capacity without requiring every parameter to participate in every computation. Routing, memory requirements, and distributed serving introduce their own trade-offs. Customers ultimately care about accuracy, cost, speed, and reliability, not parameter count in isolation.

## Post-training and preferences

A pretrained completion model is not automatically a helpful assistant. Supervised fine-tuning uses examples of desired responses to teach instruction-following behavior. Preference optimization uses comparisons or reward signals to favor outputs that meet chosen criteria. Reinforcement learning from human feedback and direct preference optimization are two approaches.

Preference is not the same as factual correctness. A model may learn to sound helpful or confident without becoming more truthful. A rigorous evaluation must separately measure instruction compliance, factual grounding, uncertainty handling, and safety. Model upgrades can change all of these behaviors, making regression testing essential.

## Inference-time reasoning

Additional inference-time computation can be used to explore candidate answers, verify solutions, or invoke tools. This can improve some tasks while increasing latency and cost. Its value depends on the quality of the verifier: a weak judge may reward persuasive but incorrect candidates.

Multimodal models extend these ideas to images, audio, video, and other inputs. Each modality introduces specific failure modes. For instance, a model may correctly describe the general scene in an image while misreading a small label. Evaluation must use evidence appropriate to the task rather than assuming that good language performance implies good visual accuracy.

## Worked explanation — Temperature and inference behavior

Given logits $z_i$ and temperature $T$, sampling probabilities are

$$p_i = \frac{e^{z_i/T}}{\sum_j e^{z_j/T}}$$

For two tokens with logits 2 and 1, temperature 1 gives probabilities of about 0.73 and 0.27; temperature 0.5 gives about 0.88 and 0.12; temperature 2 gives about 0.62 and 0.38. Lower temperature concentrates probability on high-logit tokens; higher temperature spreads probability across alternatives. This controls output diversity but does not certify factual correctness. Post-training, retrieval, and external validation address different engineering problems.

---

[← Previous](05-transformers-tokens-and-attention.md) · [Contents](../README.md#contents) · [Next →](07-engineering-ai-applications.md)
