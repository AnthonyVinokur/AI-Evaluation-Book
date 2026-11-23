# Chapter 15. Performance, Inference Optimization, and Cost

## Performance is part of model quality in production

An answer that arrives after the user's workflow has timed out is not operationally successful. Evaluation must therefore include time to first token, total latency, throughput, failure rate, and cost, not only textual quality. A correct answer delivered in forty seconds may be worse, for a customer-support chat, than a slightly less complete answer delivered in three.

Performance and quality also interact. A larger model may answer more accurately but more slowly. Retrieving more passages may improve grounding but lengthen the prompt and raise cost. Reasoning at inference time, as described in Chapter 6, can improve difficult answers while multiplying latency. Engineering decisions in production are rarely about maximizing one property; they are about choosing an acceptable point among several.

## The anatomy of latency

The latency a user experiences is the sum of several stages: network transport, authentication, retrieval, prompt assembly, the model's processing of the input, the generation of output tokens, any tool calls, and post-processing such as validation. Each stage can be measured separately, and each fails differently under load.

Two measurements describe the model's part. Time to first token, abbreviated TTFT, is the delay before the first output token appears. It is dominated by prefill — the processing of the entire input context — and therefore grows with prompt length. Time per output token, abbreviated TPOT, is the pace of generation once it has begun. It is dominated by decoding, which produces one token at a time. A simplified model of total generation time for $N$ output tokens is:

$$T \approx \mathrm{TTFT} + (N - 1) \times \mathrm{TPOT}$$

With a TTFT of 0.8 seconds, a TPOT of 0.02 seconds, and 400 output tokens, the estimate is $0.8 + 399 \times 0.02 \approx 8.8$ seconds. Streaming the response changes the experience substantially: the user starts reading after 0.8 seconds rather than waiting nearly nine.

## Averages hide the slowest users

A mean latency can conceal severe tail delays. Suppose 95 of 100 requests complete in 2 seconds and 5 take 30 seconds. The mean is 3.4 seconds, which sounds acceptable. The median is 2 seconds. But one user in twenty waits half a minute, and those users are often the ones with the longest documents or the most complex tasks — precisely the requests that matter.

Percentiles expose this. The 95th percentile, written p95, is the latency below which 95% of requests fall; p99 is the corresponding figure for 99%. Service objectives should be stated in these terms: for example, "p95 time to first token below 1.5 seconds at 20 concurrent users." A load test is meaningful only if it specifies concurrency, input and output token lengths, and cache conditions, because each of these changes the result.

## Memory, quantization, and serving

A lower bound on the memory needed to hold a model's weights is:

$$M \approx \frac{P \times b}{8} \text{ bytes}$$

where $P$ is the parameter count and $b$ is the number of bits per parameter. A 7-billion-parameter model stored at 16 bits per parameter needs about 14 gigabytes for its weights alone; at 4 bits it needs about 3.5 gigabytes. Serving also requires memory for activations and for the key-value cache, which grows with context length and with the number of concurrent requests.

Several techniques trade quality, complexity, and cost. Quantization stores weights at lower precision, reducing memory and often increasing speed, at some risk to accuracy that must be measured rather than assumed. Distillation trains a smaller model to imitate a larger one. Batching processes several requests together to use hardware more efficiently, but can increase queueing delay for each individual request. Each of these is a change to the system under test: a quantized model is a new model for evaluation purposes, and its behavior on the application's own cases should be compared with the original.

## Caching and its hazards

Caching avoids repeated computation. A response cache returns a stored answer when the same question recurs. A prompt or prefix cache reuses the processed form of a long system prompt shared across many requests, reducing time to first token. Both can produce large savings.

Both also introduce risks that belong in the evaluation plan. A response cache can return a stale answer after the underlying policy has changed. Worse, a cache keyed only on the question text can return one customer's personalized answer to another customer — a cross-tenant disclosure created by a performance optimization. Cache keys must include every factor that legitimately changes the answer, including the user's identity and permissions, and tests should deliberately probe for cache collisions.

## The economics of evaluation itself

Evaluation has its own cost structure, and an evaluation platform must measure it. If $n$ cases are each repeated $r$ times and each trial uses $j$ judge calls, the number of model calls is roughly:

$$C = n \times r \times (1 + j)$$

before retries. A suite of 500 cases, repeated 3 times, with 2 judge calls per trial, requires $500 \times 3 \times 3 = 4{,}500$ model calls per run. Run nightly, that is about 135,000 calls a month. Several judges and repeated runs can make evaluation more expensive than the application responses being evaluated. The useful measure is therefore not the number of tests executed but the cost per useful finding: how much it costs, on average, to discover a defect that matters.

## Worked explanation — Unit economics of a request

Suppose a hypothetical service charges \$2 per million input tokens and \$8 per million output tokens. These prices are illustrative, not a current vendor quotation. A request includes instructions, retrieved passages, and conversation history totaling 3,000 input tokens, and produces 500 output tokens. Its model-token cost is:

$$\frac{3{,}000}{1{,}000{,}000} \times 2 + \frac{500}{1{,}000{,}000} \times 8 = 0.006 + 0.004 = 0.010 \text{ dollars}$$

At 100,000 requests a month, this is \$1,000. Now suppose a design change retrieves eight passages instead of four, adding 1,500 input tokens per request. The cost rises to \$0.013 per request, or \$1,300 a month — a 30% increase from a change that might have been made for a modest quality gain. If the evaluation suite shows the larger context improves faithfulness by only one point, the trade may not be worthwhile; if it eliminates a class of serious errors, it may be cheap.

These figures exclude retrieval infrastructure, storage, logging, retries, monitoring, and human review. Unit economics depend on the complete system, not just advertised model prices, and every performance optimization must be evaluated for its effect on quality as well as on cost.

---

[← Previous](14-test-automation-ci-cd-and-regression-engineering.md) · [Contents](../README.md#contents) · [Next →](16-observability-governance-and-evidence.md)
