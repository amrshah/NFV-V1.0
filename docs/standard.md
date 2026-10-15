# NFV Readiness Standard v1.0-draft

## Intent and horizons
A product is NFV-ready when it solves a real customer problem today and can evolve safely as software, AI and agent capabilities change. Apply proportionately to products, APIs and shared infrastructure.

- **H1 — Competitive now (next 1–3 years):** useful workflows, security, reliability, integration, accessibility and measurable value; useful AI where it helps.
- **H2 — AI-native/agent-ready bridge:** machine-readable capabilities, permission-aware context, observable automation, approval boundaries and replaceable providers/protocols.
- **H3 — Near Future Vision:** plausible scenarios including more capable agents, cross-system workflows and multimodal interfaces. Dates and adoption are not guarantees.

Build H1; prepare stable seams for H2; preserve options for H3.

## Capability domains

### D1. Customer value and quality
- Defined target user, job-to-be-done and measurable outcome.
- Clear primary flows, responsive/accessibility basics, onboarding and support.
- Track relevant outcomes and cost-to-serve.
- Use AI only where it beats a simpler deterministic approach on value.

### D2. Security, privacy and control — critical gate
- Authentication, least privilege and resource-level authorization.
- Automated negative tests for tenant isolation where multi-tenant.
- Secrets management, secure transport, dependency scanning and secure deployment.
- Audit sensitive reads, policy decisions and consequential writes.
- Data minimization, retention, export/deletion and consent as applicable.
- Tested backup restoration and an incident response path.
- For AI: defend against prompt injection/exfiltration, treat retrieved content as untrusted, constrain tools, validate outputs and prevent cross-tenant retrieval. The model is never the security boundary.

### D3. Interoperability
- Versioned APIs, clear schemas, authentication, limits and error semantics.
- Verified webhooks/events with retry and replay handling.
- Import/export and contract/backward-compatibility tests.
- Protocol adapters including MCP/A2A only when useful; adapters may not bypass app authorization.

### D4. AI/model operations — required if model calls exist
- Provider abstraction where practical; isolate provider-specific details.
- Routing may consider capability, privacy, cost, latency, quality and availability.
- Bounded timeouts/retries, tested fallbacks, rate limits and graceful degradation.
- Versioned prompts/config, structured outputs and tool-call validation.
- Log model/provider/version, tenant/feature, latency, usage/cost estimate, outcome and error with sensitive content minimized/redacted.
- Representative evaluation sets and regression checks for material AI functions.
- Permission-aware retrieval and per-tenant/feature budgets where appropriate.

### D5. Workflows and controlled autonomy
- Explicit business events and workflow states.
- Use durable orchestration only when the requirement justifies it; a queue may suffice.
- Idempotency, bounded retries, timeout, cancellation, dead-letter and recovery.
- Human approval for financial, legal, medical, identity, irreversible or otherwise consequential actions.
- Policy is enforced in trusted application code, not an LLM prompt.
- Record trigger, actor/agent, policy decision, effects, outcome and failure. Make autonomy easy to pause/reduce.

### D6. Context and knowledge
- Separate authoritative records from summaries, embeddings and model inference.
- Track provenance, timestamp, ownership, permission and freshness where applicable.
- Apply authorization before content reaches a model.
- Support correction, expiry, export and deletion; memory is not the system of record.

### D7. Operability
- Health/readiness, structured logs, metrics and correlation/tracing proportionate to complexity.
- Monitor business outcomes alongside technical latency/errors/availability.
- For AI, track cost/usage, provider, latency, failure, fallback and quality indicators.
- Actionable alerts, runbooks, deployment history, rollback and recovery.
- SLOs/load tests proportionate to criticality and scale.

### D8. Evolution
- Stable internal interfaces isolate replaceable vendor SDKs.
- Versioned schemas/APIs, safe migrations, tested upgrades and release pinning.
- Feature flags for staged rollout with owners and cleanup.
- Tests proportional to risk; ADRs for material choices.
- Preserve a credible exit path; do not pretend portability makes migration free.

### D9. Deployment and economics
- Repeatable deployment, documented dependencies/resource profile and recovery steps.
- Measure cost-to-serve for important workload paths.
- Do not add DBs, brokers, vector stores, workflow engines or gateways without a requirement and owner.
- Separate safe dev/test workflows from production and real customer data.

### D10. Agent readiness — where useful
- Tool contracts define typed inputs/outputs, side effects, permissions, failure/idempotency behavior and autonomy ceiling.
- Distinguish read-only and write capabilities.
- Audit agent identity and delegated authority; never exceed policy/user permissions.
- Every autonomous operation has limits, observability, escalation and pause/kill controls.

## Readiness levels
- **L0 Unassessed:** evidence not gathered.
- **L1 Foundational:** applicable critical gates pass; core flows work; basic operations/recovery understood.
- **L2 Integrated:** versioned contracts, tests, observability, replaceable integrations and repeatable deployment.
- **L3 AI/automation-ready:** relevant AI/workflow controls, permission-aware context, evaluation and approvals operational.
- **L4 Adaptive/agent-ready:** governed tool/agent capabilities, bounded autonomy and tested cross-system recovery where justified.

A product can be strong at L2 without needing L4. Level claims require scope and evidence.

## Critical production gates
Applicable gates must pass; weighted score cannot offset failure:
1. Authorization and tenant isolation tested where applicable.
2. No unresolved critical security vulnerability without a time-bounded approved exception.
3. Backup/restore or equivalent recovery tested for important data.
4. Sensitive data not exposed through logs, retrieval, tools or telemetry outside policy.
5. Consequential writes have server-side authorization and policy checks.
6. AI-generated tool arguments and structured outputs validated before execution.
7. Production config, secrets and access controlled.
8. Critical workflow failures are observable and have an operator recovery path.

Exceptions name an accountable owner, compensating control, expiry and remediation. A waiver is not a pass.

## Evidence
Use tests, configuration/threat-model review, runbooks, restore drills, traces/metrics, load tests, license/dependency inventories, contract tests, evaluation results and production outcomes. Documentation alone is weak evidence. Reassess on major release, material architecture change, significant incident or scheduled review.
