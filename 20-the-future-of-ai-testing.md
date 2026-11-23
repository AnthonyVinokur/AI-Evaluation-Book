# Chapter 20. The Future of AI Testing

## More capable AI increases the need for measurement

As models acquire longer context, multimodal inputs, tool use, and autonomous workflows, the space of possible failures grows. A benchmark focused on short textual answers cannot establish the reliability of an agent that changes external state over hours or days. New capabilities require new observables and new oracles, not merely more prompts.

The durable principle is empirical accountability: systems should be judged by their behavior under relevant conditions, with explicit uncertainty and consequences. Evaluation methods will evolve, but the need to distinguish capability claims from demonstrated reliability will remain.

## Reasoning at inference time

Reasoning-oriented models devote additional computation at inference time to difficult tasks: exploring alternative solutions, checking intermediate steps, or revising a draft before answering. This can improve accuracy on mathematics, coding, and multi-step analysis. It does not guarantee correct answers. More computation spent on a flawed approach can produce a longer and more persuasive wrong answer.

Two evaluation consequences follow. First, latency and cost become part of the quality trade-off in a new way, since the same model may spend very different amounts of computation on different requests. Second, a model's visible reasoning text should not be treated as a faithful record of how it reached its answer. It may be useful for diagnosis, but the verdict must still rest on the answer and its effects, checked against independent evidence.

## Long-running agents

The most significant change is the move from single responses to extended workflows. An agent may plan, call tools, update records, delegate tasks, and act over time. Its final message is not enough to determine whether it succeeded. The evaluator must inspect tool arguments, permission decisions, state transitions, task completion, and unintended side effects across the whole trajectory, as Chapter 12 described.

Duration introduces failure modes that short tests cannot reveal. Errors accumulate: a small misunderstanding early in a task can shape every later step. State can drift: a record the agent read at the start may have changed by the time it acts on it. Recovery becomes essential: an agent that encounters a failed tool call halfway through a long task must neither give up silently nor retry an irreversible action. And memory persists: what the agent retains between sessions must be tested for both correctness and leakage across users.

Future AI evaluation will therefore increasingly require stateful environments and controlled scenarios rather than isolated question-and-answer benchmarks. A simulated booking service, a sandboxed file system, or a synthetic customer database can expose these properties safely. Sandbox success does not automatically prove production reliability, but without such environments the behaviors that matter most cannot be observed at all.

## Multi-agent systems and shared protocols

Systems composed of several cooperating agents introduce further questions: which agent was responsible for a decision, which permissions were in effect at each step, and whether information passed between agents crossed a boundary it should not have. Standardized tool and agent protocols, such as the Model Context Protocol, simplify communication between components. They cannot guarantee that the components behave correctly or within their authority. As protocols become common, evaluation must check not only each component but the composition.

## Evaluating systems that exceed their evaluators

A harder problem lies further ahead. Automated judges, discussed in Chapter 10, are themselves models. As the systems being evaluated grow more capable, there may be tasks on which the judge is less competent than the system it judges, and tasks on which human experts are slower or less reliable than either.

Several approaches help. Tasks can be designed with verifiable outcomes — code that must pass tests, calculations with checkable results, actions whose effects can be inspected in an environment — so that the oracle does not depend on the judge's understanding. Complex outputs can be decomposed into smaller claims that are individually easier to verify. And the principle of calibration from Chapter 10 applies with greater force: a judge's reliability must be measured on the kind of case it will assess, not assumed from its general capability.

## What will not change

Model families and benchmarks will change. The enduring questions will not: what behavior was intended, what actually occurred, which evidence supports the conclusion, and what uncertainty remains. Mature evaluation will continue to combine exact assertions, statistical evidence, calibrated model judges, human review, and controlled environments. No single metric captures every aspect of general-purpose AI reliability. Clear objectives, bounded authority, observable behavior, reliable evidence, and calibrated conclusions are durable principles that should outlast dependence on any particular model version.

## Worked explanation — Long-running agent reliability

Suppose an agent completes a ten-step task, and each step succeeds with probability 0.9, independently of the others. If every step must succeed, the probability that the whole task succeeds is:

$$0.9^{10} \approx 0.349$$

An agent that looks reliable at each step — succeeding nine times out of ten — completes the full task only about 35% of the time. At 0.99 per step, ten-step success rises to about 90%; at 0.999, to about 99%.

Real steps are rarely independent: a confused agent tends to stay confused, and a well-designed one can recover from some errors. The calculation is therefore not a prediction. It illustrates how modest step-level failure rates accumulate over long workflows, and why checkpoints, bounded retries, idempotent actions, verification of intermediate state, and escalation to a human are architectural requirements rather than optional refinements.

---

[← Previous](19-customer-integration-and-commercial-pilot-readiness.md) · [Contents](../README.md#contents) · [Next →](21-synthesis-becoming-an-ai-engineering-specialist.md)
