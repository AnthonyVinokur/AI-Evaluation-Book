# Chapter 18. Evaluation Execution, Evidence, and Verification

## What an evidence package must demonstrate

An evidence package is the durable output of an evaluation: the collection of records that allows someone who was not present to understand what was tested, what happened, and why the verdict was reached. It should connect a specific execution to its application identity, test dataset, policy, evaluator configuration, observed outputs, and final decision. A successful internal verification of these links establishes integrity within that system's trust assumptions; it does not automatically establish customer acceptance or independent witnessing.

## A disciplined execution sequence

A well-run evaluation follows a fixed sequence, and each step produces a record.

**Prepare.** Confirm authorization to test the target. Record the deployment identity, the dataset version, the policy and rubric versions, and the expected conditions. State the decision rule before any results are seen.

**Execute.** Submit the cases. Capture requests, responses, timing, and authorized telemetry exactly as received.

**Validate.** Check that the execution itself was sound: that the intended deployment was reached, that no cases were silently skipped, and that injected faults actually took effect.

**Score.** Apply the oracles to the stored observations.

**Seal.** Compute digests of the records and store them where they cannot be altered without detection.

**Report.** State the conclusions, their uncertainty, and their limits.

The validation step is easily overlooked. An evaluation that believed it was testing a new release while actually reaching a cached older deployment has produced a well-documented record of the wrong system.

## The contents of an evidence package

A complete package typically contains the following elements: a manifest listing every file with its digest; the execution identity, a unique identifier for this run; the target identity, including any version information the deployment exposes; the dataset version and the exact cases executed; the policy, rubric, and judge configuration; every request and response; authorized telemetry such as retrieval traces and tool calls; each oracle's verdict, kept separate rather than merged; the decision rule and the resulting decision; and a statement of limitations, including anything that could not be observed.

Keeping each oracle's verdict separate, as recommended in Chapter 9, is what makes later diagnosis possible. A merged score of "fail" says nothing about whether the schema, the boundary rule, or the factual content was at fault.

## Controlled fault injection

A baseline–fault–recovery experiment tests whether an evaluation can detect a known defect. It has three phases. First, a baseline run establishes that the requirement passes under normal conditions. Second, a controlled fault is introduced — for example, the retriever is made to return no passages — and the run is repeated; the requirement should now fail. Third, the fault is removed and the run is repeated again; the requirement should pass once more.

This sequence demonstrates sensitivity to the injected fault under the tested conditions. It does not prove that the evaluation can identify all unknown failures or infer root causes from final answers alone. Its strength comes from its narrowness: one deliberate change, one expected effect, and an observed restoration.

A defensible evidence chain binds the three observations to a single execution identity and a verified deployment identity, so that no one can substitute a baseline from a different system or a recovery run from a different day. Tampering with any record should invalidate verification rather than silently replace history.

## Separate properties, separate claims

Several properties are easily conflated in an evaluation report. Integrity means the records are unchanged. Deployment continuity means the same target was tested throughout. Authorization means the testing was permitted. Oracle correctness means the verdicts are right. Independent review means someone outside the evaluation team examined the work. Customer acceptance means the customer agreed that defined criteria were met.

Each requires its own evidence. A hash cannot establish factual truth. A signature cannot repair a flawed rubric. Customer acceptance requires agreed criteria and an explicit decision by the customer. A credible report distinguishes what was observed from what was inferred, and preserves inconclusive outcomes when evidence is insufficient rather than rounding them up to a pass or down to a fail.

## Worked explanation — Reading a fault-injection result

An evaluation of a policy assistant produces the following sequence on a single case: baseline PASS, fault FAIL, recovery PASS. The package shows one execution identity across all three phases, the same deployment version in each, and a retrieval trace confirming that the fault phase received zero passages while the other phases received the expected passage.

What may be claimed: under the recorded conditions, the evaluation detected the absence of retrieved evidence, and the application's behavior returned to baseline when the evidence was restored.

What may not be claimed: that the evaluation would detect other kinds of failure; that the application is reliable in general; or that every failure in production would be correctly diagnosed. If the recovery phase had instead shown a different deployment version, even the narrow claim would fall, because the restoration could not be attributed to the removal of the fault.

Independent execution identity, tamper-evident records, and explicit limitations are what make the narrow conclusion credible — and the discipline of stating only that conclusion is what makes the evaluation trustworthy.

---

[← Previous](17-architecture-of-an-independent-ai-evaluation-platform.md) · [Contents](../README.md#contents) · [Next →](19-customer-integration-and-commercial-pilot-readiness.md)
