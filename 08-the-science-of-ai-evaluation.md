# Chapter 8. The Science of AI Evaluation

## Construct validity: what exactly is being measured?

A measurement can be reproducible while measuring the wrong property. An evaluator might call a score factual accuracy when it actually computes textual similarity. Two sentences can be semantically similar yet differ on a critical negation or threshold. Construct validity asks whether the instrument measures the property named in the report. For a policy assistant, a defensible evaluation separates factual support, completeness, authorization, and formatting rather than compressing them into one unexamined score.

Consider a refund assistant required to apply two rules: purchase within thirty days and exclusion of opened safety equipment. The answer "Yes, because the purchase was last week" addresses the first rule but ignores the second. A claim-level oracle checks each proposition against approved evidence. If item condition is unknown, the appropriate verdict may be insufficient evidence. The distinction between unsupported and demonstrably false is fundamental: absence of proof is not automatically proof of falsity.

## Causal identification through controlled interventions

Fault injection can support causal reasoning only when the intervention is controlled. If a retrieval experiment removes the approved policy passage, the model revision, prompt, question, permissions, and grading rubric must remain unchanged. The evaluator must verify that the model actually received empty context. A hidden cache, persistent conversation state, or changed rubric can invalidate the intended comparison. A baseline–fault–recovery sequence is therefore an experiment with manipulation checks, not simply three screenshots.

If behavior degrades when evidence is withheld and improves when it is restored, the experiment supports sensitivity to the manipulated retrieval condition. It does not prove that all failures originate in retrieval, or that the system is reliable for every possible query. The scientific strength of the result comes from its narrowness: it states precisely which condition changed and which behavior was observed.

## Evaluation validity precedes scoring

A measurement is useful only when it measures the property it claims to measure. Exact match may be valid for a required JSON field and invalid for an open-ended explanation. Semantic similarity may recognize paraphrases yet overlook a negation that reverses the answer. The evaluator must connect each score to an explicit construct, oracle, and failure consequence.

A complete evaluation argument asks: What requirement is being tested? What evidence would support or contradict compliance? What alternative explanations remain? What population do the cases represent? Only then should the result be summarized numerically.

## From software assertions to behavioral measurement

Conventional software tests often compare an observed result with a known expected result. When a function calculates a sales tax, the correct numerical answer can be established independently. Generative AI complicates this familiar arrangement because many tasks permit multiple acceptable answers. An assistant asked to explain a cancellation policy may express the same correct rule in dozens of ways. The evaluator must therefore distinguish harmless variation in wording from meaningful variation in factual content, completeness, authorization, or user impact.

Evaluation is a measurement process rather than a collection of prompts. Every measurement needs an object, a standard, a procedure, and an account of uncertainty. The object might be an answer, a retrieved document, a tool call, or an entire workflow. The standard might be an authoritative policy, a business rule, a controlled environmental state, or a carefully defined human rubric. If these elements are not explicit, a score of ninety percent says remarkably little: it may reflect correct facts, attractive prose, valid formatting, or some mixture of all three.

The distinction between evaluating a foundation model and evaluating an application built around it is fundamental. A model can perform well on a public benchmark while the surrounding application fails because its retrieval index is outdated, its prompt omits a constraint, or an API returns an unexpected value. A less capable general model may be more dependable in a narrow application when paired with authoritative data and deterministic safeguards. The unit of evaluation must match the actual engineering question.

The most useful starting point is a failure model: a reasoned account of what can go wrong and why the consequences matter. A slightly awkward explanation is not equivalent to an invented refund entitlement. An incorrect search ranking is not equivalent to a cross-tenant disclosure. Good evaluation therefore measures several dimensions independently and assigns severity according to the use case. It resists the temptation to compress all quality into one reassuring number.

## Oracles, ground truth, and acceptable alternatives

An evaluation oracle is the mechanism that determines whether observed behavior meets a requirement. Some oracles are exact: an authorization decision must deny an unauthorized request, a generated JSON document must satisfy its schema, and a calculated quantity must have the correct value. Other oracles must allow semantic alternatives. Two answers can be different strings and still convey the same verified meaning. This is why exact matching is powerful for identifiers but inadequate as the sole method for assessing explanatory language.

Semantic similarity can help identify related meanings, but similarity is not proof of truth. Consider the sentences "A refund is available within forty-eight hours" and "A refund is unavailable within forty-eight hours." They share most of their vocabulary and subject matter, yet the negation reverses the policy. Embedding-based similarity may rank them as close even though they cannot both be correct under the same rule. A factual evaluator must inspect the actual claims and the evidence supporting them.

Reference-based evaluation compares an output against a prepared answer or set of expected propositions. Its strength is consistency: the target is defined before the system runs. Its weakness is incompleteness: a reference answer may omit valid alternative explanations or become outdated. Reference-free evaluation instead judges an output using the task instructions and available context. It can be more flexible, but it depends heavily on the evaluator's reasoning and on the quality of the context supplied. Neither approach is universally sufficient.

