# Scout UK GDPR Compliance Mapping — Worked Example
**Project:** Sable AI Ltd — AI Governance Framework
**Stage:** Phase 2 — Worked Examples
**Status:** Draft
**Version:** v1
**Date:** 2026-03-01
**Assumptions:** Built on outline assumptions — not verified against real Sable AI Ltd data

---

## How to Use This Document

This is the **worked example version** of `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md`. Where the template provides an obligation framework and compliance approach placeholders, this document provides Scout's actual (assumed) compliance posture as at the date of this framework.

Status codes used throughout:

| Code | Meaning |
|---|---|
| ✓ Addressed | Control is in place and documented |
| ⚠ Partially addressed | Some controls exist but gaps remain |
| ✗ Gap | No control in place — action required |

All `[ASSUMPTION]` markers denote unverified assumptions about real Sable AI Ltd data. [LEGAL REVIEW REQUIRED] flags indicate conclusions that require qualified UK data protection legal advice before being treated as definitive.

**Cross-references:** `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` (template) | `L1-2.3-Data-Flow-Map-v1.md` (data flows) | `P2-Scout-System-Profile-v1.md` (system profile) | `L2-3.4-DPIA-Template-v1.md` (DPIA)

---

## 1. Compliance Posture Summary

Scout's compliance posture as at framework date: **early-stage, partially compliant, with identified gaps**. This is the realistic posture for a post-seed startup that has built a governance framework but not yet implemented all operational controls.

**Overall assessment:** The framework documents (Stages 1–5) are drafted and provide the architecture for compliance. The primary gaps are at the implementation layer — executed contracts, deployed operational processes, confirmed retention periods, and systematic bias monitoring.

| Area | Status | Priority |
|---|---|---|
| Lawful basis documentation | ✗ Gap | Critical |
| Special category data (Art. 9) | ✗ Gap | Critical |
| Data minimisation — PII to Anthropic | ✗ Gap | High |
| Art. 22 ADM safeguards — human review | ⚠ Partially addressed | High |
| DPIA completion | ⚠ Partially addressed | High |
| Data subject rights — operational process | ✗ Gap | High |
| Sub-processor DPA (Anthropic) | ⚠ Partially addressed | High |
| International transfer mechanism | ✗ Gap | High |
| Candidate transparency notice — deployment | ⚠ Partially addressed | Medium |
| Retention periods — confirmed and implemented | ✗ Gap | Medium |
| Accuracy and output monitoring | ⚠ Partially addressed | Medium |
| Accountability — audit logs | ⚠ Partially addressed | Medium |

---

## 2. Compliance Mapping Matrix

### 2.1 Lawful Basis (UK GDPR Art. 6)

| Processing activity | Lawful basis candidate | Scout approach | Status | Action required |
|---|---|---|---|---|
| CV screening on behalf of recruitment agency customer | Legitimate interests of the agency and its employer client (to identify suitable candidates for the role) — or contract performance (between agency and employer client) [LEGAL REVIEW REQUIRED] | No lawful basis has been documented for any specific customer deployment. The DPA template (L4-5.1, App. A) addresses the controller/processor split but does not specify or confirm the lawful basis the agency customer relies on. | ✗ Gap | Sable AI Ltd should require each agency customer to document and confirm the lawful basis it relies on in its customer DPA. Sable AI Ltd cannot establish the lawful basis as processor — this is the controller's (customer's) responsibility. |
| CV screening on behalf of in-house HR customer | Legitimate interests of the employer (to recruit for the role) — or contract performance (between employer and candidate, where a job application implies processing) [LEGAL REVIEW REQUIRED] | As above — no lawful basis documented. DPA template (L4-5.1, App. B) does not specify. | ✗ Gap | As above — in-house HR customer must confirm its lawful basis. Sable AI Ltd's DPA template should require this confirmation. |
| Sable AI Ltd's own processing (system operation, audit logging, service delivery) | Legitimate interests of Sable AI Ltd (to provide the contracted service, maintain system integrity, and meet accountability obligations) [LEGAL REVIEW REQUIRED] | Not documented. No legitimate interests assessment (LIA) has been conducted. | ✗ Gap | Conduct a documented Legitimate Interests Assessment for Sable AI Ltd's own processing operations. |

