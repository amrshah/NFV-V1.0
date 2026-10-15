# OSS Component Register (Initial, Provisional)

**All entries are candidates, not approvals.** Features, licenses and maintenance change. Before production use, pin an exact release/commit and verify license files, dependency tree, advisories, maintenance, resource use and current docs.

| Capability | Candidate | Initial role | Status | Due diligence |
|---|---|---|---|---|
| LLM routing/provider abstraction | [LiteLLM](https://github.com/BerriAI/litellm) | Common model API, routing/retries/budgets | TRIAL | Verify current license and enterprise boundaries; test provider coverage, tenant/auth isolation, latency, deployment complexity and cost/usage telemetry. Routing alone is not governance. |
| LLM tracing/evaluation | [Langfuse](https://github.com/langfuse/langfuse) | Traces, datasets, evaluations, AI observability | TRIAL | Project has had MIT-licensed core and separately licensed EE areas; inspect exact release/directories/dependencies. Validate self-host footprint, access, retention, redaction and telemetry controls. |
| MCP servers/clients | [FastMCP](https://github.com/jlowin/fastmcp) | Simplify MCP tool/server implementation | TRIAL | Verify current package/repository, version and license. SDK convenience is not security: application boundary still needs authentication, permissions, schema validation, audit and tests. |
| Agent/MCP/A2A proxy | [agentgateway](https://github.com/agentgateway/agentgateway) | Potential proxy for agent/tool traffic | TRIAL | Validate actually needed protocols, maturity, identity propagation, policy, telemetry, license, resource profile and operational complexity. |
| Durable workflows | [Temporal](https://github.com/temporalio/temporal) | Long-running, durable multi-step workflows | TRIAL WHEN JUSTIFIED | Heavier than a queue. Verify server/SDK licenses, hosting/ops, backups, upgrades and operator skill. Don't add for simple background jobs. |
| Workflow/integration automation | [n8n](https://github.com/n8n-io/n8n) | Internal workflow prototypes/permitted integrations | RESTRICTED EVALUATION | **Its Sustainable Use License is not OSI open source** and may restrict embedding/resale/product use. Do not embed/resell without current-license review and authorization. |
| Local inference | [Ollama](https://github.com/ollama/ollama) | Local development/testing | DEV-FIRST TRIAL | Model weights have separate licenses. Validate hardware, deployment, quality and production suitability independently. |
| Managed AI gateway | [Cloudflare AI Gateway](https://developers.cloudflare.com/ai-gateway/) | Optional managed gateway/observability | OPTIONAL SERVICE | Not self-hosted OSS. Compare cost, provider coverage, data handling, latency and lock-in. |
| Hosted inference route | [OpenRouter](https://openrouter.ai/) | Potential primary hosted provider route | PROVIDER OPTION | Service, not OSS. Check current model/provider terms, privacy, cost, data handling, availability and task-specific quality. |
| Hosted inference fallback | [Groq](https://groq.com/) | Possible fallback for compatible workloads | PROVIDER OPTION | Service, not OSS. Test model fit, availability, limits, privacy and price. Exercise fallback in tests. |
| Basic search | Existing DB/PostgreSQL full-text search | First option to test where adequate | EVALUATE FIRST | Avoid a vector store unless semantic-search needs justify it. |
| Vector retrieval | Qdrant or alternative | Only for demonstrated vector-search requirement | TRIAL WHEN NEEDED | Compare license, backups, filtering, tenant isolation, resources, replication and deletion behavior. |

## Candidate combinations to trial

### Small app with modest AI usage
Product app → thin internal AI adapter → one provider, with structured usage/failure/cost logging. A separate router/tracing platform may not be justified.

### Shared multi-product AI
Trial LiteLLM or another router plus structured application telemetry. Add Langfuse or equivalent only if evaluation/tracing outweighs hosting, privacy and maintenance costs. Attribute usage to tenant/feature; avoid sensitive prompt retention by default.

### MCP
Define typed domain action contracts first. Add FastMCP or another maintained SDK if useful. Enforce identity, resource/tenant authorization, validation, limits and audit at/below the tool boundary. Never expose internal admin endpoints directly.

### Long-running workflows
Start with existing queues/jobs. Evaluate Temporal only if durable orchestration materially improves recovery for multi-step work. Specify idempotency, cancellation, retries, compensation and approvals first.

## Review
Review on major upgrades, security advisories, license changes, upstream abandonment or scale change, and at least annually where practical. Record owner/next review. Provider/service choices are distinct from OSS dependencies.
