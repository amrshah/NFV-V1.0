# AI Gateway Repository Inventory

**Assessment date:** 2026-10-15  
**Status:** Initial read-only inspection; not a full code/security audit.  
**Purpose:** Avoid duplicate work and select a reuse/adaptation path for the NFV AI Gateway pilot.

## Initial candidates found

| Repository | What initial inspection indicates | Initial disposition | What must be verified |
|---|---|---|---|
| [amrshah/Alamia-AI-Bridge](https://github.com/amrshah/Alamia-AI-Bridge) | `bridge.py` is a CLI-oriented helper, not an HTTP gateway. It routes aliases to OpenRouter/Groq/SambaNova and has some provider-selection fallback, but returns errors as strings and writes output to `bridge_result.txt`. | **Reuse small ideas only / inspect** | No server/API, auth, metering, budgets, tests or production failure semantics were evident in the inspected file. Verify license and tests if any before reuse. |
| [AlamiaSoft/VPS-AI-Bridge-Standalone](https://github.com/AlamiaSoft/VPS-AI-Bridge-Standalone) | `server.py` exposes OpenAI-compatible endpoints and aliases via FastAPI. Initial source review found no authentication check on `/v1/chat/completions` or `/v1/models`; route selection chooses a provider but does not appear to retry a different provider after an upstream runtime failure; JSON error handling appears to convert upstream failures to HTTP 500; streaming errors are emitted as SSE data. The inspected tree had no tests and requirements are unpinned. | **Prototype only; not production-ready** | Add auth/identity, quotas/rate limits, proper status/error semantics, real tested failover, request limits, usage/cost attribution, structured/redacted logs, dependency pins/scanning, tests and deployment hardening. Verify license. |
| [AlamiaSoft/alamia-ai-hub](https://github.com/AlamiaSoft/alamia-ai-hub) | Found a Cloudflare-specific frontend service using Workers AI through an AI Gateway route. | **Inspect separately; not automatically the shared gateway** | Scope, API boundaries, environment/secret management, deployment and whether this is product-specific UI code rather than a general provider gateway. |
| [AlamiaSoft/AlamiaAICopilot-Starter](https://github.com/AlamiaSoft/AlamiaAICopilot-Starter) | `core/ai/provider.py` defines `AIModelProvider`; `docs/ai-provider-adapter.md` describes `AIModelRouter` and selection requirements. Architecture separates provider adapters from the Copilot core. | **Keep as consumer abstraction; add adapter if gateway proves useful** | Actual router/provider implementations, test results, provider capability handling and current consumers. Avoid duplicate retry/fallback loops. |

## Source-level findings from initial code inspection

### VPS-AI-Bridge-Standalone
The following are findings from the current `server.py` and repository tree, not runtime test results:
- No auth dependency/middleware is visible on the model endpoints. Do not expose this service to the internet or trusted multi-product network until authenticated access is enforced.
- Alias-based provider selection is present, but no runtime failover to a second provider is visible after the selected upstream fails. README fallback claims need a failure-injection test.
- JSON upstream errors appear to be wrapped as HTTP 500 by broad exception handling, which obscures provider status and failure class.
- The stream path logs upstream error bodies and sends error details to clients; sanitize logs and normalize errors.
- No tests were visible in the inspected repository tree, and dependency versions are not pinned.

### Alamia-AI-Bridge
The inspected `bridge.py` is a CLI helper rather than a server; it has no shared API, tenant/client auth, metering or budget control. Treat it as a source of small routing ideas, not as a deployable shared gateway.

## Security finding — urgent

During inspection, `AlamiaSoft/alamia-ai-hub` path `reference-app-ui/services/aiGatewayService.ts` contained a hardcoded value named `API_TOKEN` alongside a Cloudflare account/gateway identifier, used in an Authorization header.

Treat this as potential credential exposure:
1. Revoke/rotate the token immediately if it is or was valid; do not paste the value into issues, chat or commits.
2. Check provider/account audit logs for unexpected use.
3. Move credentials to server-side secret configuration; never ship a privileged token to browser code.
4. Remove the hardcoded value from the current source and review Git history/artifacts for exposure. History rewriting does not replace revocation.
5. Confirm the browser only calls a trusted server-side endpoint and that endpoint enforces authentication, authorization, rate limits and data minimization.

This finding is based on source inspection; I have not verified whether the token remains valid or what permissions it has. No token value is reproduced in this document.

## Preliminary recommendation

Do **not** create a fifth gateway implementation yet.

1. Compare the actual source and tests of the two bridge repositories.
2. Select the smaller maintainable implementation as a baseline if it passes security and correctness checks; otherwise adopt only reusable parts.
3. Trial LiteLLM as a comparator, not a foregone conclusion. It may supply robust routing, retries/fallbacks, virtual keys, budgets and spend tracking, but it also introduces its own operational surface and must be tested for exact-version license, dependencies, resource use and provider compatibility.
4. Preserve a small Alamia-owned product-facing contract. Avoid duplicating a custom router inside each product or adding competing retries at both gateway and client layers.
5. Treat gateway authentication and model budget enforcement separately from application authorization: the gateway must not become an unrestricted route to business data or tools.

## Comparison checklist

For each bridge and LiteLLM trial, record PASS/FAIL/UNKNOWN for:
- Authentication is enforced and client keys cannot be ignored.
- Credentials are server-side and absent from source, images, logs and browser bundles.
- Provider/model aliases and unsupported capabilities are handled explicitly.
- Primary timeout, rate-limit, 5xx, malformed-response and all-providers-down cases are tested.
- Retry/fallback limits prevent loops and unexpected duplicate spend.
- Every request can be attributed to product/feature from trusted identity, with usage/cost/error data.
- Sensitive prompts/responses are not logged by default.
- Provider-specific quality, privacy, pricing, limits and terms are checked for the selected workloads.
- Tenant/application keys, quotas and rate limits are isolated.
- Health, correlation IDs, safe error handling, recovery and deployment rollback are documented.
- Exact version, license, dependency/security scan and resource benchmark are recorded.
- There is a clear exit/migration plan.

## Scope discipline

The first pilot should demonstrate a shared API, tested fallback, trustworthy usage attribution, secure credential handling, two consuming integrations and a manageable deployment. It should not start with a full billing platform, vector database, agent marketplace or additional observability stack unless tests show a real need.