Suppose a diving-school assistant is asked whether beginners may enroll. The approved course description says no prior certification is required. "Yes, beginners are welcome" and "Previous certification is not necessary" should both pass. But "Yes, and students must be at least eighteen" introduces a new factual claim. A claim-level oracle can approve the first proposition while rejecting or marking the added age requirement as unsupported. That is a more informative outcome than merely assigning the entire response a similarity score.

## Evaluation datasets as experimental instruments

An evaluation dataset is a sample of the situations a system may encounter. Its composition determines which kinds of performance the evaluation can reveal. If the dataset contains only short, clear questions, it may hide failures caused by ambiguity, conflicting documents, multilingual input, long conversations, missing evidence, or hostile instructions. Representative testing requires an explicit description of the intended user population, tasks, environments, and failure consequences.

Random samples of production requests and deliberately constructed stress tests serve different purposes. A random sample can estimate average performance under the sampled traffic distribution. A stress suite intentionally overrepresents rare or dangerous situations to discover vulnerabilities. Both are valuable, but they should not be merged into a single percentage without explanation. A test set dominated by adversarial prompts does not estimate ordinary customer experience; an ordinary traffic sample may miss rare security failures.

Human labels also require quality control. Reviewers can disagree because the evidence is incomplete, because a policy is ambiguous, or because the rubric leaves too much room for interpretation. Independent annotation, adjudication, clear written criteria, and dataset versioning help establish a more stable measurement. A label should not be treated as infallible merely because a person assigned it. When the policy or rubric changes, comparisons with older results must acknowledge the changed standard.

Synthetic test cases can broaden coverage but may reproduce unrealistic assumptions or errors from the generator that created them. A model-generated customer question can sound plausible without resembling genuine customer language. Synthetic examples are most useful when their purpose is clear, their expected behavior is independently checked, and they complement rather than replace representative real-world samples. Evaluation contamination is another concern: repeatedly tuning an application on the same held-out cases gradually makes those cases less independent.

## Evidence, validity, and AI Test Lab

A credible evaluation report preserves the conditions under which the result was obtained: the input, context, application revision, relevant model settings, observed output, evaluator version, decision rule, and rationale. This does not guarantee identical text on every repeat, because probabilistic systems can vary. It does allow another investigator to understand what was tested, which assumptions were made, and whether a comparison is meaningful.

Evidence integrity and evaluation validity are different properties. A digest can establish that a stored report has not changed since it was sealed, but it cannot establish that the report's assertions were correct. A perfectly preserved false conclusion remains false. Likewise, a correctly designed rubric cannot compensate for evidence that has been substituted after evaluation. Trustworthy AI assurance needs both sound measurement and reliable preservation.

AI Test Lab offers a practical example of this distinction. An independent evaluation platform can execute defined cases, record observed application behavior, and preserve verifiable evidence. Its usefulness depends on whether its oracles actually measure customer requirements, whether its datasets represent meaningful risks, and whether its conclusions remain appropriately limited. The aim is not to claim that a model is universally safe or correct, but to make defensible statements about observed behavior under documented conditions.

## Silent production failures and the limits of deterministic assertions

A silent failure occurs when an application continues to return formally valid responses while violating a behavioral requirement. An API can respond with status 200, satisfy its JSON schema, and complete within a latency objective while conveying an incorrect policy or taking an unauthorized action. Classical contract tests remain necessary, but their success proves the contract they check, not the correctness of every semantic assertion or state transition. For an AI application, the observed result depends on the user request, assembled context, model revision, retrieval state, tool responses, persistent state, and execution environment. A passing test at one combination does not establish reliable behavior over the whole distribution of combinations.

Consider a hypothetical returns agent that requests a shipping authorization from an upstream service. The service commits the request but its response times out. The agent assumes the operation failed, retries without an idempotency key, and creates a duplicate authorization. It subsequently reports that exactly one label was issued. Each individual tool call may have valid arguments, and the final message may look plausible. The defect lies in the trajectory: the sequence of actions and environmental effects. A meaningful evaluation tests confirmed success, confirmed failure, and ambiguous timeout; verifies retry policy and idempotency; and checks the external system of record rather than trusting the agent's final sentence.

A second hypothetical failure arises after a minor model revision. The same prompt, endpoint, and output schema remain valid, but the model increasingly interprets "may request a refund" as "is guaranteed a refund." This is semantic drift: the probability distribution over materially different interpretations has changed without a conventional interface break. A controlled paired evaluation holds the dataset, prompt, retrieval corpus, and rubric fixed while recording the actual model revision. If several components change together, the test may establish a regression but cannot legitimately attribute its cause to the model patch alone.

