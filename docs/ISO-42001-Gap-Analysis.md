# ISO/IEC 42001:2023 Gap Analysis

**Framework:** Sable AI Governance Framework  
**Standard:** ISO/IEC 42001:2023 — Artificial Intelligence Management System (AIMS)  
**Date:** 2026-05-02  
**Status:** Draft — for review  
**Branch:** claude/iso-42001-compliance-mapping-TVNHO

---

## Purpose

This document identifies what is currently present in the Sable AI Governance Framework, what is missing relative to ISO/IEC 42001:2023, and what needs to be created or adapted to achieve structural alignment with the standard.

The framework currently maps well to UK regulatory law (UK GDPR, DPA 2018, Equality Act 2010, ICO guidance) but is not structured around the ISO 42001 AIMS management system. This analysis provides a clear action list to close that gap.

---

## Overall Coverage Summary

| Area | Current Status |
|------|----------------|
| UK Regulatory Compliance | Strong — purpose-built for UK law |
| ISO 42001 Core Clauses (Cl.4–10) | ~55% covered — gaps in Cl.4, Cl.7, Cl.9, Cl.10 |
| ISO 42001 Annex A Controls (38 controls) | ~50% covered — gaps in A.4, A.5, A.6, A.7, A.9 |
| ISO 42001 Cross-Reference Mapping | Not present — no explicit clause-to-document mapping |

---

## Part 1: Core Clauses Gap Analysis

### Clause 4 — Context of the Organization

**What ISO 42001 requires:**
- A documented statement defining the scope of the AIMS (which systems, processes, and organisational boundaries are covered)
- An analysis of internal and external issues that affect AI objectives (legal, ethical, technical, competitive)
- An inventory of interested parties (stakeholders) and their relevant requirements

**What currently exists:**
- `STAGE1-Regulatory-Orientation-Note-v1.md` — covers external regulatory context
- `L1-2.1-AI-System-Inventory-v1.md` — partially addresses system scope

**What is missing:**

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-01 | AIMS Scope Statement | A single, formal document defining the boundary of the management system: which AI systems are in scope, which organisational units are covered, and any explicit exclusions with justification |
| G-02 | Interested Parties Register | A structured register of all stakeholders (candidates, clients/employers, regulators, data processors, board, staff) with their requirements and expectations relevant to the AIMS |
| G-03 | Internal & External Issues Analysis | A documented analysis (e.g. PESTLE or equivalent) of the issues — ethical, legal, technical, market — that affect the organisation's ability to achieve its AI objectives |

---

### Clause 5 — Leadership

**What ISO 42001 requires:**
- Evidence that top management is accountable for the AIMS
- A formal AI Policy signed/endorsed by leadership
- Explicit assignment of AIMS roles and responsibilities
- Documented commitment of resources

**What currently exists:**
- `L1-2.4-Governance-Policy-v1.md` — comprehensive AI governance policy
- `L1-2.5-Roles-and-Responsibilities-v1.md` — governance roles defined

**What is missing:**

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-04 | Top Management Commitment Statement | A formal, dated statement signed by the accountable executive confirming commitment to the AIMS, the AI Policy, and the resources allocated. This is a common audit evidence request. |

---

### Clause 6 — Planning

**What ISO 42001 requires:**
- A process to identify and address AI-specific risks and opportunities
- A mandatory **AI System Impact Assessment (ASIA)** evaluating effects on individuals, groups, and society — distinct from a GDPR DPIA
- Risk treatment plans with owners and timelines

**What currently exists:**
- `L1-2.2-Risk-Classification-Framework-v1.md` — risk identification and classification
- `L2-3.4-DPIA-Template-v1.md` — GDPR Data Protection Impact Assessment

**What is missing:**

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-05 | AI System Impact Assessment (ASIA) Template | A standalone assessment instrument (separate from the DPIA) that evaluates broader societal, group, and individual impacts of each AI system — including effects on protected characteristics, labour markets, and fairness. Maps to Clause 6 and A.5. |
| G-06 | Risk Treatment Plan | A documented plan linking identified risks to specific controls, owners, target dates, and residual risk acceptance decisions. The Risk Classification Framework identifies risk tiers but does not assign treatments and owners per system. |

