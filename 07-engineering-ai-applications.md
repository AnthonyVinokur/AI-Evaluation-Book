# Chapter 7. Engineering AI Applications

## Evaluation belongs in the application architecture

A commercial AI application should be designed so that its behavior can be observed and tested. This includes stable authorized interfaces, request identifiers, version metadata, structured error responses, and controlled access to relevant traces. Evaluation added only after deployment often struggles to distinguish a model failure from retrieval, routing, permission, or tool failures.

Black-box testing can establish what an external user observed. Gray-box testing, with authorized traces, can often narrow the cause. White-box testing can inspect implementation details. These access levels support different claims, and an independent evaluation provider should state the limits of its visibility instead of treating missing telemetry as evidence of correct behavior.

## The application beyond the model

An AI application includes user interfaces, backend logic, identity management, data access, monitoring, and error handling. The model is one component of that system. It may draft a refund explanation, but deterministic code must enforce the refund policy and authorize any financial action. A model response can be plausible while the application as a whole behaves incorrectly.

Evaluation therefore needs component-level and end-to-end perspectives. If the retrieval system supplies an obsolete policy, the model may follow the context faithfully while still giving the customer an incorrect answer. If the model answers correctly but the interface shows another customer's information, the application has failed at a different layer.

## Prompting and context

Prompt engineering designs instructions, examples, constraints, and output formats. Context engineering determines which documents, history, tool outputs, and other information are supplied. More context is not necessarily better. The useful context is relevant, authoritative, timely, and appropriate to the user's permissions.

Untrusted documents can contain instructions that attempt to override the application's legitimate rules. This is prompt injection. The defense cannot rely solely on a stronger prompt. Authorization, tool permissions, output validation, and control of external side effects must be enforced outside the model.

## Retrieval-augmented generation

Retrieval-augmented generation supplies external evidence at request time. A system ingests documents, splits them into retrievable passages, builds keyword or vector indexes, selects candidates, reranks them, and passes relevant material into the model context. This can provide private or changing facts without retraining.

Retrieval and generation fail differently. Retrieval may miss the right passage; generation may misinterpret a correct passage. Evaluation should measure whether needed evidence was retrieved, whether it ranked highly enough to be used, whether claims are supported by that evidence, and whether the answer actually satisfies the user's question. A citation alone does not establish support for a claim.

## Agents and tool use

Agents can plan tasks, select tools, act on external systems, and react to observations. This makes them more capable than a single text-generation step, but it also creates operational risks. An agent can choose the wrong tool, submit invalid arguments, duplicate an action, or modify a resource without authorization.

Testing an agent therefore requires inspection of its trajectory: tool calls, permissions, state transitions, side effects, recovery, and final task completion. The Model Context Protocol provides a standard way for clients and servers to exchange tool and resource information; it does not by itself guarantee that an agent will use those capabilities safely. Controlled environments are essential for evaluating consequential actions. AI Test Lab can be studied as an independent evaluator of such workflows, but specific production capabilities must be established from implementation evidence.

## Worked explanation — Tracing a production request

A request may pass through authentication, retrieval, context assembly, model inference, output checks, and tool execution. An accurate answer derived from a document the user was not allowed to access is still an application failure. The test boundary must include authorization and external side effects, not merely text quality.

---

[← Previous](06-training-and-adapting-foundation-models.md) · [Contents](../README.md#contents) · [Next →](08-the-science-of-ai-evaluation.md)
