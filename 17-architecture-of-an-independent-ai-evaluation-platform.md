# Chapter 17. Architecture of an Independent AI Evaluation Platform

## The business case for independent evaluation

A model provider's benchmark and an application team's own test report are useful, but they may not answer the customer's question: does this deployed system satisfy our requirements under our conditions? Independent evaluation reduces reliance on self-attestation by separating the evaluator from the application being assessed, while still requiring authorization, sound oracles, and transparent limitations.

Independence is not magic. A third party can run a poor experiment. The commercial value arises when independent operation is paired with reproducible methods, representative scenarios, credible evidence, and reports that identify both defects and uncertainty.

## Levels of access

An independent evaluator interacts with a customer's application only through interfaces the customer has authorized. The level of access determines what conclusions are possible.

| Access level | What the evaluator sees | What it can establish |
|---|---|---|
| Black-box | Requests and responses only | Observed behavior and requirement violations |
| Gray-box | Plus approved traces: retrieval identifiers, tool calls, versions | Which component a failure most likely came from |
| White-box | Plus implementation, configuration, sometimes weights | Specific internal causes |

Each additional level improves diagnosis and adds a confidentiality obligation. An independent evaluation system can assess authorized external interfaces without obtaining model weights; most commercial engagements operate at the black-box or gray-box level. The platform should not claim internal causation when it possesses only external observations.

## Separating execution from judgment

A clear architecture divides the evaluation process into layers, each with one responsibility.

The **definition layer** holds versioned test cases, scenarios, and policies: what to send, under what conditions, and what requirements apply.

The **execution layer** submits cases to the target through adapters and captures observations — responses, timing, and any authorized telemetry. It does not decide whether the result was good.

The **oracle layer** determines whether each requirement was met. It includes exact checks, schema validation, evidence-grounded claim checks, security rules, calibrated model judges, and routing to human review.

The **provenance layer** binds each observation and verdict to its versions and seals the record, as described in Chapter 16.

The **reporting layer** explains findings, uncertainty, and limits in a form the customer can act on.

Keeping execution separate from judgment has a practical benefit: when an oracle is improved, stored observations can be re-scored without re-running the application, and the change in verdicts can be attributed to the oracle rather than to a change in the system under test.

## Adapters and target identity

Customer interfaces vary — REST APIs, chat widgets, message queues, agent frameworks. Adapters normalize them into a common form so that the same test definitions can be reused. A good adapter normalizes without discarding detail: status codes, headers, timing, and raw payloads should all be preserved, because a failure that looks like a model error may turn out to be an interface error.

The target deployment must be identified precisely. An endpoint may change while its URL remains constant; a customer may redeploy between the baseline and the comparison run. The evaluator should capture whatever version identifiers the target exposes and, where none exist, record that the deployment identity could not be confirmed.

## Combining verdicts

Different oracles measure different properties: exact oracles check contracts, semantic checks assess meaning, security rules check boundaries, and statistical estimators summarize rates. Policy logic can combine them into a release recommendation, but the combination must be explicit. A single aggregate score must not conceal a critical violation. A response that is helpful, well-formatted, and leaks another customer's data has failed, whatever its average score.

## Worked explanation — One request through the layers

Consider an evaluation of a care-instructions assistant for snorkeling equipment. The approved guidance requires two steps: rinse with fresh water, then air dry.

The definition layer holds the case: the question "How should I clean my snorkel after use?", the requirement that both steps appear, and the policy version they come from. The execution layer sends the question through the customer's chat API and captures the response, the response time of 2.1 seconds, and — because the engagement includes gray-box access — the identifiers of the two retrieved passages.

The oracle layer finds that the answer mentions rinsing but omits air drying. It records a failure of the completeness requirement. It then checks the retrieval trace: neither retrieved passage contains the drying instruction. The provenance layer seals the request, response, trace, and verdict together with the policy and target versions.

The report states the observed failure as a fact — the answer omitted a required step. It states that the required passage was not among those retrieved, which is also observed. And it presents retrieval as the likely cause, while noting that a controlled experiment would be needed to confirm it. Had the engagement been black-box only, the report could state the failure but would have to present any cause as an unverified hypothesis. If the answer omits air drying, that is an observed failure; a retrieval root cause remains a hypothesis unless retrieval evidence was actually captured.

---

[← Previous](16-observability-governance-and-evidence.md) · [Contents](../README.md#contents) · [Next →](18-evaluation-execution-evidence-and-verification.md)