---

### Clause 7 — Support

**What ISO 42001 requires:**
- Documented resources allocated to operating the AIMS
- Competence requirements and records for personnel with AIMS roles
- An awareness programme so relevant staff understand AI policy and their obligations
- A procedure for controlling documented information (version control, retention, access, disposition)

**What currently exists:**
- `L1-2.1-AI-System-Inventory-v1.md` — touches on system resources
- `L1-2.3-Data-Flow-Map-v1.md` — covers data resources

**What is missing:**

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-07 | Competence & Training Framework | A document defining the AI-related competence requirements for each AIMS role, how competence is assessed, records of training completed, and how gaps are addressed. |
| G-08 | Staff Awareness Programme | A defined programme (content, audience, frequency, delivery method) ensuring that all relevant personnel understand the AI Policy, their responsibilities, and the consequences of non-conformance. |
| G-09 | Documented Information Control Procedure | A procedure governing how all AIMS documents are created, reviewed, approved, versioned, stored, retained, and disposed of. This is a standard requirement across all ISO management systems. |

---

### Clause 8 — Operation

**What ISO 42001 requires:**
- Planned and controlled operational processes for the AI lifecycle
- Controls covering design, development, testing, deployment, monitoring, and decommissioning of AI systems
- Periodic re-assessment of risk and impact for systems already in operation

**What currently exists:**
- `L1-2.3-Data-Flow-Map-v1.md` — covers data flows across the lifecycle
- `L1-2.2-Risk-Classification-Framework-v1.md` — covers risk tiers
- `L3-4.1-Monitoring-Framework-v1.md` — covers post-deployment monitoring

**What is missing:**

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-10 | AI Lifecycle Controls Document | A document defining the required controls and checkpoints at each stage of the AI lifecycle: problem definition, data acquisition, model development, testing/validation, deployment approval, live monitoring, and decommissioning. Maps to A.6. |
| G-11 | Periodic Re-assessment Procedure | A procedure specifying how and when existing AI systems undergo re-assessment of risk and impact (e.g. triggered by significant model change, data drift, incident, or on a fixed schedule). |

---

### Clause 9 — Performance Evaluation

**What ISO 42001 requires:**
- Defined AI performance metrics and monitoring methods
- A scheduled **internal audit programme** assessing AIMS conformance — independent of the area being audited
- A **management review** meeting/process with defined inputs and outputs, conducted at planned intervals

**What currently exists:**
- `L3-4.1-Monitoring-Framework-v1.md` — strong on AI performance metrics
- `L2-3.3-ICO-Audit-Gap-Analysis-v1.md` — point-in-time gap analysis against ICO expectations

**What is missing:**

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-12 | Internal Audit Procedure | A procedure defining audit objectives, scope, criteria, frequency, auditor independence requirements, evidence collection, and reporting. This must cover the AIMS itself (processes, controls, documents), not just AI system performance. |
| G-13 | Internal Audit Schedule / Programme | A rolling schedule of planned AIMS audits, stating which clauses/controls are audited, by whom, and when. |
| G-14 | Management Review Procedure | A procedure defining the management review agenda (inputs: audit results, incidents, KPIs, risk status; outputs: decisions on resources, policy changes, improvement actions), frequency, and record requirements. |

---

### Clause 10 — Improvement

**What ISO 42001 requires:**
- A process for identifying, recording, and resolving nonconformities
- A corrective action procedure that addresses root cause, not just symptoms
- A commitment to continual improvement of AIMS effectiveness

**What currently exists:**
- `L3-4.3-Incident-Response-Plan-v1.md` — covers security/data incidents well

**What is missing:**

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-15 | Nonconformity & Corrective Action Procedure | A procedure (and associated log/register) for recording any instance where the AIMS fails to meet a requirement, conducting root-cause analysis, defining corrective actions, assigning owners and deadlines, and verifying effectiveness. Distinct from incident response — covers AIMS process failures, not just operational incidents. |
| G-16 | Continual Improvement Log | A living register of improvement opportunities identified through audits, management reviews, incident analysis, and monitoring. Provides the audit trail of AIMS maturity over time. |

---