**Key issue:** Sable AI Ltd operates as a data processor for its customers. The lawful basis for processing candidate CVs is the controller's (customer's) responsibility to establish. However, Sable AI Ltd must ensure its customer contracts require this confirmation, and must have a documented lawful basis for its own operational processing. Neither is currently in place.

---

### 2.2 Special Category Data (UK GDPR Art. 9)

[LEGAL REVIEW REQUIRED] This section addresses the most significant unresolved compliance risk in the Scout processing model.

| Risk | Detail | Status | Action required |
|---|---|---|---|
| Health data in CV text | Candidate CVs routinely contain health information: disability disclosures, mental health references, pregnancy/maternity gaps, long-term illness indicators. This information appears as free text and is extracted verbatim by Scout's parser. | ✗ Gap | An explicit Art. 9 condition must be established for processing health data, OR a technical control must prevent health-related content from reaching the Anthropic API. Neither is in place. |
| Ethnicity and racial origin — inference risk | Candidate name, educational institution, residential history, and cultural references in CVs can correlate with racial or ethnic origin. Even if Claude is instructed not to use such information as scoring inputs, the data is transmitted to and processed by Anthropic's systems. The instruction not to use the data does not constitute a lawful basis for processing it under Art. 9. | ✗ Gap | An explicit Art. 9 condition must be established, or a PII/demographic data detection and exclusion step must be implemented before API transmission. This is the most legally uncertain area of the Scout processing model. |
| Biometric and criminal conviction data | Less likely in standard CV content but possible in some recruitment contexts (e.g., DBS check references). Not specifically addressed. | ✗ Gap | Assess whether any customer use cases could introduce biometric or conviction data into CV uploads. Amend system prompt and DPA to prohibit or address. |
| Scout system prompt exclusion — does it resolve Art. 9? | The Scout system prompt instructs Claude not to consider protected characteristic proxies when scoring. **This does not resolve the Art. 9 issue.** Art. 9 governs the *processing* of special category data — including receipt and transmission — regardless of whether the data is used as a scoring input. The data still reaches Anthropic's systems and is processed in the context of the API call. | N/A — legal analysis | [LEGAL REVIEW REQUIRED] A UK data protection lawyer must advise on whether the combination of system prompt exclusions and technical controls is sufficient, or whether explicit Art. 9 conditions and/or a pre-processing PII stripping step are required. |

---

### 2.3 Data Minimisation (UK GDPR Art. 5(1)(c))

| Processing step | Data minimisation obligation | Scout approach | Status | Action required |
|---|---|---|---|---|
| CV text extraction | Extract only information relevant to job screening | `pdfplumber` / `python-docx` extracts the full CV text verbatim — name, address, email, phone, all personal narrative, all employment history. No filtering or minimisation at extraction stage. | ⚠ Partially addressed — raw file not forwarded to Anthropic, only extracted text | Consider whether a structured extraction step (extracting only work history and skills, omitting contact details and personal narrative) could reduce data volume sent to Anthropic. |
| Anthropic API call | Send only data necessary for screening assessment | Full extracted CV text, including all candidate PII (name, email, phone, address) is included in the user message sent to Anthropic. There is no pre-processing step to strip PII before API transmission. [ASSUMPTION: A-030] | ✗ Gap — confirmed compliance gap | Implement a PII-stripping or redaction step before sending CV text to Anthropic. At minimum, strip name, email, phone number, and home address before API call — these are not relevant to skills-based job matching. This is a documented action item from L2-3.1-UK-GDPR-Mapping-Matrix-v1.md. |
| S3 file storage | Store raw files only as long as necessary | Raw CV files stored in S3 encrypted. Retention period proposed (6 months) but not confirmed or implemented. [ASSUMPTION: A-022] | ⚠ Partially addressed — encryption in place; retention not implemented | Confirm retention period and implement automated S3 deletion. |
| RDS output storage | Store AI output only as long as necessary | AI output JSON stored in RDS. Retention proposed (6 months) but not implemented. | ⚠ Partially addressed | As above — implement automated deletion. |

---

### 2.4 Accuracy (UK GDPR Art. 5(1)(d))

