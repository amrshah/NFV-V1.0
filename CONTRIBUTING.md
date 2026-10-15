# Contributing to NFV-RS

## Scope
This repository defines an internal engineering standard. Contributions should make decisions clearer, safer, more measurable or cheaper to maintain. Avoid requirements without applicability and evidence.

## Propose a change
For small fixes, open a pull request. For material changes, open an issue covering problem/evidence, affected domains, compatibility/migration, operational/security/cost impact, alternatives and success criteria.

## OSS proposals
Follow [the OSS selection policy](docs/oss-selection-policy.md). Include exact repository/version, license evidence, maintenance/security review, integration test, resource estimate, data-handling review, owner and exit plan. Unknown facts stay marked unknown. A candidate is not production-approved simply by being listed.

## Versioning
- Patch: clarifications, examples and non-normative fixes.
- Minor: backward-compatible controls or assessment improvements.
- Major: changed critical gates, scoring semantics or incompatible architecture rules.

The `v1.0-draft` label means this is a working internal draft, not an external certification. Material changes should state migration implications.

## Normative terms
- **MUST:** required when applicable.
- **SHOULD:** recommended; deviation requires a recorded reason.
- **MAY:** optional.
- **MUST NOT:** prohibited for conformance.

Each requirement must define evidence and applicability. Waivers require owner, rationale, compensating control, expiry and remediation. A waiver does not make a failed gate pass.

## Review
Request product-owner review and security/operations review where relevant. Changes to critical gates, license policy or autonomous writes require explicit review. Never commit secrets/customer data or third-party source without confirming rights and provenance.

## Principles
- OSS-first, not popularity-first.
- Operational footprint proportional to real needs.
- Stable shared primitives; product business logic stays in products.
- Reversible decisions and explicit exits.
- AI outputs and retrieved content are untrusted.
- Measure outcomes, not feature count.
