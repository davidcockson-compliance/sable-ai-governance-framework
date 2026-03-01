# Scout — ICO Recruitment AI Audit Gap Analysis (Worked Example)
**Project:** Sable AI Ltd — AI Governance Framework
**Stage:** Phase 2 — Worked Examples
**Status:** Draft
**Version:** v1
**Date:** 2026-03-01
**Assumptions:** Built on outline assumptions — not verified against real Sable AI Ltd data

---

## Purpose

This document is the Phase 2 worked example of `L2-3.3-ICO-Audit-Gap-Analysis-v1.md`. Where L2-3.3 assessed whether the Sable AI Ltd governance framework *addresses* each ICO finding in principle, this document assesses Scout's *operational compliance status* as of the date above.

The distinction is critical: all Phase 1 framework documents are now complete. A company that deploys this framework has a comprehensive set of compliance instruments. It does not yet have an operationally compliant system. This document maps the gap between those two positions for Sable AI Ltd.

**Framework completion status (as of this run):** All 14 Phase 1 documents are marked complete in `_agent-state/CHECKLIST-STATE.md`. The framework is now comprehensive at the document level.

**Scout operational compliance status:** Early-stage, partially implemented, with material gaps. This is the realistic posture described in the Phase 2 input brief for a post-seed startup that has written a framework but not yet completed all operational controls.

**Source documents used in this analysis:**
- ICO, *AI and Recruitment: Outcomes of our AI Audits*, November 2024 (hereafter: ICO Recruitment Outcomes Report) — primary benchmark
- Phase 2 input brief (`_input/phase-2-scout-technical-brief.md`) — Scout operational status
- `L2-3.3-ICO-Audit-Gap-Analysis-v1.md` — framework-level baseline

**How to read the gap status column:**

| Status | Meaning |
|---|---|
| **Framework complete** | A framework document fully addresses this ICO requirement. No further document work needed. |
| **Partially implemented** | A framework document and/or operational control exists but has not yet been executed, deployed, or confirmed for all in-scope customers or scenarios. |
| **Gap — action required** | No operational control currently exists. Framework document identifies the requirement but Scout does not yet fulfil it. |
| **Architectural gap** | A structural aspect of Scout's technical implementation conflicts with the ICO requirement, independent of framework document coverage. |

---

## About the ICO Audit

The ICO's November 2024 audit programme of AI recruitment tool providers produced 296 recommendations and 42 advisory notes across all audit engagements. 97% of recommendations were accepted and actioned. The audit covered CV screening, candidate scoring, and candidate sourcing tools — the exact product category Scout occupies.

Seven headline recommendation themes and three supplementary findings form the basis of this analysis.

---

## Part 1 — ICO Seven Key Recommendation Themes: Scout Status

### Theme 1 — Fair Processing: Bias and Accuracy Monitoring

**ICO finding:** Many providers monitored the accuracy and bias of their AI tools; however, some lacked adequate accuracy testing. Being "better than random" is insufficient to demonstrate fair processing. Providers must monitor for potential or actual fairness, accuracy, or bias issues and take appropriate action.

| ICO recommendation | Framework document | Framework status | Scout operational status | Overall |
|---|---|---|---|---|
| Monitor AI outputs for bias and accuracy issues | `L3-4.2-Bias-Monitoring-Protocol-v1.md` | Complete — full monitoring protocol defined | **No operational monitoring in place.** Current state: system prompt constraints only. No adverse impact analysis, no shortlisting rate tracking, no periodic review. [ASSUMPTION] | **Gap — action required** |
| Establish alert thresholds and escalation for bias signals | `L3-4.2-Bias-Monitoring-Protocol-v1.md` (Sections 5, 8, 9), `L3-4.1-Monitoring-Framework-v1.md` (M-13–M-15) | Complete — AIR thresholds (GREEN/AMBER/RED) and escalation path defined | No alert infrastructure exists. Threshold definitions are documented but not operationalised. | **Gap — action required** |
| Take corrective action when bias or accuracy issues are detected | `L3-4.2-Bias-Monitoring-Protocol-v1.md` (Section 10) | Complete — remediation options defined (prompt adjustment, exclusion, override, rollback, suspension) | No live monitoring → no basis for triggered remediation. Remediation process documented for future implementation. | **Partially implemented** |
| Demonstrate fairness beyond accuracy alone | `L1-2.2-Risk-Classification-Framework-v1.md`, `L3-4.2-Bias-Monitoring-Protocol-v1.md` | Complete — high-risk classification triggers obligation; bias protocol addresses broader fairness standard | Cannot currently demonstrate fairness beyond prompt-level constraints. No quantitative fairness evidence. | **Gap — action required** |