| Processing step | Accuracy obligation | Scout approach | Status | Action required |
|---|---|---|---|---|
| CV text extraction | Extracted text should accurately represent CV content | Extraction quality depends on CV formatting. Structured CVs extract well; scanned image PDFs, tables, or non-standard layouts may extract poorly. Poor extraction could materially affect AI scoring. | ⚠ Partially addressed | Log extraction confidence/completeness. Flag candidates where extraction quality is uncertain. |
| AI screening output | AI assessment should accurately reflect CV content against job criteria | Claude may hallucinate or mischaracterise CV content. `flags_for_human_review` field and `score_confidence` field partially mitigate this. Human review step provides a further check. | ⚠ Partially addressed | No accuracy audit of AI outputs has been conducted. Implement recruiter override rate monitoring as a proxy accuracy signal. Conduct periodic manual review of AI outputs against CVs. |
| AI output storage | Stored outputs should remain accurate and not be updated without logging | AI outputs are immutable once stored (recruiter notes added separately in `assessor_notes` field). Model version recorded. | ✓ Addressed | — |

---

### 2.5 Storage Limitation (UK GDPR Art. 5(1)(e))

| Data type | Proposed retention | Status | Action required |
|---|---|---|---|
| Raw CV files (S3) | 6 months after role filled or application closed [ASSUMPTION: A-022] | ✗ Gap — proposed only, not implemented | Legally confirm, document, and implement automated deletion |
| AI output JSON (RDS) | 6 months after role filled or application closed [ASSUMPTION: A-022] | ✗ Gap | As above |
| Recruiter decisions and notes (RDS) | 6 months after role filled or application closed [ASSUMPTION: A-022] | ✗ Gap | As above |
| Audit logs (RDS) | 2 years [ASSUMPTION: A-022] | ✗ Gap | As above |
| Job description records (RDS) | Duration of customer contract + 1 year [ASSUMPTION: A-022] | ✗ Gap | As above |
| Anthropic API (CV text in-context) | Not retained post-call [ASSUMPTION: A-005] | ⚠ Partially addressed | Confirm with Anthropic DPA that in-context data is not retained post-response |

---

### 2.6 Automated Decision-Making (UK GDPR Art. 22)

[LEGAL REVIEW REQUIRED] The Art. 22 threshold question — whether Scout constitutes a "solely automated decision with legal or significant effect" — is the most consequential legal question in this mapping.

| Question | Analysis | Status | Action required |
|---|---|---|---|
| Does Scout make decisions with legal or significant effect? | Scout's output is a ranked shortlist with match scores. Candidates not shortlisted are effectively excluded from progressing. Being excluded from a job application has a "significant effect" on a candidate's opportunities. The ICO takes the view that AI shortlisting in recruitment has significant effect on candidates. | Assessment: Yes — significant effect likely | [LEGAL REVIEW REQUIRED] |
| Is the decision "solely automated"? | The Scout workflow includes a mandatory human review step before any candidate is contacted or advanced [ASSUMPTION: A-007]. If this review is genuine — i.e., the recruiter can and does override Scout's recommendations — the decision is not "solely automated" and full Art. 22 does not apply. If the human step is effectively rubber-stamping AI outputs, the ICO may treat it as solely automated in practice. | Assessment: Human review step in place, but not independently verified as genuine [ASSUMPTION: A-007] | Verify that human review step is technically enforced (not just a policy requirement). Document recruiter override rate. |
| Art. 22C safeguards (applicable regardless) | Even if Art. 22 full prohibition does not apply (because of genuine human review), Art. 22C safeguards are relevant best practice and likely required by ICO guidance: (1) right to obtain human review; (2) right to express views; (3) right to contest the outcome. | ⚠ Partially addressed — human review exists; candidate-facing rights process not documented | Implement an operational process for candidates to request human review and contest AI-assisted assessments. Document this in candidate transparency notice. |
| Right to explanation of logic | UK GDPR Art. 15(1)(h) gives data subjects the right to "meaningful information about the logic involved" in automated processing that significantly affects them. Scout's AI output JSON (accessible in the UI) provides matched/unmet criteria and summary — this partially addresses the logic explanation requirement. But a candidate-facing explanation process does not exist. | ⚠ Partially addressed | Define and document how Scout will respond to Art. 15(1)(h) requests for explanation. |

