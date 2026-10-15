# DEC-2026-001: Taliya Copilot — private alpha readiness

- **Status:** Proposed bounded validation; no broad SaaS launch commitment
- **Date:** 2026-10-15
- **Decision owner:** AlamiaSoft
- **Horizon:** H1 with H2 extension path
- **Class:** Revenue bet + bounded experiment
- **Review trigger:** Clean install/build, live TallyPrime tests, local security review, and one design-partner commitment

## Decision question

Can Taliya Copilot for TallyPrime be made useful to one or two real users in roughly one month without diverting capacity into a broad ERP platform or public multi-tenant SaaS?

## Desired outcome

A small business or consultant using TallyPrime can complete a narrow, valuable operational workflow through a clearer interface, with transparent review/approval for financial writes. Bank reconciliation is a candidate initial wedge; no tax/compliance-advice or autonomous posting promise.

## Evidence ledger

| Claim | Label | Evidence | Confidence | What could falsify it? |
|---|---|---|---|---|
| A substantial codebase exists | Observed (repository inspection) | Main branch contains FastAPI console API, Next.js UI, Tally connector, MCP tools, action queue, skills/packs and bank reconciliation code | High for code presence; not for runtime correctness | Clean build/tests reveal extensive breakage |
| Sprint 10 extends persona UI and SSE | Sourced (repo docs/code) | feature/sprint-10-agentic-platform contains mode/persona work; completion report claims 9/9 tests and frontend build | Medium; tests/build not rerun here | Branch fails when checked out and run |
| Current main is not the full latest described work | Observed | Main points to 57d0867 (2026-06-30); Sprint 10 is on a separate branch and not merged to main | High | A newer local branch/worktree exists |
| Product is ready for external exposure | Not established | Backend source shows wildcard CORS, no visible auth on inspected API routes, seeded demo drafts and generated mock XML payloads; frontend store hardcodes localhost:8000 | High that these code patterns exist; deployment impact depends on binding and packaging | A newer local implementation already removes these issues |
| Commercial demand exists | Assumed | No current design-partner or payment evidence reviewed | Low | Confirmed buyer/user commitment and clear willingness to pay |

## Scope for the bounded alpha

**Include**
- TallyPrime only.
- Local installable bridge/API and web UI; backend bound to loopback and not exposed to the public internet.
- One or two workflows that work end-to-end against a real TallyPrime company.
- Read/report flows and reviewable drafts; explicit user approval before financial writes.
- Bank reconciliation/investigation as a candidate differentiator if it is stable with real statements.
- Clean setup instructions, configuration, backup/recovery notes, logs and clear error messages.

**Exclude**
- Public multi-tenant SaaS, billing and organization administration.
- Odoo/ERPNext connectors.
- Autonomous financial posting.
- Compliance/regulatory claims.
- Marketplace, broad industry packs, voice and generalized agent platform expansion unless already stable and directly required by the pilot.

## Critical gates before inviting users

1. Reconcile main with feature/sprint-10-agentic-platform and any newer local work; establish the actual source of truth.
2. Clean install from documented steps; run frontend production build and all relevant Python tests. Verify results rather than relying only on completion reports.
3. Remove seeded demo actions from production behavior and eliminate mock XML from any UI that could be mistaken for the actual payload.
4. Restrict the local API: loopback-only binding, restrictive CORS allowlist, no public port exposure, and appropriate local request protection. No unauthenticated financial-write endpoint may be exposed beyond a trusted local boundary.
5. Test TallyPrime disconnected state, company selection, duplicate/retry behavior, draft edits, approval, execution failures and audit trail against a test company.
6. Verify backups and safe recovery for the SQLite action queue and reconciliation state.
7. Confirm one real design partner/user and agree on the specific workflow and feedback loop.

## Effort hypothesis

Planning range for a private alpha: **45–75 focused engineering hours** if existing code builds and core workflows mostly work. At 10–15 hours/week, that is approximately 3–8 weeks, with a one-month target possible only near the lower end and with tight scope.

Likely effort bands:
- Source-of-truth reconciliation, clean setup and gap audit: 4–8 hours.
- Security/local-boundary and demo-data hardening: 8–14 hours.
- Repair and verify one or two live workflows: 12–22 hours.
- Packaging, setup docs, diagnostics and recovery: 8–14 hours.
- User trial, fixes and stabilization: 13–17 hours.

These are estimates, not measured work breakdowns. If the local code is ahead of GitHub, the remaining effort may be lower; if key workflows are broken, it may exceed this range.

A public hosted multi-tenant SaaS is not estimated by this decision and should be treated as a separate scope.

## Stop / continue / scale

- **Stop or defer** if the current local source cannot be reconciled, financial writes cannot be safely bounded, or no real user problem/design partner is found.
- **Continue** if the code runs and a narrow live workflow is feasible within the hour cap.
- **Scale to a small paid pilot** only after clean install, security gates, real-workflow acceptance and user feedback pass.
- **Revisit Odoo** only after Tally workflows and connector boundaries are proven; do not let multi-ERP ambition delay the first useful launch.

## Decision

Proceed with a bounded readiness check and private alpha only; do not commit to a public SaaS launch date yet. Review after the first 6–8 hours of source-of-truth and clean-run validation. If that gate fails, re-estimate before spending further time.
