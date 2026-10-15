# AI Gateway NFV Pilot Plan

**Status:** Proposed pilot; architecture and OSS choices are not yet production-approved.  
**Purpose:** Validate NFV-RS against a reusable, measurable capability for multiple Alamia products.

## Decision in brief

Use the Alamia AI Gateway as the first NFV pilot because it has cross-product leverage and measurable outcomes. Start with the smallest viable provider-agnostic inference boundary, then add OSS only where it removes more complexity than it introduces.

The pilot is not a mandate to deploy a large AI platform. It must prove:
1. At least two product integrations can use one stable internal API.
2. Provider failure and rate limits produce controlled, tested behavior.
3. Usage can be attributed to product/feature and cost measured without unsafe prompt logging.
4. Model selection can be changed without rewriting business logic.
5. Security, deployment, observability and recovery meet the NFV critical gates.

## Scope for the first increment

### Include
- One OpenAI-compatible internal API surface where it covers the required workload.
- Semantic model aliases owned by Alamia (for example, a task-level alias rather than provider model IDs in product code).
- A narrow initial set of workloads: text generation and structured outputs; add embeddings/tools only when a consuming product requires them.
- Provider adapters for the initial primary route and one tested fallback. Current preference: evaluate OpenRouter as primary and Groq as fallback for compatible tasks; local Ollama is useful for development/testing. These are choices to benchmark, not hard-coded assumptions.
- Bounded timeout/retry/fallback behavior, per-product/feature attribution, usage/cost records, request correlation IDs and health/diagnostic endpoints.
- Secret management, authentication between products and gateway, rate limits, sensible request-size limits, redaction and safe error responses.
- Automated smoke, contract, failure-injection and regression tests.
- A thin integration example for Alamia Accounts Copilot and Kamal Express, once their actual API needs are confirmed.

### Exclude initially
- Building a model registry or training/fine-tuning platform.
- A separate agent gateway or A2A coordination platform.
- A vector database or knowledge platform.
- A full billing/chargeback product.
- An extra observability stack unless the gateway's built-in telemetry and existing monitoring are insufficient.
- Complex automatic quality-based routing before representative evaluation data exists.
- Unbounded autonomous tool execution.

## OSS-first candidate decision

### LiteLLM — first router candidate, TRIAL
Official docs describe a common API/proxy, provider routing, retries/fallbacks, virtual keys, budgets/rate limits, spend tracking and logging integrations. That aligns with the pilot. The project also documents a proxy architecture involving authentication/budget checks, a database, optional Redis-backed counters/cache, routing and provider adapters.

Before adopting a specific release:
- Pin version/image by immutable digest and inspect license/dependencies/advisories.
- Verify which desired features are in the OSS release versus commercial offerings.
- Test OpenRouter and Groq compatibility for selected models and endpoints.
- Measure memory/CPU, latency overhead, persistence needs and operational complexity on the intended deployment.
- Confirm tenant/product attribution, key isolation, rate limits, budget behavior and failure semantics.
- Verify that telemetry can be configured to avoid storing prompts/responses unnecessarily.
- Define how product applications behave if the gateway is down.

Do not wrap every LiteLLM feature in our own abstraction. Keep a small Alamia-owned contract for the product-facing calls we actually use; expose provider-specific features only through explicit capabilities.

### Langfuse — defer pending need
Potentially useful for traces, prompt experiments and evaluation. Self-hosting includes multiple application and storage components (including PostgreSQL, ClickHouse, Redis/Valkey and object storage in the documented architecture). Do not deploy it in the first increment if structured gateway metrics, application logs and a small evaluation harness meet our needs. Re-evaluate when trace-level debugging or repeatable evaluation materially improves development.

### Cloudflare AI Gateway / Workers AI — optional comparison
Treat as a managed/platform-specific option, not a required OSS dependency. Compare actual workload cost, data handling, provider coverage, latency and portability before use.

### OpenRouter, Groq and Ollama — routes, not OSS components
- OpenRouter and Groq are hosted service options. Check current model terms, price, rate limits, privacy/data handling and reliability.
- Ollama is a local development/testing route; model weights have separate licenses. Do not assume local results match production provider quality.

## Proposed logical flow

Product → authenticated Alamia gateway contract → routing policy → chosen provider adapter → response.

Cross-cutting: request ID, product/feature identity, authorized model alias, limits, usage/cost record, redaction policy, outcome/error telemetry.

The gateway is not the source of truth for product permissions. The consuming application authorizes the user and requested business operation. Gateway credentials identify the calling application/workload and constrain model access; they do not grant access to arbitrary tenant records or tools.

## Minimum contracts

### Request metadata
- product/application ID and feature ID from authenticated server-side identity, not trusted arbitrary client claims
- request/correlation ID
- approved semantic model alias
- operation/endpoint, token/input limits, timeout and optional deadline
- data classification/redaction policy
- optional evaluation or experiment tag

### Response/record
- request ID, resolved provider/model/version where available
- status and normalized error category
- latency, token/usage counts and cost estimate with source/accuracy label
- fallback/retry information
- structured result or provider response compatible with the contract

Never log provider secrets. Avoid storing full prompts/completions by default. If payload logging is explicitly needed for debugging/evaluation, it must be opt-in, scoped, redacted, access-controlled and time-limited.

## Routing policy

Start with explicit, deterministic routing rules:
1. Route by approved semantic alias and task requirements.
2. Consider provider/model capability, privacy constraints, availability, cost and latency.
3. Retry only bounded transient errors; do not retry non-idempotent side effects blindly.
4. Fall back only to a compatible model/endpoint with acceptable quality and data policy.
5. Do not silently downgrade quality-critical work without an explicit policy.
6. Fail clearly when no eligible provider is available.
7. Record the chosen route and why, without leaking secrets or sensitive content.

