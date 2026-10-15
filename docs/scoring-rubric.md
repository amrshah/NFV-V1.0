# NFV Readiness Scoring Rubric

## Formula
**Weighted score = Σ (domain score ÷ 5 × domain weight).** Total weight is 100. Score applicable domains 0–5 using evidence. Mark N/A only with written rationale; normalize remaining weights proportionally and show excluded domains. N/A must not hide a gap.

| Domain | Weight | Score 0 | Score 3 | Score 5 |
|---|---:|---|---|---|
| Customer value and UX | 10 | No validated outcome or usable primary flow | Primary flows work and key outcomes measured | Strong usability/accessibility/outcome evidence, continuous improvement |
| Security and privacy | 15 | Critical protections absent or unknown | Controls implemented and important risks tested | Threat-informed controls, verification, recovery drills and demonstrated boundaries |
| Interoperability and contracts | 12 | Ad hoc coupling/no stable contract | Documented versioned interfaces with contract tests | Mature compatibility policy, reliable events, tested adapters and exits |
| AI/model operations | 12 | N/A with reason if no AI; otherwise opaque calls | Routing/limits/logging/fallback/evaluation for important flows | Reproducible evaluation, policy routing, privacy-aware telemetry, cost/quality control |
| Workflow and autonomy | 12 | Critical work opaque or unsafe | Observable jobs/workflows, retries, approvals and recovery | Policy-bounded automation, reliable recovery, audited autonomy/escalation |
| Context and knowledge | 10 | Uncontrolled/untraceable context | Permission-aware retrieval and provenance on important paths | Freshness, lifecycle, reconstruction, correction/deletion and tested boundaries |
| Operability/observability | 12 | Failures hard to detect/diagnose | Health, logs, metrics and actionable alerts | SLO-informed operations, tracing, tested recovery/capacity/failure exercises |
| Modularity/evolution | 12 | Tightly coupled/manual changes | Stable boundaries, tests, migrations and versioning | Proven replaceability, staged rollout, dependency lifecycle/drift controls |
| Deployment/cost/resource fit | 5 | Unrepeatable or uneconomic | Repeatable deploy and known resource/cost profile | Measured unit economics, cost controls, practiced deployment/recovery |
| **Total** | **100** |  |  |  |

Scores 1, 2 and 4 are intermediate: 1 = isolated/mostly manual; 2 = partial/basic with material gaps; 4 = mature and consistently applied but not fully demonstrated at 5.

## Evidence grades
- **E0 Assertion:** unverified statement.
- **E1 Artifact:** documentation/code/config exists.
- **E2 Test:** repeatable test or inspection passes.
- **E3 Operational:** production/representative evidence, metrics or drill.
- **E4 Sustained:** trends across releases/incidents and independent review where proportionate.

Scores 4–5 normally need E2–E3 evidence; score 5 for critical/high-risk capabilities usually needs E3–E4. Record evidence links and assessment date.

## Interpretation (not certification)
- **0–39:** foundational gaps; fix critical gates and customer-critical risks.
- **40–59:** developing; prioritize contracts, operations and testing.
- **60–74:** integrated; strengthen evidence and AI/automation controls as relevant.
- **75–89:** strong readiness; validate failure modes, exits and production outcomes.
- **90–100:** advanced; require independent scrutiny for high scores.

## Mandatory gate overlay
Record each critical gate PASS / FAIL / N/A WITH RATIONALE. FAIL means not production NFV-ready regardless of score. Unassessed is UNKNOWN, not PASS.

## Assessment record
For every domain record score, weight, scope/applicability, evidence grade/link, date/assessor, gap and risk, next action/owner/date, and any exception/expiry.

Prioritize using risk, likelihood, customer impact and dependency leverage, not score alone. Security blockers precede easy score improvements.
