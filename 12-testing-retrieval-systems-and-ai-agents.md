# Chapter 12. Testing Retrieval Systems and AI Agents

## A correct answer can conceal an incorrect process

For RAG, retrieval recall and answer faithfulness are separate measurements. A correct answer may come from memorized model knowledge even when the retriever returned no relevant evidence. Conversely, a faithful answer may repeat an obsolete policy. Testing only the final sentence conceals which subsystem succeeded or failed.

For agents, the same distinction is more consequential. A booking assistant may announce success without creating a reservation, or may complete a task through an unauthorized tool. Evaluation needs the final environmental state, tool-call arguments, authorization context, and evidence of side effects.

## RAG is a system of interacting stages

Retrieval-augmented generation, commonly called RAG, gives a language model access to external information at the time of a request. The system searches a document collection, selects relevant material, and places that material in the model's context. This can make changing business facts available without retraining the model. It also introduces new ways to fail. A system may retrieve the wrong passage, omit a critical exception, expose a document the user is not authorized to see, or generate an answer that does not follow the retrieved evidence.

A typical retrieval pipeline begins with ingestion and parsing. Documents are converted into text and structural elements, divided into chunks, and indexed. Embedding models can map text into numerical vectors so that semantically related passages can be retrieved. Keyword search preserves exact terms and identifiers. Hybrid retrieval combines these signals, while reranking can use a more detailed model to reorder candidate passages. Each stage influences the context that the generator eventually receives.

Chunking is a practical compromise. Very small chunks can lose context; very large chunks can contain distracting material and increase processing cost. A table may be misinterpreted if its heading or footnote is separated from the relevant row. Overlapping chunks can preserve continuity but also increase duplication. The appropriate strategy depends on document structure and the questions users ask. RAG quality therefore begins well before the foundation model generates an answer.

A long context window does not remove the need for careful retrieval. Even when a model can process a large quantity of text, it may overlook an important passage or confuse similar sections. Adding irrelevant material can increase latency and distract attention. The engineering objective is not to maximize context volume, but to supply the evidence needed for the task in an authorized, current, and interpretable form.

## Measuring retrieval quality

Retrieval must be evaluated independently of generation. Precision at k measures the fraction of the first k retrieved items that are relevant. Recall at k measures how much of the relevant material appears among those results. Ranking metrics reward placing important passages earlier. These measurements are useful only when relevance is defined carefully. A document may discuss the correct subject without containing the information required to answer the question.

Suppose a question asks whether advanced diving instruction requires prior certification. A general course overview may be topically relevant but insufficient. The actual prerequisite rule may appear in a separate policy paragraph. An evaluator that rewards only semantic similarity may approve the wrong retrieval. A stronger test labels answer-bearing passages and asks whether the retrieval system makes those passages available to generation.

Freshness and authorization must also be part of retrieval evaluation. A passage may be perfectly relevant but outdated. Another may answer the question while belonging to a different customer account. Neither should count as a successful retrieval for the deployed user. A real application needs document versioning, permission-aware filtering, and evidence that those controls work under representative conditions.

Useful diagnostic traces include the submitted query, any rewritten query, the candidate passages, the selected context, and source identifiers. If the correct passage was absent from the candidate list, the failure differs from one where it was retrieved but dropped during context assembly. Capturing these distinctions makes remediation more precise and prevents a developer from changing the wrong component.

## Answer correctness, faithfulness, and citations

The generated answer introduces a second evaluation problem. Correctness asks whether the answer agrees with the facts or requirements that matter. Faithfulness asks whether the answer is supported by the context provided to the model. These properties are related but not identical. A model can state a true fact not found in its retrieved context; it can also faithfully repeat a retrieved document that contains an obsolete policy. An evaluation report should say which property was measured rather than treating all favorable scores as proof of truth.

Citation evaluation is more demanding than checking whether a source name or link appears. A citation must support the specific claim it accompanies. A model can cite a genuine document while attributing to it a rule that is not there. Claim-level verification compares the proposition with the cited passage and determines whether the passage supports, contradicts, or fails to establish the claim. Citation presence is a formatting property; citation validity is an evidentiary property.

