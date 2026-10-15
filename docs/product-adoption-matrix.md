# NFV Product Adoption Matrix

This is a **planning template**, not a claim that these products have been audited. Inspect repositories/runtime evidence before assigning status or scores.

Status values: **Unknown** (not assessed), **Present** (code/config appears to exist; not verified end-to-end), **Verified** (tests/environment prove it), **Gap** (missing/below standard), **N/A** (justified by product requirements/risk).

| Product/asset | Customer value | Security/isolation | API/contracts | AI ops | Automation/approvals | Context/knowledge | Observability/recovery | Modularity/deploy | Next action |
|---|---|---|---|---|---|---|---|---|---|
| Alamia Core | Unknown | Unknown | Unknown | Shared contract to assess | Unknown | Unknown | Unknown | Unknown | Inspect modules, tenancy, contracts, tests and deployment. |
| Alamia AI Gateway | Unknown | Unknown | Unknown | Unknown | N/A at gateway layer; caller policy still required | Integration point | Unknown | Unknown | Verify provider abstraction, usage records, fallback, timeout and redaction. |
| Alamia Copilot/Automation | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Verify role/skill/tool contracts, permission checks, approvals and action audit. |
| QRSello | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Inspect repository; prioritize revenue-driving and minimum safe-release gaps. |
| TravelOS | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Check tenant scope, booking states, integration reliability, deadlines and approvals. |
| Gharly | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Check financial calculations, privacy, import deduplication, restore/export and payment scope. |
| ZelixVet | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Check clinical-data access/audit, vet review of generated notes, billing/inventory and AI boundaries. |
| Other: ______ | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | Unknown | ______ |

## Product assessment worksheet
- Product/repository URL:
- Owner and assessor:
- Assessment date / commit / release:
- Product type, target users and critical workflows:
- Business outcome and current evidence:
- Data classification and regulatory/contractual constraints:
- Tenancy model and trust boundaries:
- Deployment topology and operations owner:
- External providers and dependency inventory:
- NFV score with evidence links:
- Critical gates: PASS / FAIL / UNKNOWN with evidence:
- Top three gaps ranked by risk and business value:
- OSS candidates to adopt/adapt/trial/build/reject:
- Resource and monthly operating cost estimate:
- Next 30-day milestone, owner and success criteria:
- Future options preserved but not implemented:

## Product risk prompts
- **TravelOS:** user/agency authorization; booking/payment state consistency; third-party dependencies; deadlines and human review; automation audit.
- **Gharly:** financial correctness; export/backup; privacy of SMS/bank-derived data; duplicate imports; consent and access control.
- **ZelixVet:** clinical privacy and audit; vet review of generated notes; billing/inventory integrity; human control of consequential workflows.
- **QRSello:** validate customer problem and paid demand before speculative NFV work delays a sellable release.
- **Shared Alamia foundations:** optional capabilities must not force heavy infrastructure on every product.
