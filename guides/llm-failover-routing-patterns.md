# LLM Failover and Routing Patterns for OpenAI-Compatible APIs

Teams often say they want "LLM failover" when what they actually need is a routing policy that does not turn one provider problem into a full application outage.

The important distinction is this:
- retries try the same path again
- fallback chooses a different path
- routing decides which path should be attempted in the first place

If you operate against OpenAI-compatible endpoints, this is where the real production work begins.

## 1. Fallback is not the same as blind retry

A retry loop can make a partial outage worse if the primary backend is unhealthy and every request keeps piling onto the same failing route.

A healthier pattern is:
1. classify the failure
2. decide whether the failure is retryable
3. decide whether the request should move to a fallback model or provider
4. log which route served the final response

Good fallback targets are usually explicit, not dynamic magic.

## 2. Failure classes should map to different routing decisions

Not every error should trigger failover.

Examples:
- `429` or temporary upstream overload → maybe retry, maybe degrade to a cheaper/faster backup
- timeout or connection failure → fail over quickly if latency matters
- malformed request or schema mismatch → do not fail over; fix the caller
- tool-calling incompatibility → route only to models known to support that behavior

This is why production routing logic should separate provider-availability failures from caller-side mistakes.

## 3. Health-aware routing reduces blast radius

The most useful health check is not a huge benchmark prompt.
It is a cheap probe that verifies the route can still answer within a short budget.

A simple pattern:
- keep a primary route for normal traffic
- probe candidate routes on a short interval
- temporarily remove unhealthy routes from selection
- gradually restore them after recovery

That turns failover from a panic behavior into a normal operating mode.

## 4. Latency-tier routing works better than one-model-for-everything

Many workloads do not need your strongest or most expensive model on the first attempt.

A common production split is:
- fast/cheap tier for summarization, tagging, extraction, internal automation
- stronger tier for complex synthesis, user-facing reasoning, or escalation paths

This kind of tiering matters because it improves both cost control and incident behavior.
When the strong tier is degraded, you can keep low-risk workloads alive instead of failing everything together.

## 5. Observability is part of the routing design

If you cannot answer these questions, your routing layer is too opaque:
- which route served the request?
- how often did fallback happen?
- which workloads trigger the most retries?
- where did latency jump first?
- which models are creating cost spikes?

A routing policy without route-level logs becomes very hard to tune once traffic grows.

## 6. OpenAI-compatible APIs lower integration friction, not verification work

Compatibility helps because many apps can keep the same SDK surface and only change:
- API key
- base URL
- model mapping

But the production behavior still needs testing around:
- streaming
- timeouts
- response structure assumptions
- tool/function calling
- regional latency

That is the part many “just swap the base URL” demos leave out.

## Practical next steps

If you want a code-first starting point, see:
- `llm-failover-router-demo`
- `xidao-python-examples`

If you want the migration and rollout side, see:
- `guides/openai-compatible-rollout-checklist.md`
- `guides/migrate-from-openai.md`

## Product context

We ran into these issues while building XiDao API, an OpenAI-compatible gateway. The recurring lesson is that routing quality is not just a cost feature. It is one of the main ways teams reduce outage blast radius while keeping migration friction low.
