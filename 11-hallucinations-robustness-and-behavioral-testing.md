# Chapter 11. Hallucinations, Robustness, and Behavioral Testing

## Why hallucination detection is a reliability problem

Hallucination evaluation should distinguish a demonstrably false claim from a claim merely unsupported by available evidence. A source may be missing, outdated, or contradictory. Calling every unsupported statement false overstates what the evaluator knows; treating every fluent statement as acceptable understates the risk.

Robustness tests should vary wording, context order, retrieval availability, and tool outcomes while preserving the requirement being tested. Repeated trials estimate instability; fault injection tests whether a known defect becomes observable; recovery trials check whether normal behavior returns. None of these alone proves the root cause of an unknown failure.

## Why plausible language can be wrong

A language model learns patterns that allow it to produce coherent sequences of text. That achievement is remarkable, but the ability to write a convincing sentence is not the same as the ability to verify the sentence. A model can generate an explanation of a policy that sounds authoritative even when the policy does not contain the claimed rule. This difference between plausibility and truth is one of the central problems of AI engineering. The model is performing a generation task; the application must separately establish whether the resulting claims are supported.

The word hallucination often refers to statements that are false, fabricated, or unsupported. These are not identical categories. A statement can contradict a verified fact, in which case the error is demonstrable. Another statement may be plausible but absent from the evidence supplied to the application; it is unsupported, although not necessarily false. A third statement may have been correct last year but no longer reflects current policy. Each failure requires a different response. Better retrieval can supply missing evidence; freshness controls can prevent outdated answers; an explicit uncertainty policy can prevent a model from inventing details when information is unavailable.

An initial incorrect assumption can also influence everything that follows. If a model invents a prerequisite for a nonexistent training course, it may continue explaining that prerequisite in increasingly specific terms. The generated text becomes part of the context for subsequent tokens. The final answer can be internally consistent while being externally wrong. Internal consistency therefore cannot substitute for evidence. A fluent and detailed answer may actually be more dangerous because readers are less likely to question it.

Lowering sampling temperature can reduce some variation, but it does not turn generation into fact-checking. A model may consistently repeat the same mistake. Nor does a larger model eliminate the possibility of error. Reliability depends on the entire system: its data, instructions, retrieval, tools, permissions, and verification mechanisms.

## The origin of an error matters

A modern AI application often contains several stages between a user question and the final answer. The application may rewrite the query, retrieve passages, select context, call a foundation model, validate the output, and invoke external tools. An incorrect answer can originate at any of these boundaries. If the correct document was never retrieved, the generator may be unable to answer faithfully. If the correct document was retrieved but a critical exception was truncated, the resulting answer may be incomplete. If a tool supplied an obsolete value, the model may accurately repeat information that is operationally wrong.

Consider a maintenance assistant for snorkeling equipment. Its approved care guidance requires rinsing with fresh water and allowing the equipment to air dry. During a baseline test, the assistant includes both instructions. During a controlled fault, the retrieval system supplies no relevant passage, and the response omits air drying. The observed defect is an incomplete answer. The likely explanation is missing retrieval context, but a reliable diagnosis should inspect the retrieval request, candidate passages, selected context, and model output before asserting that the retrieval component caused the failure.

This distinction is essential for causal analysis. The same symptom could arise from an indexing error, a ranking error, a context-length limit, or a generation error that ignored a relevant passage. A diagnostic system should maintain competing hypotheses until evidence distinguishes them. Fixing the prompt to force the phrase air dry might make one test pass while leaving the underlying retrieval defect intact. Effective engineering repairs causes rather than merely hiding symptoms.

When traces are unavailable, conclusions must remain appropriately narrow. A black-box evaluator can establish that a response violated a requirement, but cannot always identify the internal component responsible. Gray-box access to retrieval records and tool calls makes diagnosis more precise. Independent evaluation gains credibility by distinguishing what was observed from what was inferred.

## Robustness and metamorphic relationships

Robustness is the ability to preserve required behavior across changes that should not alter the underlying task. A person might ask whether a beginner can enroll, whether certification is required, or whether someone with no experience may register. The wording changes, but the relevant policy remains the same. A robust assistant should preserve that policy even if its sentences differ. This does not require identical outputs. It requires stable factual and behavioral properties.

