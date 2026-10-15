# NFV Adoption Roadmap

Outcome-driven, not a mandate to build everything. The roadmap governs how we decide and learn as well as how we adopt technical capabilities.

## Phase 0 — Establish the decision-and-learning loop (current NFV priority)

- Use the [decision and learning loop](decision-learning-loop.md) for material choices.
- Keep decision records for major product, OSS, architecture and investment commitments.
- Bound experiments by hours/cash and define success, stop and review criteria before work begins.
- Maintain a portfolio review with capacity, opportunity cost, active-work-in-progress and explicit defer/stop decisions.
- Separate observed facts, sourced claims, inferences, assumptions and speculative futures.
- Review this framework after real decisions; simplify rules that add paperwork without improving outcomes.

**Exit:** the framework has been used to record at least one real decision and one bounded experiment; estimates include confidence and opportunity cost; a review records what was learned. This is a process test, not permission to start any particular product.

## Phase 1 — Inventory and baseline

- Identify repositories, commits, runtime topology, owners and critical flows.
- Assess readiness with evidence grades.
- Check critical gates: tenant boundaries, vulnerabilities, data leakage, secrets, recovery and consequential writes.
- Inventory OSS, providers, SDKs, licenses and deployed services.
- Select a pilot only after comparing it with other commitments using the NFV decision process.

**Exit:** baseline, actual architecture map, risk register and prioritized backlog exist.

## Phase 2 — Reliable foundation

- Standardize health checks, structured logs, correlation IDs and error handling.
- Document API/event contracts and deployment/recovery steps.
- Add tests, dependency/license inventory and migration safety.
- Make authorization and tenant boundaries testable.
- Establish audit events and privacy-conscious telemetry.

**Exit:** critical gates pass or have approved time-bounded exceptions; critical workflows have tested recovery.

## Phase 3 — Economical AI foundation (AI products only)

- Define a small internal model invocation contract.
- Record model/provider/version, tenant/feature, latency, usage/cost estimate, outcome/error with redaction.
- Implement bounded timeout/retries and tested fallback; evaluate quality as well as reachability.
- Add budgets/quotas and a representative evaluation set.
- Trial LiteLLM only if complexity removed exceeds operations cost.

**Exit:** provider interruption and budget exhaustion have known behavior; important AI paths have measured cost/quality.

## Phase 4 — Safe business automation

- Define typed actions and domain events.
- Enforce policy/authorization in application code.
- Add idempotency, bounded retries, timeout, cancellation, dead-letter/recovery and approvals.
- Start with deterministic workflows and queues.
- Trial durable orchestration only for demonstrated long-running/recovery needs.

**Exit:** critical workflows have visible status, traceable effects and tested recovery.

## Phase 5 — Permission-aware knowledge and integrations

- Define source ownership and provenance metadata.
- Add retrieval only where full-text/direct APIs/relational queries are inadequate.
- Filter by authorization before context reaches the model.
- Add freshness/deletion propagation and retrieval evaluations.
- Introduce MCP/A2A only for specific integration or agent-collaboration needs.

**Exit:** tests demonstrate retrieval/tool isolation and policy enforcement.

## Phase 6 — Bounded autonomy

- Classify action risk and assign allowed autonomy level.
- Start read-only/draft-only; expand to approved writes where justified.
- Define limits, approval rules, escalation, pause/kill controls and rollback/compensation.
- Run adversarial, fault-injection and regression tests.
- Measure success, erroneous actions, override rate and support burden.

**Exit:** owner accepts residual risk; controls and rollback are tested. Do not advance merely to reach a level.

## Operating cadence

- **Weekly:** 10–15 minute capacity/evidence check.
- **Monthly:** 30–60 minute decision and portfolio review.
- **Quarterly:** 60–90 minute scenario, assumptions, OSS health, cost and exit-readiness review.
- **Event-driven:** review after a material security incident, provider/license change, customer signal, failed release or market shift.

Use the [portfolio review template](portfolio-review.md). Each material architecture/dependency decision should have an ADR; use a lightweight decision record for reversible lower-risk choices.

## Resource rule

Prioritize work that improves multiple products or enables near-term customer/revenue outcomes, but treat reuse as a hypothesis to validate. Don't retrofit the whole portfolio at once. Ask of every component: can the current stack, a small library or a thin adapter safely solve most of the actual need?

Every new initiative must state its opportunity cost: what work loses time or is explicitly deferred. Keep some capacity for maintenance and unexpected work; don't allocate 100% of available hours to roadmap features.