**Theme 1 Scout assessment:** The framework provides comprehensive methodology. Scout's current operational state does not meet ICO expectations. Bias monitoring is the highest-priority gap for a company at Scout's stage. See `P2-Scout-Bias-Monitoring-Protocol-v1.md` for Scout-specific implementation design.

---

### Theme 2 — Adequate and Accurate Bias Monitoring Data

**ICO finding:** Any special category data processed to monitor bias must be adequate and accurate. Inferred or estimated protected characteristic data (e.g., ethnicity inferred from a candidate's name) is not adequate and accurate, and will not comply with data protection law. The ICO found this to be a prevalent and unlawful practice.

| ICO recommendation | Framework document | Framework status | Scout operational status | Overall |
|---|---|---|---|---|
| Do not use inferred or estimated protected characteristic data for bias monitoring | `L3-4.2-Bias-Monitoring-Protocol-v1.md` (Section 3), `L2-3.2-Equality-Act-2010-Compliance-Map-v1.md`, `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` | Complete — prohibition confirmed; Option C (inferred data) explicitly not recommended | Scout does not currently use inferred demographic data. System prompt (Brief Section 5) instructs Claude not to consider or infer from any protected characteristic proxy. Compliance on this specific finding is the default position, not an active control. | **Partially implemented** |
| Use only adequate and accurate data sources for bias monitoring | `L3-4.2-Bias-Monitoring-Protocol-v1.md` (Section 3.4 — Option A voluntary monitoring preferred) | Complete — voluntary equality monitoring (Option A) is the recommended approach; aggregate statistical testing (Option B) as fallback | No demographic data collection of any kind exists. Option A not yet operationalised. The framework defines the right approach; Scout has not yet implemented it. | **Gap — action required** |
| Ensure a lawful basis exists before processing special category data for monitoring | `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` (Art. 9 analysis), `L2-3.4-DPIA-Template-v1.md` (Art. 9 risk documented) | Complete — legal constraint identified; permitted routes (Art. 6(1)(a) + Art. 9(2)(a)) documented | No special category data is currently processed — by default, no unlawful processing is occurring. However, no Art. 9 lawful basis is confirmed for the *future* voluntary monitoring pathway. [LEGAL REVIEW REQUIRED] | **Partially implemented** |

**Theme 2 Scout assessment:** Scout is currently compliant on this theme by omission — it does not process demographic data at all, so it cannot be unlawfully processing inferred data. However, this means it also cannot perform meaningful individual-level bias monitoring. The path forward requires confirming an Art. 9 route for voluntary equality monitoring before that capability can be added. [LEGAL REVIEW REQUIRED]

---

### Theme 3 — Transparency: Informing Candidates About AI Processing

**ICO finding:** Recruiters must inform candidates how their personal information will be processed by AI. The ICO found detailed privacy information for candidates to be consistently absent or inadequate. Providers should either supply this information or ensure recruiting organisations supply it.

| ICO recommendation | Framework document | Framework status | Scout operational status | Overall |
|---|---|---|---|---|
| Provide candidates with detailed privacy information about AI processing | `L4-5.2-Candidate-Transparency-Notice-v1.md` | Complete — template drafted in plain English, covering all ICO-required elements | Template exists. No confirmed deployment to any customer. Candidate-facing communication is a customer's operational obligation; Sable AI Ltd's obligation is to provide a contractually required template and support deployment. [ASSUMPTION A-003] | **Partially implemented** |
| Transparency notice must be provided before or at the point of CV processing | `L4-5.2-Candidate-Transparency-Notice-v1.md` (timing guidance included) | Complete — delivery timing addressed in notice template | No confirmed customer has deployed the notice. This represents a live compliance gap for any customer currently using Scout. | **Gap — action required** |
| Disclose that AI is used in candidate screening | `L4-5.2-Candidate-Transparency-Notice-v1.md`, `L1-2.4-Governance-Policy-v1.md` (transparency principle) | Complete — governance policy establishes the obligation; template delivers the candidate-facing statement | Template covers this requirement. Not confirmed as deployed. | **Partially implemented** |
| Address Arts. 13/14 UK GDPR disclosure requirements for ADM | `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` (disclosure obligations mapped) | Complete — Arts. 13, 14, 22(3) disclosure content specified | GDPR matrix defines obligations. Delivery mechanism (transparency notice template) is ready. Not confirmed as delivered to any candidate. | **Partially implemented** |

**Theme 3 Scout assessment:** The transparency gap is structurally simple to close — the tool (L4-5.2 template) exists and requires deployment through customer contracts. The ICO audit found this failing in nearly every provider audited. Given its prominence as an enforcement concern, deploying the transparency notice in all live customer agreements is a high-priority near-term action.

**Action required:** Include transparency notice deployment as a mandatory condition in all DPA agreements (L4-5.1) executed with customers. Track deployment status by customer.

---

### Theme 4 — Data Minimisation: Avoiding Excess Collection and Unlawful Repurposing

**ICO finding:** Tools that collected more personal information than necessary, and that repurposed candidate data — in some cases through social media scraping — without candidates' knowledge. The ICO confirmed this practice as unlawful.

| ICO recommendation | Framework document | Framework status | Scout operational status | Overall |
|---|---|---|---|---|
| Limit data collection to what is necessary for the stated purpose | `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` (Art. 5(1)(c) control), `L1-2.3-Data-Flow-Map-v1.md` | Complete — data minimisation obligations mapped; permissible data inputs defined | **Architectural gap:** Full CV text — including candidate PII (name, email, phone, address) — is passed to the Anthropic API with no pre-processing step to strip PII. Brief Section 6 confirms this. This is a documented violation of Art. 5(1)(c) data minimisation that is specific to Scout's technical implementation. The framework identifies it; Scout has not resolved it. | **Architectural gap** |
| Do not scrape or aggregate candidate data from social media | `L1-2.3-Data-Flow-Map-v1.md`, `L1-2.4-Governance-Policy-v1.md` (prohibited use cases) | Complete — Scout's defined data inputs are CVs submitted through customer workflows only; social scraping is outside scope | Scout does not scrape social media [ASSUMPTION A-002]. This finding does not apply to Scout's assumed architecture. | **Framework complete** |
| Do not repurpose candidate data incompatibly with original collection purpose | `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` (Art. 5(1)(b) purpose limitation) | Complete — purpose limitation addressed | No evidence of repurposing. Candidate data is used for the stated purpose of CV screening [ASSUMPTION]. | **Framework complete** |

**Theme 4 Scout assessment:** Scout largely passes the social scraping/aggregation concerns that motivated this ICO finding. The significant gap is architectural: passing full CV text including PII to Anthropic as a sub-processor without pre-processing violates the data minimisation principle. This gap was identified in `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` and `P2-Scout-UK-GDPR-Mapping-v1.md` and requires a specific engineering fix: a PII-stripping step before the Anthropic API call.

**Action required (engineering):** Implement a PII extraction and stripping layer in the FastAPI backend — process the CV text extracted by `pdfplumber`/`python-docx` through a named-entity recognition (NER) step to identify and remove or pseudonymise PII fields (name, address, phone, email) before constructing the Anthropic API call. This is the highest-priority engineering control for data minimisation compliance.

---

### Theme 5 — Risk Assessments: Completing DPIAs Adequately and Proactively

**ICO finding:** The majority of AI providers completed DPIAs before processing personal data. However, in many cases DPIAs were not sufficiently detailed — lacking data flow maps, inadequate necessity and proportionality assessments, and no consideration of alternative approaches.

| ICO recommendation | Framework document | Framework status | Scout operational status | Overall |
|---|---|---|---|---|
| Complete a DPIA before commencing processing | `L2-3.4-DPIA-Template-v1.md` | Complete — full DPIA template drafted with all ICO-required sections | DPIA template exists and references Scout's architecture at placeholder level [ASSUMPTION]. No evidence that a completed DPIA exists for Scout's current production deployment. [ASSUMPTION] | **Partially implemented** |
| DPIA must include a detailed data flow map | `L2-3.4-DPIA-Template-v1.md` (Section 1.7), `L1-2.3-Data-Flow-Map-v1.md` | Complete — dedicated data flow map document with end-to-end CV processing flow, Anthropic sub-processor path, and dual customer type paths | Framework content exists and provides the raw material for DPIA completion. | **Partially implemented** |
| DPIA must include meaningful necessity and proportionality assessment | `L2-3.4-DPIA-Template-v1.md` (Section 2) | Complete | Must be populated with real operational data before DPIA is complete. | **Partially implemented** |
| DPIA must be completed proactively, not retrospectively | `L2-3.4-DPIA-Template-v1.md` (completion timing note) | Complete | If Scout is processing live candidate data before a completed DPIA exists, this is a compliance failure. A completed DPIA must precede production processing. [ASSUMPTION] | **Gap — action required** |
| DPIA must be kept current when processing changes | `L1-2.4-Governance-Policy-v1.md` (review cycle), `L3-4.1-Monitoring-Framework-v1.md` (DPIA review triggers) | Complete | Governance policy specifies review obligation; monitoring framework defines triggers. No evidence this is operationally scheduled. | **Partially implemented** |

**Theme 5 Scout assessment:** The template infrastructure is comprehensive. The single critical action is populating and completing the DPIA using Scout's real architecture before live production candidate data is processed. The template (L2-3.4) already references Scout's technical stack in placeholder form — the gap is completing it with confirmed values rather than [ASSUMPTION] markers.

---

### Theme 6 — Role Allocation and Contracting

**ICO finding:** Several providers incorrectly identified themselves as processors rather than controllers. Contracts were vague, failed to specify data processed, party responsibilities, or end-of-contract handling.

| ICO recommendation | Framework document | Framework status | Scout operational status | Overall |
|---|---|---|---|---|
| Correctly identify controller/processor role | `L1-2.2-Risk-Classification-Framework-v1.md`, `L1-2.3-Data-Flow-Map-v1.md` | Complete — controller/processor determination documented for both customer types; joint controller question addressed for agency customers | Framework determination exists. No confirmed executed contracts that reflect this determination [ASSUMPTION A-003]. | **Partially implemented** |
| Contracts must specify what personal data is processed and how | `L4-5.1-Data-Processing-Agreement-Template-v1.md` (Appendix A — agency customer; Appendix B — in-house HR) | Complete — both DPA templates include full subject matter, data type, data subject, purpose, and retention specifications | DPA template exists. Not confirmed as executed with any customer. | **Gap — action required** |
| Contracts must specify responsibilities of each party | `L4-5.1-Data-Processing-Agreement-Template-v1.md`, `L1-2.5-Roles-and-Responsibilities-v1.md` | Complete | Internal RACI is complete. Customer-facing responsibilities in DPA template. Not yet executed. | **Gap — action required** |
| Contracts must address joint controller arrangement for agency customers | `L1-2.3-Data-Flow-Map-v1.md` (joint controller risk flagged), `L4-5.1-Data-Processing-Agreement-Template-v1.md` (Appendix A — Art. 26 arrangement) | Complete — Art. 26 clauses included in Appendix A [LEGAL REVIEW REQUIRED] | Clause structure exists but legal determination of whether agency customers constitute joint controllers has not been confirmed [LEGAL REVIEW REQUIRED]. | **Partially implemented** |
| Contracts must address what happens to data in AI models at termination | `L4-5.1-Data-Processing-Agreement-Template-v1.md` (end-of-contract provisions, Anthropic sub-processor chain) | Complete — deletion/return on termination specified; Anthropic obligations addressed | DPA template covers this. Not confirmed as executed. Additionally: Anthropic (as sub-processor) receives CV text on every API call; Sable AI Ltd does not retain extracted CV text post-call [ASSUMPTION A-005] — the termination data question for Anthropic is constrained by what Sable AI Ltd has confirmed with Anthropic about data handling. [LEGAL REVIEW REQUIRED] | **Partially implemented** |

**Theme 6 Scout assessment:** The DPA templates are complete and comprehensive. The entire gap on this theme is contractual execution — no confirmed DPA has been executed with any customer [ASSUMPTION A-003]. This must be remedied for all live customers before processing begins or continues.

---

### Theme 7 — Human Review of AI Outputs

**ICO finding:** Organisations should introduce robust and meaningful human review of AI outputs so that issues are addressed at an early stage. A feedback mechanism for recruiters to report errors should be implemented. AI tools should not make autonomous decisions beyond their designed scope.

| ICO recommendation | Framework document | Framework status | Scout operational status | Overall |
|---|---|---|---|---|
| Introduce robust and meaningful human review of AI outputs | `L1-2.2-Risk-Classification-Framework-v1.md` (Art. 22 safeguards), `L1-2.5-Roles-and-Responsibilities-v1.md` | Complete — mandatory human review before any candidate contact is a core design assumption [ASSUMPTION A-007] | Human review is built into Scout's workflow (Brief Section 3, Step 8): recruiter reviews AI shortlist and logs decision before any candidate contact. Recruiter can reject AI recommendation and log reason. This is an architectural control, not only a policy commitment. | **Partially implemented** |
| Implement feedback mechanism for recruiters to report AI errors | `L3-4.1-Monitoring-Framework-v1.md` (error-reporting mechanism), `L3-4.3-Incident-Response-Plan-v1.md` (escalation pathway) | Complete — monitoring framework and incident plan define the mechanism | Scout's UI includes a recruiter decision log with override capability (Brief Section 3, Step 8). Whether this constitutes a formal error-reporting mechanism requires confirmation. No dedicated error-reporting workflow outside override logging. | **Partially implemented** |
| Do not allow AI to make autonomous decisions beyond designed scope | `L1-2.2-Risk-Classification-Framework-v1.md`, `L1-2.4-Governance-Policy-v1.md` (approved/prohibited use cases) | Complete — Scout is defined as a shortlisting support tool; autonomous decision-making is a prohibited use case | Scout's UI design and workflow require human decision at Step 8 before any candidate contact [ASSUMPTION A-007]. The JSON output schema (`match_score`, `flags_for_human_review`) is designed to inform rather than replace recruiter judgement. | **Framework complete** |

**Theme 7 Scout assessment:** This is Scout's strongest compliance area. Human review is an architectural feature of the product, not only a policy commitment. The ICO finding that "recruiters should not use AI tools to make automated recruitment decisions where the AI is not designed for this purpose" is addressed by Scout's design. The remaining gap is confirming that the recruiter override logging constitutes a sufficient error-reporting mechanism — and potentially surfacing error reports through the monitoring framework for aggregate analysis.

---

## Part 2 — Supplementary ICO Findings: Scout Status

### 2.1 Discriminatory Search Functionality

**ICO finding:** Some tools enabled recruiters to filter out candidates with certain protected characteristics. The ICO found this to be a design-level discrimination risk.

| ICO finding | Framework document | Scout operational status | Overall |
|---|---|---|---|
| Do not build filtering functionality enabling exclusion by protected characteristic | `L1-2.4-Governance-Policy-v1.md` (prohibited use cases), `L2-3.2-Equality-Act-2010-Compliance-Map-v1.md` | Scout's assumed architecture (Brief Sections 3, 4) does not include recruiter-accessible filter controls. Scout outputs a ranked shortlist; filtering by any field requires custom recruiter action outside Scout's UI. [ASSUMPTION A-002] | **Framework complete** (subject to assumption verification) |
| Audit existing UI and API for unintended protected-characteristic filtering routes | `L3-4.1-Monitoring-Framework-v1.md` | Monitoring framework includes UI audit as a standing review item. No evidence that such an audit has been conducted on Scout's React frontend or API endpoints. [ASSUMPTION] | **Partially implemented** |

**Scout note:** The primary ICO concern about discriminatory search functionality was directed at sourcing tools, not inbound CV screeners. Scout's architecture does not match the concerning pattern. However, periodic UI and API reviews for unintended protected-characteristic filtering remain a standing obligation under `L3-4.1-Monitoring-Framework-v1.md`.

---

### 2.2 Inferred Protected Characteristic Data

**ICO finding:** Some providers inferred gender, ethnicity, and other characteristics from CV content or candidate names without a lawful basis. The ICO found this to be both methodologically unreliable and in most cases unlawful.

| ICO finding | Framework document | Scout operational status | Overall |
|---|---|---|---|
| Do not infer protected characteristics from name or CV content | `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` (Art. 9 prohibition), `L2-3.2-Equality-Act-2010-Compliance-Map-v1.md` (proxy risk) | Scout's system prompt explicitly instructs Claude not to consider, reference, or infer from any information that could be a proxy for a protected characteristic (Brief Section 5). This is a named engineering constraint at the prompt level. | **Partially implemented** |
| Engineering constraint must prevent inference of protected characteristics | Not currently confirmed as independently audited | The system prompt constraint is the primary control. However: (a) prompt-level controls cannot guarantee that implicit model associations do not influence scoring; (b) the instruction not to use protected characteristics does not prevent their influence if correlated with other CV features (Brief Section 5 acknowledges both limitations explicitly). Prompt-level controls have not been independently audited. | **Architectural gap** |
| Named engineering constraint prohibiting name-based ethnicity or age inference | `L3-4.2-Bias-Monitoring-Protocol-v1.md` (Section 3 — inference prohibition) | Prohibition is documented at protocol level. Operational verification (prompt audit, canary testing with demographically diverse CV sets) has not been confirmed. | **Partially implemented** |

**Scout note:** Scout's architecture places it at lower risk than the ICO's concern case (providers building inference features), but the limitation of prompt-level controls is explicitly acknowledged in the system design. This remains a live risk requiring periodic prompt audit and canary testing to confirm the constraint is operationally effective.

---

### 2.3 DPIA Completeness

**ICO finding:** DPIAs were in many cases not sufficiently detailed, particularly lacking data flow maps and meaningful necessity/proportionality assessments.

| ICO finding | Framework document | Scout operational status | Overall |
|---|---|---|---|
| DPIA must map data flows in detail | `L2-3.4-DPIA-Template-v1.md` (Section 1.7), `L1-2.3-Data-Flow-Map-v1.md` | Both documents exist and together constitute a comprehensive data flow record. Template provides the DPIA framing; data flow map provides the operational detail. | **Framework complete** |
| DPIA must address necessity and proportionality | `L2-3.4-DPIA-Template-v1.md` (Section 2) | Framework complete — pending DPIA completion with real values. | **Partially implemented** |
| DPIA must not be retrospective | `L2-3.4-DPIA-Template-v1.md` | Framework establishes this requirement. Operational compliance depends on when Scout completes and executes the DPIA relative to live production processing. [ASSUMPTION] | **Gap — action required** |

---

## Part 3 — Summary Table

| ICO Theme | Framework status | Scout operational status | Priority |
|---|---|---|---|
| 1. Fair processing / bias monitoring | Framework complete | **Gap — no operational monitoring** | P1 — Critical |
| 2. Adequate bias monitoring data | Framework complete | Partially implemented (no demographic data, no monitoring capability) | P1 — Critical |
| 3. Transparency to candidates | Framework complete | **Gap — template undeployed** | P1 — Critical |
| 4. Data minimisation | Framework complete | **Architectural gap — PII reaches Anthropic unstripped** | P1 — Critical |
| 5. DPIAs | Framework complete | **Gap — DPIA not completed for production** | P1 — Critical |
| 6. Role allocation / contracting | Framework complete | **Gap — no DPA executed with any customer** | P1 — Critical |
| 7. Human review | Framework complete | Partially implemented (architectural control exists; error reporting informal) | P2 — Important |
| 2.1 Discriminatory search | Framework complete | Low risk; UI audit not confirmed | P3 — Monitor |
| 2.2 Inferred characteristics | Framework complete | Architectural gap — prompt control not independently audited | P2 — Important |
| 2.3 DPIA completeness | Framework complete | Depends on DPIA completion (Theme 5) | P1 — Critical |

---

## Part 4 — Priority Action Register

The following actions are required to close the identified gaps. Ordered by priority. This register is the primary deliverable of this document for operational use.

### P1 — Critical (required before live production candidate processing)

| Ref | Action | Owner | Dependency | Framework reference |
|---|---|---|---|---|
| ACT-001 | Complete `L2-3.4-DPIA-Template-v1.md` using real Scout architecture data — not assumption-filled placeholders. DPIA must be signed off before Scout processes live candidate CVs. | CTO / Founder | None | `L2-3.4-DPIA-Template-v1.md` |
| ACT-002 | Execute `L4-5.1-Data-Processing-Agreement-Template-v1.md` (Appendix B minimum; Appendix A for agency customers) with all live and prospective customers before or concurrent with data processing commencing. | Founder / Customer Success Lead | Legal review of joint controller determination (Appendix A) | `L4-5.1-Data-Processing-Agreement-Template-v1.md` |
| ACT-003 | Deploy `L4-5.2-Candidate-Transparency-Notice-v1.md` as a customer obligation in all DPA agreements. Track deployment confirmation per customer. | Customer Success Lead | ACT-002 (DPA execution) | `L4-5.2-Candidate-Transparency-Notice-v1.md` |
| ACT-004 | Implement PII stripping in FastAPI backend: add a processing step between CV text extraction (`pdfplumber`/`python-docx`) and Anthropic API call construction to remove or pseudonymise candidate name, email, phone, and address fields. | Engineering Lead | None — independent engineering task | `L1-2.3-Data-Flow-Map-v1.md`, `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` |
| ACT-005 | Implement at minimum Option B bias monitoring (aggregate shortlisting rate statistical analysis using existing PostgreSQL audit logs) as an operational baseline before enabling Option A (voluntary demographic monitoring). | Engineering Lead / CTO | None — feasible with current Scout architecture and existing data | `P2-Scout-Bias-Monitoring-Protocol-v1.md` |

### P2 — Important (required within 3 months of launch)

| Ref | Action | Owner | Dependency | Framework reference |
|---|---|---|---|---|
| ACT-006 | Commission legal advice to confirm the lawful basis route (Art. 6(1)(a) + Art. 9(2)(a)) for voluntary equality monitoring (Option A in `L3-4.2`) and confirm whether this is a viable operational pathway. | Founder | ACT-002 (DPA execution) | `L3-4.2-Bias-Monitoring-Protocol-v1.md` (Section 3.4) |
| ACT-007 | Conduct initial keyword and proxy-feature audit of Scout's system prompt. Review for language patterns associated with demographic bias. Document findings. | CTO | None | `L3-4.2-Bias-Monitoring-Protocol-v1.md` (Section 6), `P2-Scout-Bias-Monitoring-Protocol-v1.md` |
| ACT-008 | Establish recruiter override rate monitoring using existing audit log data in PostgreSQL. Define alert threshold and assign monitoring owner. | Engineering Lead | Audit log verification — confirm override decisions are logged in structured form | `L3-4.1-Monitoring-Framework-v1.md` (M-06) |
| ACT-009 | Confirm Anthropic DPA (sub-processor agreement) is in place and meets UK GDPR Art. 28 requirements. Document sub-processor authorisation chain. | Founder / CTO | None | `L4-5.1-Data-Processing-Agreement-Template-v1.md` (sub-processor chain provisions), `L2-3.3-ICO-Audit-Gap-Analysis-v1.md` (Theme 6) |
| ACT-010 | Conduct independent prompt audit — engage external consultant to review Scout's system prompt for proxy feature bias. | CTO | ACT-007 (internal audit first) | `L3-4.2-Bias-Monitoring-Protocol-v1.md` (Section 11), `P2-Scout-Bias-Monitoring-Protocol-v1.md` |

### P3 — Monitor (required within 6 months, or when Scout expands)

| Ref | Action | Owner | Dependency | Framework reference |
|---|---|---|---|---|
| ACT-011 | Schedule periodic UI and API audit for unintended protected-characteristic filtering routes. Add as a standing item in quarterly monitoring review. | Engineering Lead | ACT-005 (monitoring baseline operational) | `L3-4.1-Monitoring-Framework-v1.md` |
| ACT-012 | Document candidate rights handling process — how Scout's customers respond to UK GDPR Arts. 15–22 rights requests from candidates. | Customer Success Lead / CTO | ACT-002 (DPA execution) | `L1-2.5-Roles-and-Responsibilities-v1.md` |
| ACT-013 | Develop and deliver AI governance training for all staff with access to Scout or candidate data. At minimum: UK GDPR obligations; Art. 22 ADM safeguards; bias awareness; incident reporting. | Founder | None | `L1-2.4-Governance-Policy-v1.md` (training requirement) |

---

## Items Requiring Legal Review Before Completion

- [LEGAL REVIEW REQUIRED] — Art. 9(2) condition for voluntary equality monitoring data (ACT-006). Legal sign-off required before implementing Option A demographic monitoring.
- [LEGAL REVIEW REQUIRED] — Joint controller determination for agency customer relationships. Appendix A of `L4-5.1` includes Art. 26 clauses; whether Sable AI Ltd's agency customers are joint controllers or sole controllers is a legal question requiring advice. Affects ACT-002.
- [LEGAL REVIEW REQUIRED] — Anthropic sub-processor DPA confirmation (ACT-009). The existence of a valid DPA with Anthropic is assumed (Assumption A-005) but has not been verified. UK GDPR Art. 28 requires a documented sub-processor agreement before personal data is shared with Anthropic.
- [LEGAL REVIEW REQUIRED] — Retention period confirmation. Proposed retention periods in the Phase 2 brief (Section 7) are unconfirmed [ASSUMPTION A-022]. These must be confirmed with legal advice before the DPIA (ACT-001) is finalised.

---

*This document should be updated each time Scout's operational compliance status changes. It is a living record, not a static assessment.*

---

*Cross-references: `L2-3.3-ICO-Audit-Gap-Analysis-v1.md` (framework baseline); `P2-Scout-Bias-Monitoring-Protocol-v1.md` (ACT-005 and ACT-007 implementation); `L4-5.1-Data-Processing-Agreement-Template-v1.md`; `L4-5.2-Candidate-Transparency-Notice-v1.md`; `L2-3.4-DPIA-Template-v1.md`; `L3-4.2-Bias-Monitoring-Protocol-v1.md`.*