Indirect prompt injection offers a third example. An assistant retrieves a document containing a command to export a customer's records to an external address. The document is relevant as information but has no authority to issue instructions. If the agent obeys it, a trust boundary has been crossed. Schema tests for retrieval and export may all pass. The required oracle verifies that lower-trust content cannot authorize actions, and inspects tool-call traces and actual side effects. Permission enforcement must exist outside the language model; a polite refusal in one test is not an adequate security boundary.

These cases are illustrative composites, not assertions about particular real incidents. They establish why evaluation must observe both outputs and processes. A correct final answer can conceal an incorrect trajectory; an incorrect answer can result from retrieval rather than generation; and an apparently stable interface can conceal a changed behavioral distribution. Scientific evaluation distinguishes the observed symptom from a causal hypothesis and states exactly which evidence supports each.

## Manual inspection, «погрешность», and the economics of rare failure

Informal prompt testing is a measurement procedure with poorly controlled error. The developer typically chooses familiar questions, knows which version is new, and interprets fluent prose as a sign of correctness. Measurement error, «погрешность», has at least two distinct sources. Random error («случайная ошибка») reflects variation across samples and reviewers. Systematic error («систематическая ошибка») arises when selection, labeling, or judgment consistently distorts the result. A useful conceptual model is:

$$\text{observed estimate} = \text{target quantity} + \text{bias} + \text{random error}$$

 Repeating a biased review may reduce random variability without reducing bias.

Suppose a developer reports 96 successes among 100 handpicked questions. The observed rate is 96%, but it describes those questions, not necessarily future traffic. Even reviewing 1,000 similarly easy questions cannot establish production reliability if difficult and adversarial cases were excluded. Blind review, written rubrics, adjudication, representative sampling, and independent held-out sets address different aspects of this problem. Human-in-the-loop review is valuable for ambiguous and consequential judgments; it does not eliminate the need for statistical design.

Rare failures make manual random inspection especially ineffective. If each independent representative trial has failure probability p, the probability of detecting at least one failure in $n$ trials is

$$P(\text{at least one failure}) = 1 - (1 - p)^n$$

At $p = 0.0001$, or one failure per 10,000 requests, 100 tests have a detection probability of only about 0.995%, and 1,000 tests about 9.52%. Requiring a 95% chance of seeing the failure means solving $1 - (1-p)^n \ge 0.95$, which gives $n \ge \ln(0.05)/\ln(0.9999)$ — approximately 29,956 trials. At six minutes of human inspection per trial, this entails about 2,996 reviewer-hours, assuming every encountered failure is actually recognized.

Zero observed failures do not establish zero risk. Under independent representative Bernoulli trials, the rough 95% upper bound following zero failures is $3/n$. With 300 clean trials the bound is about 1%; with 3,000 it is about 0.1%. These bounds are not valid without their sampling assumptions. Cross-tenant disclosure may occur only after a specific authorization mismatch, cache collision, or multi-step sequence, so ordinary random traffic may never exercise its preconditions. Risk-directed boundary tests and controlled adversarial scenarios are needed alongside representative sampling. A targeted exploit establishes reachability under tested conditions, not the frequency of occurrence in production.

Economics reinforces the mathematics. At ten reviewed traces per hour, 10,000 traces require 1,000 reviewer-hours before adjudication and re-review after updates. The scientifically defensible approach is not to remove humans, but to allocate them intelligently: deterministic checks for exact contracts, calibrated automated evaluation for scalable semantic properties, and expert review for disputed or high-impact outcomes. This architecture produces more auditable coverage than the subjective impression that a handful of answers "look good."

## Worked explanation — When similarity is not accuracy

The approved policy states: "Opened safety equipment is not eligible for a refund." A candidate answer states: "Opened safety equipment is eligible for a refund." The policy contains nine words. The answer contains eight, and every one of them also appears in the policy. A simple word-overlap score — shared words divided by all distinct words — is therefore $8/9 \approx 0.89$, which looks like a strong match. An embedding-similarity score would typically also be high, because the two sentences discuss the same subject in almost the same language.

Yet the answer reverses the policy. A claim-level oracle extracts the single material claim — opened safety equipment is eligible for a refund — and checks it against the approved passage. The passage contradicts it, so the verdict is a failure, regardless of surface similarity.

The lesson is construct validity in miniature. A repeatable score is not necessarily a valid one. Exact equality suits identifiers, source-grounded claim checks suit factual statements, and environmental-state checks suit agent actions. Pairing exact deterministic checks for structured fields with claim-level verification ensures that each factual assertion is audited against approved sources.

---

[← Previous](07-engineering-ai-applications.md) · [Contents](../README.md#contents) · [Next →](09-metrics-statistics-and-reliable-conclusions.md)
