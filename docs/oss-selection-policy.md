# OSS-First Component Selection Policy

## Rule
Search before building, but do not adopt a library because it is popular, has many stars or makes an impressive demo. Evaluate a specific repository, release/version and use case. Listing a project means candidate, not endorsement.

## Required checks
1. **Capability fit:** Does it solve the actual need? Is the existing stack/smaller library enough?
2. **License/commercial use:** Read license at the pinned release and relevant directories. Check distribution, hosted service, modification, attribution, source disclosure, trademark and dependency obligations. Seek legal review if ambiguous/material.
3. **Maintenance:** Release cadence, substantive changes, maintainer bus factor, issue response, roadmap/deprecation. Stars are not maintenance evidence.
4. **Security:** Security policy/advisories, dependency exposure, secrets handling, isolation, secure defaults, SBOM and patch response.
5. **Maturity/fit:** Production evidence, release stability, docs, tests, language/framework fit, compatibility and failure behavior.
6. **Operations:** CPU/RAM/storage, external services, deployment complexity, upgrades, on-call burden, backup/restore and team skills.
7. **Integration cost:** Auth, tenancy, APIs, extension points, telemetry, data model, migrations and lock-in.
8. **Data handling:** Egress, telemetry defaults, retention, redaction, residency and ability to disable collection.
9. **Exit:** Stable wrapper/interface, export format, migration method, replace plan, owner and estimated effort.
10. **Provenance:** Exact version/commit, license evidence URL, dependency scan date and approver.

## Decision outcomes
- **ADOPT:** fits as-is with configuration.
- **ADAPT:** use behind a wrapper/adapter; document patches/conventions.
- **TRIAL:** proof-of-concept only; not production-approved.
- **BUILD:** no acceptable fit; document why candidates fail.
- **REJECT:** material risk, mismatch, excessive cost or unsuitable license.
- **REPLACE/RETIRE:** current dependency no longer meets needs.

## Suggested scoring
Rate 1–5: capability fit (20%), license/commercial clarity (15%), maintenance (15%), security (15%), maturity/testing (10%), integration fit/cost (10%), resource cost (5%), data/privacy controls (5%), exit path (5%). Scores cannot override license or critical-security blockers. Unknown is not a pass.

## Production adoption record
Repository/publisher; exact version/tag/commit; license and dependency licenses; maintenance evidence; security/dependency review; functional/scale/resource tests; data-flow/telemetry review; integration boundary/owner; ADR and alternatives; upgrade/exit plan; approver/status/review trigger.

## Patches and forks
Prefer configuration and extension APIs. Keep patches minimal and track upstream issue/PR. Fork only with a named maintainer and budget. Preserve notices/license obligations.

## No cargo-cult stacks
Do not install overlapping gateways, observability tools, workflow engines or vector stores just to appear future-ready. One component per responsibility is the starting assumption. Additional components need measurable benefit, a resource budget, an owner and a removal plan.
