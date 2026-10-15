# NFV Adoption Roadmap

Outcome-driven, not a mandate to build everything. Work incrementally and ship value early.

## Phase 0 — Inventory and baseline
- Identify repositories, commits, runtime topology, owners and critical flows.
- Assess readiness with evidence grades.
- Check critical gates: tenant boundaries, vulnerabilities, data leakage, secrets, recovery and consequential writes.
- Inventory OSS, providers, SDKs, licenses and deployed services.
- Select one pilot based on customer impact and shared-component leverage.

**Exit:** baseline, actual architecture map, risk register and prioritized backlog exist.

## Phase 1 — Reliable foundation
- Standardize health checks, structured logs, correlation IDs and error handling.
- Document API/event contracts and deployment/recovery steps.
- Add tests, dependency/license inventory and migration safety.
- Make authorization and tenant boundaries testable.
- Establish audit events and privacy-conscious telemetry.

**Exit:** critical gates pass or have approved time-bounded exceptions; critical workflows have tested recovery.

## Phase 2 — Economical AI foundation (AI products only)
- Define a small internal model invocation contract.
- Record model/provider/version, tenant/feature, latency, usage/cost estimate, outcome/error with redaction.
- Implement bounded timeout/retries and tested fallback; evaluate quality as well as reachability.
- Add budgets/quotas and a representative evaluation set.
- Trial LiteLLM only if complexity removed exceeds operations cost.

**Exit:** provider interruption and budget exhaustion have known behavior; important AI paths have measured cost/quality.

## Phase 3 — Safe business automation
- Define typed actions and domain events.
- Enforce policy/authorization in application code.
- Add idempotency, bounded retries, timeout, cancellation, dead-letter/recovery and approvals.
- Start with deterministic workflows and queues.
- Trial durable orchestration only for demonstrated long-running/recovery needs.

**Exit:** critical workflows have visible status, traceable effects and tested recovery.

## Phase 4 — Permission-aware knowledge and integrations
- Define source ownership and provenance metadata.
- Add retrieval only where full-text/direct APIs/relational queries are inadequate.
- Filter by authorization before context reaches the model.
- Add freshness/deletion propagation and retrieval evaluations.
- Introduce MCP/A2A only for specific integration or agent-collaboration needs.

**Exit:** tests demonstrate retrieval/tool isolation and policy enforcement.

## Phase 5 — Bounded autonomy
- Classify action risk and assign allowed autonomy level.
- Start read-only/draft-only; expand to approved writes where justified.
- Define limits, approval rules, escalation, pause/kill controls and rollback/compensation.
- Run adversarial, fault-injection and regression tests.
- Measure success, erroneous actions, override rate and support burden.

**Exit:** owner accepts residual risk; controls and rollback are tested. Do not advance merely to reach a level.

## Portfolio cadence
- Each sprint: update the highest-value gap and evidence.
- Each material architecture/dependency decision: create an ADR.
- Each major release or incident: revisit relevant critical gates.
- Periodically (quarterly where practical): review OSS health, licenses, cost, unused infrastructure and exit readiness.

## Resource rule
Prioritize work that improves multiple products or enables near-term customer/revenue outcomes. Don't retrofit the whole portfolio at once. Ask of every component: can the current stack, a small library or a thin adapter safely solve most of the actual need?
