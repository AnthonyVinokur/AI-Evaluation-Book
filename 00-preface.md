# Preface — Understanding Before Engineering

Artificial intelligence is easy to encounter and difficult to understand. A language model can answer a question in fluent prose, but fluency does not reveal which information was learned during training, which facts came from external sources, or whether the output is dependable. A serious education in AI must begin with the mechanisms that produce model behavior rather than with the convenience of a chat interface.

This book begins with the idea of learning from examples. It proceeds to probability, neural networks, and modern foundation models before addressing the engineering of retrieval systems, agents, and production applications. The sequence is deliberate. Without understanding how a model learns and generates, the techniques used to improve or evaluate it become recipes whose limits are difficult to recognize.

## How this book is organized

Part I, *Foundations of Artificial Intelligence*, explains how models learn, why probability is central to their behavior, and how transformers and foundation models are trained and assembled into applications. Part II, *Evaluating AI Systems*, treats evaluation as a measurement discipline: validity, statistics, automated judges, hallucination and robustness testing, and the evaluation of retrieval systems and agents. Part III, *Engineering, Operations, and Practice*, turns these principles into working practice — security, regression pipelines, performance and cost, governance, platform architecture, evidence, commercial pilots, and the future of the field. The final chapter draws the threads together. A mathematical reference in the appendix collects the book's formulas in one place.

Every chapter ends with a worked explanation: a small calculation or a carefully reasoned case that makes the chapter's central mechanism visible. Those examples are deliberately simplified. In a deployed system the same reasoning must be extended to uncertain inputs, versioned configurations, imperfect evidence, and real operational consequences. The discipline remains the same: define the property, select the measurement, observe behavior under controlled conditions, and limit the conclusion to what the evidence actually supports.

## A note on the Russian mathematical tradition

This book draws on a way of learning familiar to many students educated in Russia: begin with the definition, derive the mechanism, test it against a concrete example, and only then discuss its application. The Russian phrase «от простого к сложному» — from the simple to the complex — captures the intended progression. A formula is not decoration; it is a compact statement whose assumptions should be understood.

Occasional Russian terminology appears alongside English, not in place of it. The language of contemporary AI engineering is largely English, and learning its technical vocabulary is essential. But a second mathematical language can provide an additional intuition: «обучение» for training, «обобщение» for generalization, and «погрешность» for measurement error. The aim is a rigorous, readable English-language book with a subtle Russian educational character.

## A recurring case study: AI Test Lab

AI Test Lab is an independent AI evaluation platform: software that runs defined test cases against a customer's AI application through authorized interfaces, records what the application did, scores the results, and preserves the evidence in tamper-evident form. It appears throughout the book as a practical example, not as the central subject. Its value here is to illustrate why an independent evaluator must distinguish model behavior from application behavior, and a test record from scientific evidence of reliability. Where the book describes what such a platform should do, it describes engineering principles, not claims about any product's current capabilities.

---

[Contents](../README.md#contents) · [Next →](01-the-nature-of-artificial-intelligence.md)