Metamorphic testing is useful when there is no single exact answer to compare. Instead of specifying a complete expected sentence, an evaluator specifies a relationship that should hold across transformed inputs. Paraphrasing a question should not reverse an eligibility decision. Adding irrelevant background should not change a numerical result. Reordering independent evidence passages should not create a new policy. An unauthorized request should not become authorized because it is phrased more persuasively. These are testable relationships even when the final response is open-ended.

The transformation itself must be valid. Changing before to after in a cancellation question can legitimately change the answer. Removing a qualification from a policy can change its meaning. Translation may introduce ambiguity. A metamorphic test is only trustworthy when the evaluator has established that the transformation preserves the property under investigation. Otherwise, the test can confuse legitimate adaptation with failure.

Repeated execution addresses another dimension of robustness. A model may answer correctly nine times and fail on the tenth. A single success therefore cannot establish stability. Multiple runs with recorded settings can reveal variability and help estimate failure frequency, although repeated outputs may be correlated and should not be treated as independent samples without justification.

## Adversarial inputs and uncertainty

Ordinary robustness tests examine benign changes. Adversarial tests examine attempts to manipulate the application. A malicious retrieved document may contain instructions telling the model to disregard the user's request or reveal hidden data. A user may disguise an unauthorized action as a harmless request. Such attacks exploit confusion between trusted instructions and untrusted content. The appropriate defense includes application-level authorization, strict tool boundaries, validation, and testing; a warning inside a prompt is not a substitute for these controls.

Reliable applications also need an appropriate response to missing information. If a course catalog does not state the maximum training depth, the assistant should not invent a depth simply to be helpful. Abstention can be correct. Yet a system that refuses every question is not useful. Evaluation must measure both the tendency to answer without support and the tendency to refuse when sufficient evidence is present. The goal is selective reliability rather than universal confidence or universal refusal.

A model's verbal expression of confidence is not necessarily calibrated. Saying that an answer is 95% certain does not demonstrate that 95% of similar answers are correct. Calibration requires comparisons with observed outcomes across representative cases. When such calibration is unavailable, it may be more informative to describe evidence status: supported by the approved document, contradicted by the document, absent from the available material, or requiring review.

## Abstention and the coverage–accuracy trade-off

A reliable assistant must know when not to answer. Define coverage as the fraction of questions receiving a substantive answer, and selective accuracy as correctness among those answered. If an assistant answers 80 of 100 questions and 76 of its answers are correct, coverage is 80%, selective accuracy is $76/80 = 95\%$, and correct-answer yield — correct answers as a share of all questions — is 76%. Reporting only 95% conceals the unanswered requests; reporting only 76% conceals the role of safe abstention.

The appropriate trade-off depends on severity. High-risk domains may prefer abstention whenever sources conflict, whereas low-risk drafting applications can tolerate more exploratory output. A report should therefore present all three numbers, together with the rate of unnecessary refusals on questions the documents do answer.

## Failure, recovery, and justified diagnosis

A controlled failure-and-recovery experiment begins with an established baseline, introduces one deliberate fault, and then restores the system. If the expected behavior passes, fails under the fault, and passes after recovery, the experiment shows that the evaluation can detect the induced defect and recognize restoration. It does not prove that the evaluator will diagnose every unknown failure in every customer system. That broader claim requires more diverse experiments and independent acceptance evidence.

A good diagnostic report should separate observed symptoms, candidate explanations, supporting evidence, contradictory evidence, and unresolved uncertainty. It should also state which experiment would distinguish remaining hypotheses. For AI Test Lab, this is an important direction: move beyond a failure label toward evidence-based causal analysis and independent verification of repairs. The scientific value lies in claims that survive scrutiny, not in the confidence of the report's language.

## Worked explanation — Separating symptoms from causes

An assistant omits an air-drying instruction. Retrieval may have returned nothing, context assembly may have dropped the passage, or generation may have ignored it. The final answer alone cannot discriminate among these causes. Retrieval traces and controlled fault experiments are needed before asserting a root cause.

---

[← Previous](10-ai-judges-and-automated-quality-assessment.md) · [Contents](../README.md#contents) · [Next →](12-testing-retrieval-systems-and-ai-agents.md)
