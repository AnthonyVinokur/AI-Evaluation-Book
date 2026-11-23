# Chapter 14. Test Automation, CI/CD, and Regression Engineering

## Continuous evaluation is the AI counterpart of regression testing

Traditional continuous integration and continuous delivery — CI/CD — protects deterministic contracts such as schemas and API status codes. AI evaluation extends this protection to probabilistic properties such as groundedness, task success, and robustness. The challenge is to avoid treating every random textual difference as a failure while still detecting meaningful regressions.

A practical pipeline separates fast deterministic gates from slower sampled behavioral evaluations and targeted adversarial suites. Each layer has a defined purpose. An evaluation suite becomes an engineering control when its failures produce actionable evidence and release decisions — not merely a dashboard of attractive percentages.

## The engineering habit of proving a claim

A strong technical education encourages a distinction between «доказано» (proved), «проверено» (tested or checked), and «предполагается» (assumed). In software engineering these words should not be used interchangeably. A passing test demonstrates that particular assertions held under particular conditions. It does not constitute a mathematical proof that every possible input will behave correctly. Nor does a green continuous-integration dashboard establish that a customer deployment is ready.

For an AI system this distinction is even more important. A repeatable baseline–fault–recovery experiment can demonstrate sensitivity to an induced defect. It does not establish that the system can identify every unknown defect. The value of the experiment lies in the narrowness and precision of its claim. Good engineering is not merely about producing evidence; it is about refusing to claim more than the evidence supports.

## What still works from conventional testing

Conventional tests remain essential in AI applications, and they should run first because they are fast, cheap, and exact. Schema validation, authorization, parsing, HTTP contracts, and deterministic business rules can all be checked with ordinary assertions. A refund calculation still has one correct value. An unauthorized request must still be denied. A JSON response must still parse.

The familiar levels of testing also carry over. Component tests isolate mechanisms such as a chunking function or a retrieval filter. Integration tests verify the boundaries between components, such as whether the retriever's output reaches the prompt assembler intact. End-to-end tests exercise the deployed workflow through its real interface. Tools such as pytest and Playwright remain useful at every level — but observing an answer in a browser does not establish its factual correctness. The end-to-end test confirms the answer appeared; a separate oracle must decide whether it was right.

## What is new: versioned identity and probabilistic oracles

Two things change when a learned component enters the pipeline.

First, the identity of the system under test expands. A regression in conventional software can usually be traced to a code commit. In an AI application, behavior also depends on the model identifier, the prompt revision, the retrieval corpus and its index, the decoding settings, the tool definitions, and the evaluator's own configuration. Every evaluation run must record all of them. Otherwise a changed score cannot be explained, and two runs cannot be fairly compared.

Second, many oracles become statistical. A behavioral property such as faithfulness is measured as a rate over a sample, with the uncertainty described in Chapters 3 and 9. Repeated trials estimate variable behavior, and paired comparisons — running the same cases through both the old and the new version — expose which individual cases changed.

## A layered pipeline

These considerations lead naturally to a layered design, in which each layer runs at a frequency matched to its cost and speed.

| Layer | Runs | Examples | Typical duration |
|---|---|---|---|
| Deterministic checks | Every commit | Schemas, authorization rules, parsers, evidence digests | Seconds to minutes |
| Behavioral evaluation | Candidate builds, nightly | Faithfulness, task success, robustness samples | Tens of minutes |
| Adversarial suites | Before release, after security-relevant changes | Prompt injection, data exfiltration, permission boundaries | Tens of minutes to hours |
| Deployment acceptance | Release candidates | Live end-to-end runs against the target environment | Hours |

Fast deterministic checks give developers immediate feedback without spending money on model calls. Expensive live evaluations are reserved for changes that could affect behavior and for release candidates. A prompt change triggers behavioral evaluation; a change to tool permissions triggers the adversarial suite; a change to documentation does neither.

## Flaky tests and nondeterminism

In conventional CI, a flaky test — one that passes and fails without any code change — is a defect to be fixed. In AI evaluation, some variation is genuine behavior of the system under test. The response is not to suppress it but to measure it. A behavioral check should run several trials per case and report a pass rate, with the number of trials chosen in advance. A case that passes four of five times is not "passing"; it is a case with an estimated failure rate of about 20%, and that rate may be acceptable or not depending on severity.

Setting the temperature to zero can reduce variation but rarely eliminates it, because serving infrastructure, batching, and model updates can change outputs. It also hides the variation users would actually experience if production runs at a higher temperature. Evaluation settings should match production settings unless there is a stated reason to differ.

## Release gates

A release gate is a decision rule applied to evaluation results. It should be written before the results are seen, so that thresholds cannot be adjusted after the fact to favor a preferred version. A sound gate combines two kinds of condition. Hard constraints must hold without exception: no confirmed cross-tenant disclosure, no unauthorized tool call, no invalid evidence record. Statistical criteria apply to behavioral properties: for example, faithfulness must not fall by more than a stated margin relative to the baseline, measured with paired comparison and an explicit interval.

The gate should also allow a third outcome besides pass and fail: insufficient evidence. If a behavioral suite was too small to resolve the question, the honest result is to say so and gather more data, not to manufacture a definitive verdict.

## Worked explanation — Reading a regression honestly

Suppose a baseline passes 180 of 200 cases and a candidate passes 188 of the same cases. The observed gain is 4 percentage points, but the totals alone do not reveal what happened. Two very different situations produce the same totals.

In the first, the candidate fixes 8 failures and breaks nothing. In the second, it fixes 20 failures and introduces 12 new ones. The net change is identical — plus 8 cases — but the second release carries 12 fresh regressions, and if any of them is critical, the release should not ship regardless of the improved average.

Only a paired, case-level comparison distinguishes these situations. Using the paired method from Chapter 9, the first situation has a mean difference of 0.04 with a standard error of about 0.014, giving a rough 95% interval of 1.3 to 6.7 points — a clear improvement. The second has the same mean difference but a standard error of about 0.028, giving an interval of −1.5 to 9.5 points — not distinguishable from no change. The same headline number can therefore support a confident release in one case and require further investigation in the other. Recording the prompt, dataset, model, evaluator, and application revisions makes it possible to trace each changed case to its cause rather than guessing.

---

[← Previous](13-ai-security-and-trust-boundaries.md) · [Contents](../README.md#contents) · [Next →](15-performance-inference-optimization-and-cost.md)
