# AI Gateway Repository Inventory

**Assessment date:** 2026-10-15  
**Status:** Initial read-only inspection; not a full code/security audit.  
**Purpose:** Avoid duplicate work and select a reuse/adaptation path for the NFV AI Gateway pilot.

## Initial candidates found

| Repository | What initial inspection indicates | Initial disposition | What must be verified |
|---|---|---|---|
| [amrshah/Alamia-AI-Bridge](https://github.com/amrshah/Alamia-AI-Bridge) | README describes a universal provider bridge for OpenRouter, Groq, SambaNova and Ollama; model aliases and fallback logic. Tree query showed README as the only path matching gateway/provider/router/model/usage-related terms, so inspect the full tree before conclusions. | **Inspect / possibly consolidate** | Actual implementation files, tests, auth, usage records, budgets, timeout/retry behavior, error handling, model capability differences, deployment and license. |
| [AlamiaSoft/VPS-AI-Bridge-Standalone](https://github.com/AlamiaSoft/VPS-AI-Bridge-Standalone) | README describes a lightweight FastAPI OpenAI-compatible proxy with aliases and provider fallback. | **Strong prototype candidate** | Full source, test coverage, client authentication (README example suggests a dummy key may be ignored—must verify and fix before any shared deployment), request limits, attribution, usage/cost logging, secrets, retries, resource footprint and license. |
| [AlamiaSoft/alamia-ai-hub](https://github.com/AlamiaSoft/alamia-ai-hub) | Found a Cloudflare-specific frontend service using Workers AI through an AI Gateway route. | **Inspect separately; not automatically the shared gateway** | Scope, API boundaries, environment/secret management, deployment and whether this is product-specific UI code rather than a general provider gateway. |
| [AlamiaSoft/AlamiaAICopilot-Starter](https://github.com/AlamiaSoft/AlamiaAICopilot-Starter) | `core/ai/provider.py` defines `AIModelProvider`; `docs/ai-provider-adapter.md` describes `AIModelRouter` and selection requirements. Architecture separates provider adapters from the Copilot core. | **Keep as consumer abstraction; add adapter if gateway proves useful** | Actual router/provider implementations, test results, provider capability handling and current consumers. Avoid duplicate retry/fallback loops. |

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
