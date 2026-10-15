# Productization Roadmap (Decision-Driven)

This is a portfolio view for commercialization, not a promise that every listed idea will be built. Candidates must be assessed using the NFV [Decision & Learning Loop](decision-learning-loop.md), including evidence quality, effort ranges, capacity and opportunity cost. Do not infer readiness from a repository or a design demo.

## Current candidate record

### Taliya Copilot for Tally — validate as a narrow launch candidate

- **Stage:** Candidate / validate cheaply; not approved for broad public SaaS launch.
- **Initial customer hypothesis:** a small business or consultant who already uses TallyPrime and wants a more accessible operational interface and assistance with accounting workflows.
- **Initial product scope:** TallyPrime only; installable/local bridge and web UI; read/report workflows plus reviewable drafts and explicit approval for financial writes. Prioritize bank reconciliation and clear business-language workflows.
- **Later expansion:** Odoo and other ERPs only after Tally workflows, connector contract and product economics are validated.
- **Code evidence:** GitHub main has a FastAPI console API, Next.js UI, Tally connector, MCP tools, SQLite draft/action queue, accounting/reconciliation capabilities and unit-test documentation. A separate feature/sprint-10-agentic-platform branch contains persona/mode switching and SSE workflow UI, with a completion report claiming 9/9 tests and a successful frontend build. These are repository claims, not independently rerun verification.
- **Important uncertainty:** the public GitHub branch state is dated 2026-06-30 and Sprint 10 is not merged to main. Current local work may be newer; reconcile before estimating implementation from scratch.
- **Initial effort hypothesis:** 35–60 focused hours for a tightly scoped closed alpha/design-partner pilot, assuming the existing code runs and local work is current; 100+ hours may be required if code is stale or core workflows need repair. A secure multi-tenant hosted SaaS is a separate, larger scope.
- **Decision:** run a time-boxed readiness check and attempt a private alpha only if the critical gates pass. Do not promise a public commercial launch date yet.
- **Review trigger:** after a clean install/build, live TallyPrime workflow tests, a security review, and confirmation of one reachable design partner.

## Candidate status rules

- **Unassessed:** idea recorded; no comparative decision made.
- **Validate cheaply:** a bounded test can resolve a key uncertainty.
- **Ready to commit:** customer, feasibility, security and capacity gates pass; effort and opportunity cost are accepted.
- **Active:** implementation has explicit owner, scope, budget and review gate.
- **Launched:** real users can onboard and complete the promised workflow; not merely deployed.
- **Deferred / stopped:** reason and revisit trigger recorded.

Other products should be added only as candidates, then assessed against the same process. Do not treat this initial Taliya entry as a ranking of the full portfolio.
