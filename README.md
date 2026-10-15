# NFV Readiness Standard (NFV-RS)

**Version:** 1.0-draft  
**Status:** Initial working standard; subject to evidence-based review  
**Owner:** AlamiaSoft / NFV initiative

## Purpose

Make software valuable today and adaptable to plausible near-future software and AI capabilities without speculative overengineering.

**Core principle: OSS-first, integration-led engineering.** Reuse proven open-source components where they fit; adapt them behind stable contracts; build only missing differentiation; preserve a credible exit path. A collection of repositories is not an architecture.

## Three horizons

1. **Competitive now (next 1–3 years):** customer value, security, reliability, integration, operational efficiency, and useful AI where it makes a measurable difference.
2. **AI-native and agent-ready bridge:** machine-readable capabilities, permission-aware context, observable automation, explicit approval boundaries, replaceable model providers and interop adapters.
3. **Near Future Vision:** plausible future scenarios such as more autonomous agents, cross-system workflows, richer interfaces and machine-to-machine interaction. Dates and adoption rates are scenarios, not guarantees.

Build for today's needs, architect a bridge to agent readiness, and preserve options for later.

## Documents

- [The Standard](docs/standard.md)
- [Weighted scoring rubric and evidence rules](docs/scoring-rubric.md)
- [Reference architecture and capability boundaries](docs/reference-architecture.md)
- [OSS-first selection policy](docs/oss-selection-policy.md)
- [Provisional OSS component register](docs/oss-component-register.md)
- [Product adoption matrix](docs/product-adoption-matrix.md)
- [Phased roadmap](docs/roadmap.md)
- [AI Gateway pilot plan](docs/ai-gateway-pilot-plan.md) — proposed first implementation pilot
- [AI Gateway repository inventory](docs/ai-gateway-repository-inventory.md) — existing bridge assets and initial security finding
- [Architecture Decision Record template](docs/adr-template.md)
- [Contributing and change control](CONTRIBUTING.md)

## Non-goals

NFV-RS does not require every application to use AI, MCP, agents, a vector database, a workflow engine, Kubernetes or every project in the register. Do not add infrastructure without a demonstrated requirement. A small implementation can be the right implementation.

## Initial operating rules

1. Critical security and data-boundary failures are release blockers; a high aggregate score cannot offset them.
2. Claims require evidence. Documented is not implemented; implemented is not tested; tested is not necessarily production-proven.
3. Prefer provider-agnostic AI interfaces. OpenRouter as a primary route, a tested fallback such as Groq, optional Cloudflare AI Gateway/Workers AI, and local Ollama for development are current directions to evaluate—not universal or permanently approved choices.
4. Where model calls exist, usage/cost/failure logging is mandatory, with privacy-conscious redaction and data minimization.
5. OSS candidates remain provisional until exact-version license, dependency, security, maintenance, integration, resource and exit-path checks are recorded.
6. Prefer one well-understood component per capability over overlapping stacks.
7. Consequential actions require server-side policy checks and explicit approval boundaries until bounded autonomy is justified and tested.

## Status

This is an internal working standard, not an external certification or claim of compliance with a third-party framework. Use issues and pull requests to propose changes.