## Part 2: Annex A Controls Gap Analysis

### A.2 — AI Policies

**Status: Covered**  
`L1-2.4-Governance-Policy-v1.md` meets the core intent. Ensure the policy is reviewed on a defined cycle (recommend annually) and that a review log is maintained.

---

### A.3 — Internal Organisation

**Status: Covered**  
`L1-2.5-Roles-and-Responsibilities-v1.md` meets this control. Ensure it includes a mechanism for staff to raise concerns about AI systems (whistleblowing/escalation pathway).

**Minor gap:**

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-17 | AI Concern / Escalation Pathway | A documented, accessible route for staff to raise concerns about AI system behaviour, bias, or ethical issues without fear of reprisal. Can be a section within the Roles document or a standalone procedure. |

---

### A.4 — Resources for AI

**Status: Partial**  
System inventory and data flow map cover AI systems and data. Computing infrastructure, third-party tooling, and human resource capacity are not explicitly documented as AIMS resources.

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-18 | AI Resource Register | A register documenting the key resources required to operate each AI system: compute infrastructure, third-party models/APIs, data sources, and the human roles required. Supports resource planning and supplier risk management. |

---

### A.5 — Impact Assessment

**Status: Gap — critical**  
The DPIA Template (`L2-3.4`) addresses data protection impacts under UK GDPR. ISO 42001 A.5 requires a separate AI System Impact Assessment covering broader harms: effects on protected groups, labour market impact, societal fairness, and access to services.

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-05 | AI System Impact Assessment (ASIA) Template | *(See Clause 6 above — same gap.)* The ASIA should be a formal template applied to each AI system in scope, with outputs retained as documented evidence. |

---

### A.6 — AI Lifecycle

**Status: Gap**  
No single document defines the required controls at each lifecycle stage.

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-10 | AI Lifecycle Controls Document | *(See Clause 8 above — same gap.)* Should cover: requirements definition, data acquisition controls, development standards, testing/validation criteria, deployment gates, live monitoring requirements, and decommissioning. |

---

### A.7 — Data for AI Systems

**Status: Partial**  
`L1-2.3-Data-Flow-Map-v1.md` maps data flows and sources well. However, there is no documented procedure for validating data quality before use in AI systems, and no data lineage/provenance tracking mechanism.

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-19 | Data Quality & Provenance Procedure | A procedure defining how data used in AI training and inference is assessed for quality (completeness, accuracy, representativeness, bias), how provenance (origin, transformations, consent basis) is recorded and tracked, and how data preparation decisions are documented. |

---

### A.8 — Information for Parties Using AI Systems

**Status: Mostly covered**  
`L4-5.2-Candidate-Transparency-Notice-v1.md` is strong on candidate-facing transparency. `L3-4.3-Incident-Response-Plan-v1.md` covers incident notification. A minor gap exists around system-level documentation for operator/client users.

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-20 | AI System User Documentation | Per-system documentation for operator/client users explaining: what the system does, its intended use, known limitations, how to interpret outputs, and how to raise concerns. Distinct from the candidate-facing notice. |

---

### A.9 — Use of AI Systems by the Organisation

**Status: Partial**  
Intended use is referenced in the Governance Policy. No standalone procedure governs how employees and operators must use AI systems responsibly in practice.

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-21 | Responsible AI Use Procedure | A procedure defining: permitted and prohibited uses of each AI system, human oversight requirements, when an AI decision must be reviewed by a human before acting, and how misuse is reported. |

---

### A.10 — Third-Party and Customer Relationships

**Status: Mostly covered**  
`L4-5.1-Data-Processing-Agreement-Template-v1.md` covers data processor obligations well. A gap exists in AI-specific supplier governance — ensuring third-party AI components (e.g. foundation models, scoring APIs) meet the organisation's AI governance standards.

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-22 | AI Supplier Governance Clause / Checklist | A standard clause or checklist used when procuring or renewing third-party AI components, covering: supplier AI governance practices, bias testing obligations, incident notification requirements, and the right to audit. |

---

## Part 3: Cross-Cutting Gap