---

### 2.7 DPIA (UK GDPR Art. 35)

| Requirement | Detail | Status | Action required |
|---|---|---|---|
| DPIA required? | ICO confirms AI in recruitment triggers a DPIA. Scout processes personal data of large numbers of candidates; it uses an automated system to systematically evaluate natural persons against job criteria. Both ICO DPIA trigger criteria are met. | ✓ Confirmed | — |
| DPIA template | `L2-3.4-DPIA-Template-v1.md` provides the full DPIA structure. | ✓ Template exists | — |
| Deployment-specific DPIA | A DPIA must be completed for Scout's specific processing, not just as a template. The template must be populated with confirmed technical details, risk assessments, and residual risk sign-off. [ASSUMPTION: A-021] | ✗ Gap — not completed | Complete deployment-specific DPIA using `L2-3.4-DPIA-Template-v1.md`. This is a pre-launch obligation. |
| ICO prior consultation (Art. 36) | If the DPIA concludes that residual risk cannot be mitigated, Art. 36 requires consultation with the ICO before processing. | To be determined at DPIA completion | Complete DPIA first; assess whether Art. 36 consultation is required. |

---

### 2.8 Sub-Processor Obligations (UK GDPR Art. 28)

| Obligation | Anthropic as sub-processor | Status | Action required |
|---|---|---|---|
| Written authorisation | Processor (Sable AI Ltd) may only engage sub-processors with controller's (customer's) written authorisation (Art. 28(2)). | ⚠ Partially addressed — DPA template (L4-5.1) includes sub-processor provision; not confirmed as executed with any customer | Ensure customer DPAs explicitly authorise Anthropic as sub-processor. |
| Flow-down obligations | Sub-processor must be subject to the same data protection obligations as the processor (Art. 28(4)). | ⚠ Partially addressed — DPA template requires flow-down; Anthropic DPA not confirmed as executed [ASSUMPTION: A-005] | Execute Data Processing Agreement with Anthropic. Confirm it meets UK GDPR Art. 28 requirements. |
| AWS as sub-processor | AWS provides compute, storage, and database infrastructure in eu-west-2. | ⚠ Partially addressed — assumed to be covered by AWS Data Processing Addendum; not confirmed | Confirm AWS DPA is in place and covers eu-west-2 processing. |

---

### 2.9 International Transfers (UK GDPR Art. 46)

| Transfer | Detail | Status | Action required |
|---|---|---|---|
| Candidate CV text → Anthropic (US) | Anthropic, PBC is incorporated in the United States. API calls from Scout's AWS eu-west-2 backend transmit extracted CV text (including PII) to Anthropic's API infrastructure, which may be located outside the UK. This constitutes a restricted international transfer under UK GDPR if Anthropic processes the data on non-UK servers. [LEGAL REVIEW REQUIRED] | ✗ Gap — transfer mechanism not confirmed [ASSUMPTION: A-027] | Confirm the applicable UK international transfer mechanism. Options include: (1) UK-US Data Bridge (if Anthropic is certified under the UK Extension to the EU-US Data Privacy Framework); (2) International Data Transfer Agreement (IDTA); (3) UK Addendum to EU Standard Contractual Clauses. This must be resolved before processing candidate data via the Anthropic API at commercial scale. |
| Scout backend ↔ AWS eu-west-2 | All Scout compute and storage is in AWS eu-west-2 (London region). UK data, processed within UK. [ASSUMPTION: A-004] | ✓ No international transfer — UK region | Confirm actual AWS region configuration. |

---

### 2.10 Data Subject Rights (UK GDPR Arts. 12–22)

