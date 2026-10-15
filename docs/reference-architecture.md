# NFV Reference Architecture

## Design goals
1. Keep domain logic independent of model vendors and agent protocols.
2. Centralize controls where consistency materially improves safety.
3. Reuse OSS behind stable contracts instead of scattering vendor-specific code.
4. Start with the smallest topology that satisfies measured needs.
5. Preserve exit paths without pretending migration is free.

## Logical view

```text
People / Product UI / Authorized External Agents
                    |
          API + Identity Boundary
                    |
       Product Domain Application
    (rules, records, permissions, policy)
         |          |             |
      Events     Tool/API       Read model
         |       contracts          |
    Workflow / Automation       Knowledge & Retrieval
         |          |                |
         +------ Policy & Audit ------+
                    |
          AI Application Adapter
                    |
         Alamia AI Gateway interface
                    |
       Router / Provider adapters
   (hosted providers / local inference)
                    |
    Logs, Metrics, Traces, Cost, Evals
```

This is a logical view, not a requirement that every box be a separate deployable service.

## Capability ownership

### Alamia Core — application foundation
Own stable primitives/conventions: identity and tenancy integration, permission checks, audit-event contracts, modularity, configuration, usage-metering contracts, API conventions, event schemas, health/diagnostics hooks and migration/testing conventions. Do not put product-specific business rules here or couple Core to a particular LLM, vector DB, workflow engine or agent protocol.

### Alamia AI Gateway — model access and governance
Own provider-agnostic internal interface; routing/selection; budget/rate-limit integration; timeouts/fallback policy; provider telemetry; prompt/model config references; redaction/data-minimization and evaluation hooks.

A router alone is not full governance. Product authorization, tenant data access and consequential-action policy stay enforced by trusted application services. Usage logs should enable cost/latency/provider/failure/outcome analysis without indiscriminate retention of prompts or sensitive payloads.

Current directions to validate—not permanent defaults: OpenRouter may be primary; Groq may be fallback for suitable workloads; Cloudflare AI Gateway/Workers AI are optional; Ollama is useful for local development/testing. Benchmark quality, price, privacy, terms, availability and product fit.

### Alamia Copilot / Automation — orchestration and action
Own request/session context, role and skill registry, tool contracts, approvals, policy integration, workflow state/action status, cancellation, retry/escalation and actor-aware audit.

The LLM is not the policy engine. Tool implementations re-check authorization and validate input on execution. Start with deterministic jobs/queues. Adopt durable workflow platforms only when long-running work/recovery justifies them.

### Alamia Knowledge — retrieval and context
Own ingestion contracts, provenance, document lifecycle, access-filtered retrieval, freshness, deletion propagation and retrieval evaluation. Keep authoritative business records in their systems of record; embeddings/indexes are derived infrastructure. Do not add a vector database until semantic retrieval is genuinely required; full text, relational queries or direct APIs may suffice.

### Integration adapters
Own protocol/vendor details for REST, webhooks, MCP and, where justified, A2A. Translate external requests to typed internal capability calls. Do not bypass authorization or duplicate business rules. MCP/A2A are optional and do not replace authorization, schemas, idempotency or monitoring.

### Product domain application
Own canonical records, domain rules, authorization decisions, invariants, user flows and business outcomes. It is the source of truth for permitted actions and success criteria.

## Cross-cutting contracts

For every tool/action: stable ID/version; purpose/owner; typed schema; side effects/data classification; actor/tenant/resource permissions; idempotency/retry behavior; errors/timeouts/rate limits; approval requirement/autonomy ceiling; audit fields, metrics and tests.

For every event: producer, schema/version, consumer expectations, delivery/retry behavior, sensitivity, retention and correlation ID.

## Minimal deployment profiles

### Profile S — small product
Product app + relational DB + built-in queue/managed job runner if needed + logs/health checks. Add a model adapter if AI is needed. Don't add separate workflow/vector/cache/gateway/observability platforms without demonstrated need.

### Profile M — multi-tenant SaaS with meaningful AI
Consider provider router, structured AI telemetry, budgets, permission-aware retrieval where needed, workflow controls and actionable alerts. Keep component count proportional to operational capacity.

### Profile L — high-criticality/multi-system work
Consider durable orchestration, stronger policy controls, isolated tool execution, centralized tracing, multiple inference paths, disaster-recovery tests and threat modeling. Requires operational ownership; don't select merely for future-proofing.

## Required ADRs
Create an ADR for adding infrastructure; selecting model/router/workflow/vector/MCP/A2A implementation; accepting material license/security risk; enabling autonomous writes; changing source-of-truth/retention; or replacing a shared capability.
