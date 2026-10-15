# NFV Decision & Learning Loop

## Purpose
Turn NFV from a set of standards and architecture notes into a lightweight operating system for decisions under uncertainty. The loop exists to improve choices and resource allocation—not to justify a preferred project or generate paperwork.

## Operating loop
1. **Sense:** record meaningful changes in customers, technology, economics, regulation, competitors, OSS health and operations.
2. **Interpret:** state which assumptions, scenarios, capabilities or risks the change affects. Separate fact from inference and speculation.
3. **Frame:** define the decision, user outcome, why now, cost of waiting and alternatives—including doing nothing.
4. **Experiment:** choose the smallest reversible test that resolves a consequential uncertainty.
5. **Allocate:** state hours, cash, attention and opportunity cost. Name what will be delayed or stopped.
6. **Evaluate:** compare results with pre-agreed success, failure and safety criteria.
7. **Learn:** record changed beliefs and whether to stop, continue, scale, defer or revisit.

## Required decision brief
For material product, architecture, OSS or investment decisions, record:
- Decision question and owner.
- User/customer outcome sought.
- Evidence links and evidence quality.
- Assumptions and confidence (high/medium/low).
- Plausible scenarios that could change the choice.
- Options: adopt, adapt, trial, build, reject, defer, replace, or do nothing.
- Effort range and confidence; cash/operating cost.
- Opportunity cost: what work loses capacity.
- Reversibility, lock-in, security/privacy and operational risks.
- Smallest useful experiment and measurable acceptance criteria.
- Decision, rationale, review trigger/date and outcome.

Use [the decision record template](decision-record-template.md). A lightweight note is enough for reversible low-risk choices; use a full ADR for consequential architecture/security choices.

## Evidence rules
Label important claims:
- **Observed:** directly measured or verified in code/runtime/customer interaction.
- **Sourced:** supported by a credible external source; include date and link.
- **Inferred:** reasonable interpretation of evidence.
- **Assumed:** unverified premise used for planning.
- **Speculative:** plausible future possibility, not a current fact.

Repository presence is not proof of deployability; a demo is not proof of production reliability; personal enthusiasm is not demand validation. Estimates remain ranges with confidence and should state what would change them.

## Portfolio and capacity rules
- Plan against actual capacity, including interruptions, support, deployment and context switching.
- Keep a weekly capacity budget and a short active-work-in-progress limit. Every new initiative must identify what it displaces.
- Distinguish **customer/revenue commitments**, **reusable strategic capabilities**, **risk reduction/maintenance**, and **options/experiments**.
- Prefer work with a credible near-term customer/revenue outcome or reusable leverage, provided critical gates pass.
- Reuse is a hypothesis until integration cost, operating burden and actual adoption are measured.
- Apply critical security, legal, customer-value and feasibility gates before weighted scoring; scores inform judgment but do not replace it.
- Do not run several speculative platform builds in parallel. Bound experiments and define stop conditions first.
- Reserve capacity for maintenance and unexpected work; don't allocate 100% to roadmap features.

## Stop / continue / scale / defer
Before an experiment starts, define:
- **Stop:** critical safety/security failure, invalidated core hypothesis, no credible user problem, or cost cap exceeded without new evidence.
- **Continue:** hypothesis remains plausible, evidence is incomplete, and another test is affordable.
- **Scale:** criteria pass and the next increment has credible customer value or measured leverage.
- **Defer:** possible value, but timing, dependencies, distribution or opportunity cost make it inferior now.
- **Revisit trigger:** named event, customer signal, price/capability change, new evidence or date.

Stopping or deferring is a valid outcome. Avoid sunk-cost reasoning.

## Review cadence
- **Weekly (10–15 minutes):** capacity, blockers, evidence gained and whether active experiments still deserve time.
- **Monthly (30–60 minutes):** decisions, customer/revenue signals, risk changes and stop/continue/scale/defer calls.
- **Quarterly (60–90 minutes):** scenarios, assumptions, OSS/provider dependencies, portfolio balance and future signposts.
- **Event-driven:** after a security incident, material provider/license change, major customer signal, failed release or market shift.

Keep reviews proportional. If nothing changed, record that briefly rather than manufacturing activity.

## Scenario discipline
- **H1 — Competitive now (1–3 years):** customer outcomes, revenue, reliability, distribution and sustainable operations.
- **H2 — Bridge capabilities:** stable interfaces, modularity, permission-aware automation, data portability and replaceable dependencies where they help now or preserve options.
- **H3 — Near Future Vision:** plausible scenarios and signposts; prefer low-cost probes and reversible choices over speculative implementation.

For each strategic scenario, record its assumptions, observable signposts, implications if it strengthens, and decisions that should remain robust across scenarios. Dates are not guarantees.

## Success measure for NFV
NFV is useful when it improves decision quality: fewer unsupported commitments, faster discovery of fatal risks, more reliable estimates, clearer trade-offs, earlier stopping of weak bets, and better conversion of limited hours into customer value and reusable capability. Document count and score count are not success metrics.
