# ADR-0001: Select Alamia AI Gateway as First NFV Pilot

- **Status:** Accepted for pilot selection; implementation engine undecided
- **Date:** 2026-10-15
- **Decision owner:** AlamiaSoft engineering
- **Affected products:** Alamia AI Gateway, AlamiaAICopilot-Starter, prospective Kamal Express / Alamia Accounts integrations
- **Review trigger:** after source-level comparison and a small LiteLLM-vs-existing-bridge prototype

## Context

NFV-RS needs a real pilot to validate its scoring, critical gates, OSS-first selection process and reference architecture. The AI Gateway is intended to serve more than one AI-capable product and has measurable outcomes: provider portability, cost per successful task, latency, fallback success, usage attribution and operational burden.

Existing code was found:
- `AlamiaSoft/AlamiaAICopilot-Starter` defines `AIModelProvider` and describes an `AIModelRouter`.
- `amrshah/Alamia-AI-Bridge` is a CLI-oriented provider bridge, not a production HTTP gateway.
- `AlamiaSoft/VPS-AI-Bridge-Standalone` is an OpenAI-compatible FastAPI prototype. Initial source review found no visible authentication on model endpoints, no visible runtime provider failover after upstream failure, broad error wrapping, unpinned dependencies and no tests in the inspected tree.
- `AlamiaSoft/alamia-ai-hub` contains a Cloudflare-specific service file with a hardcoded token in source. Treat as potential credential exposure and rotate/revoke if valid; see [repository inventory](../ai-gateway-repository-inventory.md).

## Decision

Select **Alamia AI Gateway as the first NFV pilot**, but do not yet select an implementation engine or create another gateway repository. First compare:
1. A small, secured adaptation of the existing FastAPI bridge.
2. LiteLLM OSS as the routing/proxy engine behind an Alamia-owned contract.
3. The existing Copilot in-process router for local/mock and capability selection.

The current hypothesis is that LiteLLM may reduce custom routing, budget, fallback and spend-tracking code. This must be demonstrated against its exact release, operational footprint, license/dependencies and actual providers. If its complexity exceeds the value, retain a small bridge behind a strict interface.

## Boundaries

- Copilot Starter retains its provider abstraction; if the shared gateway is adopted, add a provider adapter/client rather than duplicating the router in each product.
- Gateway authenticates workloads, applies gateway-level limits and records usage; it does not replace product authorization or tenant-data permissions.
- Avoid competing retry/fallback loops. Assign responsibility to one layer and test behavior.
- OpenRouter primary and Groq fallback are hypotheses for compatible workloads, not unconditional rules. Ollama is a development/testing option; Cloudflare is optional.
- No extra Langfuse deployment in the first increment unless existing logs/metrics/evaluation harness prove inadequate.
- No vector DB, agent gateway, billing platform or orchestration engine in the first increment absent demonstrated need.

## Acceptance criteria

- Authentication and per-caller isolation are tested; unauthorized requests are rejected.
- Credentials are not in browser bundles, source, logs or images.
- Primary timeout, 429, 5xx, malformed output and all-provider-down cases are tested.
- Fallback succeeds for compatible workloads without retry storms or unbounded cost.
- Every request can be attributed to product/feature using trusted server-side identity, with privacy-conscious usage/cost/error telemetry.
- Provider/model can change through configuration/alias policy without domain-code rewrites.
- Two real consuming workflows integrate successfully.
- Critical NFV gates have evidence; resource usage and operational cost are measured.
- An exit plan and exact-version license/dependency/security review are recorded.

## Consequences

Positive: shared value across products; real test of OSS-first composition; measurable cost/quality and reliability improvements.

Negative: adds a network hop and a service to operate; requires credential management, availability monitoring and clear retry ownership. If only one product uses it or central metering/routing does not justify the operational burden, keep the local abstraction and defer the shared service.
