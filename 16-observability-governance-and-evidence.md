# Chapter 16. Observability, Governance, and Evidence

## Trustworthy evidence is necessary but not sufficient

An evaluation result is only as useful as the record that supports it. If no one can later establish which application version was tested, which cases were used, what the system actually returned, and how the verdict was reached, the result cannot be audited, compared, or defended. Observability makes behavior visible; governance makes the resulting evidence trustworthy and usable.

Yet trustworthy evidence is not the same as a true conclusion. A record can be perfectly preserved and still describe a flawed experiment. The Russian distinction between «проверено» (checked) and «доказано» (proved) is useful here. A successful check supports a bounded conclusion under specified assumptions. Governance should preserve that conclusion accurately, including its uncertainty, rather than transform it into a universal assurance claim.

## Logs, metrics, and traces

Observability in conventional software rests on three kinds of data. Logs record discrete events. Metrics aggregate measurements over time, such as request counts and latency percentiles. Traces follow a single request through every component it touches, so that a slow or failing step can be located.

AI applications need all three, with additional content. A useful trace for a retrieval-augmented request records the original query and any rewritten query, the identifiers and versions of the candidate and selected passages, the prompt revision, the model identifier and decoding settings, each tool call with its arguments and result, each authorization decision, the output, and any validation verdicts. With such a trace, the diagnostic questions of Chapters 11 and 12 — was the passage retrieved, was it included, was it used — can be answered from evidence instead of inferred.

A trace does not, however, reveal why a model generated a particular token. It shows what went into the model and what came out. Explanations of internal reasoning remain hypotheses, and reports should present them as such.

## Monitoring for drift

A system that passed evaluation last month may not pass it today. Documents are added and revised, users change how they phrase requests, and hosted models are updated by their providers. Drift monitoring compares current behavior with an established reference. Useful signals include the distribution of input lengths and topics, the rate of abstentions and refusals, the share of answers that cite retrieved evidence, judge-assessed faithfulness on a sampled stream, and the rate of user corrections or escalations.

A shift in any of these is a prompt for investigation, not a verdict. A rising abstention rate may indicate a retrieval failure — or a newly added class of questions that the documents genuinely do not answer. Monitoring tells the engineer where to look; controlled evaluation then establishes what happened.

## Privacy and data minimization

Traces of AI applications often contain the most sensitive data a system handles: customer questions, retrieved internal documents, and personal records returned by tools. Observability must therefore be designed with the same care as the application itself. Sensitive fields should be minimized or redacted at capture time where the diagnostic value permits, access to traces should be restricted to those with a defined need, and retention periods should be set and enforced. An evaluation provider with gray-box access holds a confidentiality obligation for everything it collects. Collecting less, deliberately, is a design virtue.

## Integrity, authenticity, and validity

Three separate questions arise about any piece of evidence.

Integrity asks whether the record has changed since it was created. A cryptographic hash function such as SHA-256 maps a sequence of bytes $B$ to a fixed-length digest $H(B)$. Changing even one byte produces a completely different digest. If the digest of a stored evaluation record is recorded somewhere trustworthy at the time of evaluation, any later alteration can be detected by recomputing it. But a digest can detect changes only when compared against a trusted expected value; a digest stored alongside the record it protects can be replaced together with the record.

Authenticity asks who created the record. A digital signature binds a record to a cryptographic key. Its credibility depends on who controls that key and what role they actually played. A signature binds evidence to a key, not automatically to an independent witness.

Validity asks whether the record's conclusions are correct. This is the subject of Chapter 8, and no cryptographic mechanism addresses it. A perfectly preserved false conclusion remains false; a flawless signature cannot repair a flawed rubric.

## Provenance

Provenance connects a result to everything that produced it: the execution identity, the target application revision, the test dataset version, the evaluator and judge configuration, and the time of execution. With complete provenance, two results can be fairly compared and a changed result can be traced to its cause. Without it, the question "did the model get worse, or did we change the test?" has no answer.

A practical rule follows. Missing telemetry should be described as unobserved, not inferred. If an evaluation could not see retrieval traces, the report should say that retrieval behavior was not observed — not that retrieval worked.

## Governance frameworks

Organizational frameworks provide structure around these practices. The NIST AI Risk Management Framework organizes AI risk work into four functions — govern, map, measure, and manage — and its Generative AI Profile adds guidance specific to generative systems. ISO/IEC 42001 specifies requirements for an AI management system that an organization can be audited against. These frameworks support organizational risk management: defined responsibilities, documented processes, and continual improvement. They do not, by themselves, guarantee the correctness of any individual output, and conformance to a framework is not evidence that a particular application is reliable. Their value lies in ensuring that the measurements described in this book are performed, recorded, reviewed, and acted upon.

## Worked explanation — Integrity is not truth

An evaluator records that a support assistant correctly stated a 30-day return window, stores the full record, and computes its SHA-256 digest. The digest is written to a separate, append-only log controlled by a different team.

Six months later, an auditor recomputes the digest and finds it matches. This establishes integrity: the record has not changed since it was sealed. Now suppose the auditor also discovers that the policy actually specified 14 days, and the evaluator's reference answer was wrong. The record is intact and authentic — and its conclusion is false.

Consider the reverse. The evaluation was sound, but someone later edited the record to change a failure into a pass. Recomputing the digest exposes the mismatch, and the altered record is rejected. Here integrity protection did exactly its job.

The two cases together show the boundaries. Provenance, validity, custody, and independent verification are distinct layers of assurance, and a trustworthy evaluation needs all of them.

---

[← Previous](15-performance-inference-optimization-and-cost.md) · [Contents](../README.md#contents) · [Next →](17-architecture-of-an-independent-ai-evaluation-platform.md)