Do not claim a fallback works until automated tests deliberately make the primary provider fail and verify the actual secondary response, usage record and client-visible behavior.

## Validation gates and acceptance criteria

### Security / privacy
- Product credentials are isolated and rotatable.
- Unauthorized callers cannot invoke the gateway.
- One product cannot use another product's key, budget or usage record.
- Provider credentials never reach clients or logs.
- Prompt/response payloads are not retained by default unless a documented use case authorizes it.
- Request limits and rate limits are enforced.
- Critical dependencies and container images are reviewed/pinned.

### Reliability
- Primary success path works.
- Primary timeout, 429/rate limit, 5xx and invalid response paths are tested.
- Fallback succeeds for compatible tasks and does not loop indefinitely.
- If all providers fail, caller receives a bounded, normalized error.
- Gateway restart and persistence recovery are tested to the extent required by the selected architecture.
- Client retries cannot cause uncontrolled duplicate cost.

### Cost / quality
- Every request is attributable to product and feature.
- Usage and cost estimates are recorded; unknown pricing/usage is marked rather than fabricated.
- Benchmark at least representative simple and complex tasks against the baseline.
- Compare cost per successful outcome, latency, failure rate and task-specific quality—not token price alone.
- Budgets/quotas have a known enforcement point and are tested.

### Operability
- Health/readiness, structured logs and request correlation exist.
- Metrics distinguish provider errors from gateway errors and fallback attempts.
- A deployment/runbook, secret rotation, backup/recovery guidance and rollback path exist.
- Resource usage is measured on the intended host.
- An operator can diagnose a failed request from its ID without needing raw sensitive prompts.

### Adoption
- Two real consuming workflows use the gateway without embedding provider SDK logic in domain code.
- At least one provider-failure exercise passes.
- NFV critical gates are evidenced; the weighted score is not used to hide a failed gate.
- A short ADR records exact version, license review, deployment decision and exit path.

## Existing Alamia codebase finding (initial read-only inspection)

The private repository [AlamiaSoft/AlamiaAICopilot-Starter](https://github.com/AlamiaSoft/AlamiaAICopilot-Starter) already contains:
- `core/ai/provider.py`: an `AIModelProvider` abstraction with chat, streaming, structured-output and tool-calling methods.
- `docs/ai-provider-adapter.md`: describes an `AIModelRouter` that selects providers from requirements such as quality, local-only privacy and tool-calling needs.
- Its architecture separates the Copilot runtime from AI provider adapters and says core should not depend on a particular LLM.

This changes the implementation question. **Do not build a second custom router inside the gateway project.** The pilot should evaluate whether a separate shared service (LiteLLM or another candidate) adds enough central value—cross-product keys, budgets, usage/cost attribution, provider failover and operations—to justify another network hop and service to operate.

Recommended boundary:
- Keep `AIModelProvider` / `AIModelRouter` as the Copilot's in-process abstraction for provider capability selection and a testable local/mock path.
- If the shared gateway trial passes, implement one provider adapter/client in the Copilot Starter that calls the gateway's stable API.
- Keep product business rules, tenant-data authorization and tool permissions inside the consuming application.
- Avoid competing retry/fallback loops at both layers. Assign routing/retry ownership explicitly and test that it cannot create retry storms or duplicate spend.
- Inspect the current router implementation, provider adapters, tests and actual consuming-app requirements. A README test claim is not a substitute for running tests against the current commit.

This is an initial tree/document/code inspection, not a full repository audit or a claim that the existing router is production-ready.

## Suggested implementation sequence

1. Inspect the current Copilot Starter router/provider code and tests, plus any existing AI Gateway starter/repositories; do not duplicate work or start a competing repo.
2. Define the minimal Alamia request/response contract and threat/data-flow model.
3. Trial LiteLLM in a disposable environment with pinned version; measure resources and verify licensing/dependencies.
4. Configure one primary route and one fallback; add tests that force each failure mode.
5. Add usage/cost attribution and privacy-safe structured telemetry.
6. Integrate one low-risk workload from each of two products, after confirming their real API needs.
7. Run NFV assessment and update the component register with evidence.
8. Only then decide whether Langfuse, a managed gateway, a separate policy service or additional infrastructure is warranted.

## Success measures
- Provider switch requires configuration, not domain-code rewrite.
- Fallback tests prove the secondary path is exercised.
- Every request has an attributable usage record or a clearly classified telemetry failure.
- No critical tenant/auth/data leak is found in negative tests.
- Cost, latency, success rate and task quality can be compared across the baseline and candidate routes.
- Operational footprint is acceptable on the target deployment.
- Two consuming workflows are integrated without unnecessary new services.

## Sources to recheck at implementation time
- LiteLLM architecture: https://docs.litellm.ai/docs/proxy/architecture
- LiteLLM cost tracking: https://docs.litellm.ai/docs/proxy/cost_tracking
- LiteLLM logging and redaction options: https://docs.litellm.ai/docs/proxy/logging
- LiteLLM budget fallbacks: https://github.com/BerriAI/litellm-docs/blob/main/docs/proxy/budget_fallbacks.md
- Langfuse self-hosting: https://langfuse.com/self-hosting
- Langfuse self-hosted license features: https://langfuse.com/self-hosting/license-key

This plan is a proposed pilot, not proof that the components or architecture have been fully validated.