End-to-end RAG tests combine retrieval and generation through the user-facing interface. They reveal integration defects that component tests may miss. Component tests remain necessary because an end-to-end failure alone does not always identify the cause. In the snorkeling-care example, one test can check whether the correct care passage is retrieved, another can check whether the response includes both rinsing and air drying, and a third can verify that missing context produces an appropriate acknowledgment of uncertainty.

## Agents must be evaluated as workflows

An AI agent extends text generation into action. It may interpret a goal, select tools, supply arguments, inspect results, update state, and repeat until it believes the task is complete. Evaluating only its final answer is therefore insufficient. An agent can claim that a booking succeeded after the booking API returned an error. It can reach a correct final answer by using an unauthorized data source. It can execute a task twice because it misunderstood a timeout. These are failures of workflow behavior even when the final prose sounds reasonable.

The agent trajectory is the sequence of observations, decisions, tool calls, state transitions, and outputs that occurred during execution. Trajectory evaluation asks whether those steps were appropriate. Task-completion evaluation asks whether the intended environmental outcome actually occurred. A reservation is not complete because the agent says it is; the authorized booking system must confirm the reservation. Similarly, an agent that reveals another customer's record has violated a critical boundary regardless of the usefulness of its answer.

Tool-call correctness includes selecting the appropriate tool, supplying valid parameters, respecting authorization, and handling errors. Recovery testing introduces timeouts, malformed responses, unavailable services, and partial execution. The agent should not endlessly retry irreversible actions or fabricate success. Persistent memory creates further obligations: a test should verify what information survives between turns and whether information leaks across users or independent sessions.

Multi-agent workflows introduce additional boundaries. One agent may delegate to another, which may use a different tool or permission set. Coordination can improve task specialization but also obscure responsibility for mistakes. An evaluation must preserve the identities of participating components, the permissions in effect, and the provenance of important decisions. A standardized tool interface makes communication more predictable, but does not guarantee that the tool is used correctly or safely.

## Controlled environments and independent evidence

Agents often require controlled environments for safe evaluation. A test should not need to make real purchases, send real customer messages, or alter production records to verify tool behavior. Sandbox services can expose realistic interfaces while allowing the evaluator to inspect exact state changes. These tests establish behavior under controlled conditions; live customer acceptance requires separate verification against the intended deployment environment.

A rigorous agent scenario defines the initial state, the user goal, permitted and prohibited actions, expected final state, and required recovery behavior. Consider an assistant arranging a training session. It may inspect availability through an approved service, but must not commit a booking without the required authorization. The decisive oracle is not whether the agent uses polite language; it is whether the environment records an authorized booking and whether no unauthorized side effects occurred.

An independent evaluation platform may see only black-box behavior. It can submit authorized requests and observe outputs, but it may not see internal prompts or tool traces. Under those conditions it can establish observable failures while leaving internal causes uncertain. With authorized gray-box access, retrieval traces and tool records can support stronger diagnosis. AI Test Lab should therefore distinguish evidence it actually observed from explanations it infers, and should avoid presenting a plausible cause as a proven one.

The strongest reports connect a requirement to an observation, an evidence artifact, a decision rule, and an explicit limitation. This is what turns a demonstration into engineering assurance. It is also the bridge to the next subjects of the book: security boundaries, automated regression, production observability, and the governance of AI systems. Those disciplines are not separate from evaluation; they determine whether an evaluation can be trusted and acted upon.

## Worked explanation — Retrieval and agent oracles

Retrieval recall at $k$ asks whether the necessary source appears among the first $k$ retrieved results. If 43 of 50 questions retrieve an answer-bearing passage in the first five results, recall at five is $43/50 = 86\%$. This does not establish that the passages are current, authorized, or correctly used. Generation faithfulness must separately check whether each material claim follows from the supplied context. Answer correctness must compare the response with authoritative facts, which may differ from a stale retrieved document.

Agent testing requires an environmental oracle. An agent can announce that a refund was issued without any refund record existing, or issue two refunds after an ambiguous timeout while claiming one. Test evidence should include tool arguments, permission checks, idempotency keys, observed side effects, and final state. The authoritative oracle is the state of the refund system together with evidence of proper authorization — not the agent's final summary.

---

[← Previous](11-hallucinations-robustness-and-behavioral-testing.md) · [Contents](../README.md#contents) · [Next →](13-ai-security-and-trust-boundaries.md)