| Right | Candidate relevance | Status | Action required |
|---|---|---|---|
| Art. 13/14 — transparency | Candidates must be informed about Scout's processing before or at the point of data collection. Candidate transparency notice template exists (`L4-5.2`). | ⚠ Partially addressed — template exists; customer deployment not confirmed | Require customer DPAs to confirm deployment of transparency notice to candidates. |
| Art. 15 — access | Candidates can request a copy of their personal data held by Scout and access to the AI output produced about them. | ✗ Gap — no operational process | Document and implement an Art. 15 request process. Define how requests from candidates reach Sable AI Ltd vs. the customer controller. |
| Art. 17 — erasure | Candidates can request deletion of their data (subject to retention obligations). No automated deletion is in place. | ✗ Gap | Implement automated deletion aligned with retention periods. Document erasure request process. |
| Art. 21 — object to legitimate interests processing | If legitimate interests is the lawful basis, candidates have the right to object to processing on grounds relating to their particular situation. | ✗ Gap — lawful basis not confirmed; no objection process | Resolve lawful basis first. Then document objection handling process. |
| Art. 22C — human review, contest AI outcome | Candidates have the right to request human review of AI-assisted assessments, express their views, and contest outcomes. | ⚠ Partially addressed — human review step exists [ASSUMPTION: A-007]; no candidate-facing process for requesting it or contesting outcomes | Implement and publicise candidate rights process. Include in transparency notice. |
| Art. 15(1)(h) — explanation of logic | Data subjects have the right to meaningful information about the logic of automated processing that significantly affects them. | ⚠ Partially addressed — AI output JSON captures matched/unmet criteria (partial logic explanation); no candidate-facing explanation process | Define explanation format and delivery process for Art. 15(1)(h) requests. |

---

## 3. Action Register (Prioritised)

| Priority | Action | Owner (proposed) | Framework reference |
|---|---|---|---|
| Critical | Establish Art. 9 lawful basis for special category data in CV text, OR implement PII/demographic data stripping before Anthropic API calls | CTO / Legal | Section 2.2 |
| Critical | Confirm international transfer mechanism for Anthropic API calls (UK → US) | CTO / Legal | Section 2.9 |
| Critical | Document lawful basis for all processing activities. Require customers to confirm lawful basis in DPAs. | CEO / Legal | Section 2.1 |
| High | Execute Data Processing Agreement with Anthropic. Confirm UK GDPR Art. 28 compliance and international transfer mechanism. | CTO / Legal | Section 2.8 |
| High | Implement PII stripping before Anthropic API calls — at minimum strip name, email, phone, address from extracted CV text | Engineering Lead | Section 2.3 |
| High | Complete deployment-specific DPIA using `L2-3.4-DPIA-Template-v1.md` | CTO / DPO | Section 2.7 |
| High | Implement operational data subject rights process (Art. 15, 17, 21, 22C requests) | Customer Success / CTO | Section 2.10 |
| High | Confirm and implement retention periods with automated deletion jobs | Engineering Lead | Section 2.5 |
| Medium | Verify human review step is technically enforced — not just a policy commitment. Document recruiter override rate. | Engineering Lead | Section 2.6 |
| Medium | Require customer DPAs to confirm deployment of candidate transparency notice | Customer Success | Section 2.10 |
| Medium | Conduct extraction quality monitoring — log and flag poor-quality CV extractions | Engineering Lead | Section 2.4 |
| Medium | Conduct Legitimate Interests Assessment for Sable AI Ltd's own operational processing | CTO / Legal | Section 2.1 |

---

## 4. Items Requiring Legal Review Before Implementation

The following conclusions in this document are flagged [LEGAL REVIEW REQUIRED] and must not be treated as definitive legal advice:

1. **Art. 22 threshold** — Whether Scout constitutes "solely automated decision-making with legal or significant effect" given the human review step (Section 2.6)
2. **Art. 9 special category data** — Whether the system prompt exclusion is sufficient, or whether an explicit Art. 9 condition and/or PII stripping is required (Section 2.2)
3. **Lawful basis** — The appropriate Art. 6 basis for each customer type and each processing activity (Section 2.1)
4. **International transfer mechanism** — The applicable UK transfer mechanism for Anthropic API calls (Section 2.9)
5. **Art. 26 vs Art. 28** — The controller/processor characterisation for agency customers (forthcoming — see `L4-5.1-Data-Processing-Agreement-Template-v1.md`)

[LEGAL REVIEW REQUIRED] This entire document should be reviewed by a qualified UK data protection lawyer before use in an operational compliance programme.

---

*This document is a worked example for educational purposes. It is not a legal compliance certification. All assumptions must be verified against real Sable AI Ltd data before operational use.*