| # | Missing Artefact | Description |
|---|-----------------|-------------|
| G-23 | ISO 42001 Clause-to-Document Mapping Table | A master cross-reference table showing which existing framework document satisfies which ISO 42001 clause or Annex A control. Essential for audit evidence presentation. Without this, auditors cannot efficiently verify conformance even where controls exist. |

---

## Consolidated Action List

| ID | Artefact to Create | ISO 42001 Mapping | Priority |
|----|-------------------|-------------------|----------|
| G-01 | AIMS Scope Statement | Cl.4 | High |
| G-02 | Interested Parties Register | Cl.4 | High |
| G-03 | Internal & External Issues Analysis | Cl.4 | Medium |
| G-04 | Top Management Commitment Statement | Cl.5 | High |
| G-05 | AI System Impact Assessment (ASIA) Template | Cl.6, A.5 | High |
| G-06 | Risk Treatment Plan | Cl.6 | High |
| G-07 | Competence & Training Framework | Cl.7 | Medium |
| G-08 | Staff Awareness Programme | Cl.7 | Medium |
| G-09 | Documented Information Control Procedure | Cl.7 | Medium |
| G-10 | AI Lifecycle Controls Document | Cl.8, A.6 | High |
| G-11 | Periodic Re-assessment Procedure | Cl.8 | Medium |
| G-12 | Internal Audit Procedure | Cl.9 | High |
| G-13 | Internal Audit Schedule / Programme | Cl.9 | Medium |
| G-14 | Management Review Procedure | Cl.9 | High |
| G-15 | Nonconformity & Corrective Action Procedure | Cl.10 | High |
| G-16 | Continual Improvement Log | Cl.10 | Low |
| G-17 | AI Concern / Escalation Pathway | A.3 | Medium |
| G-18 | AI Resource Register | A.4 | Low |
| G-19 | Data Quality & Provenance Procedure | A.7 | High |
| G-20 | AI System User Documentation | A.8 | Medium |
| G-21 | Responsible AI Use Procedure | A.9 | Medium |
| G-22 | AI Supplier Governance Clause / Checklist | A.10 | Medium |
| G-23 | ISO 42001 Clause-to-Document Mapping Table | All | High |

**High priority gaps: 10**  
**Medium priority gaps: 9**  
**Low priority gaps: 2**  
**Total gaps: 23**

---

## What Already Exists (Strengths to Build On)

| Existing Document | ISO 42001 Coverage |
|-------------------|--------------------|
| `L1-2.4-Governance-Policy-v1.md` | Cl.5, A.2 |
| `L1-2.5-Roles-and-Responsibilities-v1.md` | Cl.5, A.3 |
| `L1-2.2-Risk-Classification-Framework-v1.md` | Cl.6 (partial) |
| `L1-2.1-AI-System-Inventory-v1.md` | Cl.4 (partial), A.4 (partial) |
| `L1-2.3-Data-Flow-Map-v1.md` | Cl.8 (partial), A.7 (partial) |
| `L2-3.4-DPIA-Template-v1.md` | A.5 (partial — GDPR DPIA ≠ ASIA) |
| `L3-4.1-Monitoring-Framework-v1.md` | Cl.9 (partial) |
| `L3-4.2-Bias-Monitoring-Protocol-v1.md` | Cl.9, A.6 (partial) |
| `L3-4.3-Incident-Response-Plan-v1.md` | Cl.10 (partial) |
| `L4-5.1-Data-Processing-Agreement-Template-v1.md` | A.10 (partial) |
| `L4-5.2-Candidate-Transparency-Notice-v1.md` | A.8 |
| `STAGE1-Regulatory-Orientation-Note-v1.md` | Cl.4 (partial) |

---

## Notes

- This gap analysis is based on the framework as of May 2026 and the ISO/IEC 42001:2023 standard structure.
- Priorities are set relative to audit risk: High items are typically first-line evidence requests during an ISO 42001 certification audit.
- Some gaps can be resolved by adapting existing documents (e.g. adding an AIMS scope section to the Governance Policy) rather than creating entirely new files.
- The DPIA Template and the ASIA Template should remain separate instruments — the DPIA satisfies UK GDPR Article 35, while the ASIA satisfies ISO 42001 Clause 6 and A.5. They have different scopes, stakeholders, and triggers.
