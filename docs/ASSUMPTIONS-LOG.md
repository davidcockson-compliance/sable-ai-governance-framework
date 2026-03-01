# Assumptions Log

**Project:** Sable AI Ltd — AI Governance Framework
**Version:** v1
**Date:** 2026-02-28

---

## About This Document

This framework is built using a fictional company — Sable AI Ltd — as the demonstration vehicle. Because no real company data is available, every claim about Sable AI Ltd's systems, operations, or characteristics is an assumption, not a verified fact.

This log records all assumptions in one place. Every document in the framework also flags its own assumptions inline using the `[ASSUMPTION]` marker.

**If you are adapting this framework for a real organisation:** this log is your starting point. Work through every assumption and validate it against your actual company data before treating any framework document as complete.

---

## Status Key

| Status | Meaning |
|---|---|
| 🔴 Unverified | Not confirmed against real data — placeholder only |
| 🟡 Partial | Partially confirmed — needs full verification |
| 🟢 Confirmed | Verified against real company data |

---

## Core Company Assumptions

| # | Assumption | Source | Status | Date confirmed |
|---|---|---|---|---|
| A-001 | Sable AI Ltd is a UK-incorporated early-stage company (10–15 person team, post-seed, pre-Series A) | CLAUDE.md §2 | 🔴 Unverified | — |
| A-002 | Scout is an AI-powered CV screening and candidate shortlisting tool using the Anthropic Claude API | CLAUDE.md §2 | 🔴 Unverified | — |
| A-003 | Customers are UK recruitment agencies (B2B) and in-house HR teams at UK corporates | CLAUDE.md §2 | 🔴 Unverified | — |
| A-004 | Hosting is on AWS UK region (eu-west-2) | CLAUDE.md §2 | 🔴 Unverified | — |
| A-005 | Anthropic is a sub-processor; candidate data is not used for model training; a valid DPA with Anthropic is in place | CLAUDE.md §2 | 🔴 Unverified | — |
| A-006 | Sable AI Ltd operates under UK GDPR, DPA 2018, Data (Use and Access) Act 2025, and Equality Act 2010 | CLAUDE.md §2 | 🔴 Unverified | — |
| A-007 | Scout outputs are subject to mandatory human review before any candidate contact is made | CLAUDE.md §2 | 🔴 Unverified | — |

---

## Document-Level Assumptions

*Populated by Assumptions Tracker (Agent 4) during each run. Entries added here as each document is drafted.*

| # | Assumption | Source document | Status | Date confirmed |
|---|---|---|---|---|
| A-008 | Sable AI Ltd's primary role in the candidate data processing chain is as a data processor acting on behalf of its customers, not as an independent controller of candidate personal data | STAGE1-Regulatory-Orientation-Note-v1.md | 🔴 Unverified | — |
| A-009 | Legitimate interests under Art. 6(1)(f) UK GDPR is the most likely lawful basis for Scout's core CV screening and shortlisting activity | STAGE1-Regulatory-Orientation-Note-v1.md | 🔴 Unverified | — |
| A-010 | Scout is deployed as a SaaS web application accessed by customer users via browser | L1-2.1-AI-System-Inventory-v1.md | 🔴 Unverified | — |
| A-011 | Only extracted CV text and job description text are transmitted to the Anthropic Claude API — no raw document files or additional personal data fields beyond CV content | L1-2.1-AI-System-Inventory-v1.md | 🔴 Unverified | — |
| A-012 | The human review step in Scout's workflow is designed such that reviewers exercise genuine independent judgment and can override Scout's outputs — i.e., the review is not a token gesture | L1-2.2-Risk-Classification-Framework-v1.md | 🔴 Unverified | — |
| A-013 | Retention periods for candidate personal data processed through Scout have not been formally defined by Sable AI Ltd | L1-2.3-Data-Flow-Map-v1.md | 🔴 Unverified | — |
| A-014 | Anthropic processes Scout API requests outside the UK (likely USA) — UK GDPR Chapter V international transfer obligations apply | L1-2.3-Data-Flow-Map-v1.md | 🔴 Unverified | — |
| A-015 | Sable AI Ltd does not currently employ a dedicated Data Protection Officer; the CTO carries DPO-equivalent responsibilities at this stage | L1-2.4-Governance-Policy-v1.md | 🔴 Unverified | — |
| A-016 | Art. 6(1)(f) legitimate interests is the primary lawful basis for Scout's CV processing; a Legitimate Interests Assessment has not yet been conducted or documented | L2-3.1-UK-GDPR-Mapping-Matrix-v1.md | 🔴 Unverified | — |
| A-017 | Scout's screening criteria and ranking logic do not currently incorporate any explicit mechanism to detect, flag or suppress protected characteristic proxies (e.g., name-based ethnicity inference, employment gap penalisation) | L2-3.1-UK-GDPR-Mapping-Matrix-v1.md | 🔴 Unverified | — |
| A-018 | Scout does not currently provide an alternative manual assessment pathway for candidates with disabilities who require a reasonable adjustment | L2-3.2-Equality-Act-2010-Compliance-Map-v1.md | 🔴 Unverified | — |
| A-019 | Recruiter customers are not currently required by contract to provide candidates with a transparency notice disclosing Scout's use prior to CV processing | L2-3.2-Equality-Act-2010-Compliance-Map-v1.md | 🔴 Unverified | — |
| A-020 | Scout's application produces structured audit log events with sufficient timestamp granularity to automate human review compliance monitoring (M-02) and review completion time tracking (M-05) | L3-4.1-Monitoring-Framework-v1.md | 🔴 Unverified | — |
| A-021 | Monitoring evidence (KPI reports, test results, audit logs, bias monitoring records) is retained for a minimum of 3 years — retention period not yet formally defined in a data retention policy | L3-4.1-Monitoring-Framework-v1.md | 🔴 Unverified | — |
| A-022 | Sable AI Ltd does not currently collect voluntary equality monitoring demographic data from candidates; demographic data for bias monitoring (Option A per L3-4.2) is not currently available | L3-4.2-Bias-Monitoring-Protocol-v1.md | 🔴 Unverified | — |
| A-023 | Sable AI Ltd has not yet commissioned an external bias audit of Scout; no external audit reports currently exist | L3-4.2-Bias-Monitoring-Protocol-v1.md | 🔴 Unverified | — |

---

## Phase 2 Assumptions

*To be populated when Phase 2 Scout worked examples are drafted.*

| # | Assumption | Source document | Status | Date confirmed |
|---|---|---|---|---|
| — | — | — | — | — |

---

## How to Resolve Assumptions

For each assumption, the validation process is:

1. Identify the relevant data source (CTO, legal counsel, engineering documentation, etc.)
2. Obtain written confirmation of the actual position
3. Update the relevant framework document to replace assumed values with confirmed values
4. Remove the `[ASSUMPTION]` flag from that field
5. Update this log with status 🟢 Confirmed and the date confirmed

Some assumptions (particularly the Art. 22 threshold analysis and joint controller question) require a qualified UK lawyer, not just internal verification.

---

## Resolved Assumptions

*None — this framework was built entirely on assumed company characteristics. All assumptions remain unverified pending adaptation for a real organisation.*

---

| A-020 | The Art. 22A–22D ADM threshold for Scout's shortlisting outputs has not been legally assessed; the DPIA assumes that mandatory human review prevents solely automated decisions, but this conclusion has not been confirmed by a qualified UK lawyer | L2-3.4-DPIA-Template-v1.md | 🔴 Unverified | — |
| A-021 | No technical controls currently exist within Scout to detect, flag, or prevent inadvertent processing of special category data contained in free-text CV sections | L2-3.4-DPIA-Template-v1.md | 🔴 Unverified | — |
| A-022 | Retention periods for CV documents, AI-generated shortlisting outputs, and system logs have not been established or documented | L2-3.4-DPIA-Template-v1.md | 🔴 Unverified | — |
| A-023 | Scout's user interface does not currently include any feature allowing recruiters to filter or search candidate shortlists by protected characteristic | L2-3.3-ICO-Audit-Gap-Analysis-v1.md | 🔴 Unverified | — |
| A-024 | P1/P2 incident severity thresholds based on number of affected candidate records (50+ for P1; 1–49 for P2) are assumed and not yet confirmed against Sable AI Ltd's DPIA risk assessment | L3-4.3-Incident-Response-Plan-v1.md | 🔴 Unverified | — |
| A-025 | A five-business-day acknowledgement SLA for candidate complaints is assumed; this has not been confirmed against Sable AI Ltd's customer service or legal obligations | L3-4.3-Incident-Response-Plan-v1.md | 🔴 Unverified | — |
| A-026 | A minimum three-year retention period for the incident and near-miss register is assumed as recommended practice; this has not been confirmed with a legal adviser against Sable AI Ltd's specific regulatory position | L3-4.3-Incident-Response-Plan-v1.md | 🔴 Unverified | — |

| A-027 | The applicable UK international transfer mechanism for Sable AI Ltd's processing of Candidate Data via Anthropic, Inc. (US) has not been confirmed — it may be the UK-US Data Bridge (where Anthropic is certified under the UK Extension to the EU-US Data Privacy Framework), a UK International Data Transfer Agreement, or a UK Addendum to the EU Standard Contractual Clauses | L4-5.1-Data-Processing-Agreement-Template-v1.md | 🔴 Unverified | — |
| A-028 | The controller/processor characterisation for each individual recruitment agency customer — whether the standard Art. 28 terms or the Art. 26 joint controller addendum applies — has not been determined; this requires legal analysis of each specific customer relationship and cannot be resolved by template | L4-5.1-Data-Processing-Agreement-Template-v1.md | 🔴 Unverified | — |
| A-029 | Sable AI Ltd has not confirmed that operational processes exist to handle candidate Art. 22C rights requests (human review, representations, and contest of AI-assisted assessments) within the timeframes stated in L4-5.2-Candidate-Transparency-Notice-v1.md | L4-5.2-Candidate-Transparency-Notice-v1.md | 🔴 Unverified | — |
| A-030 | Scout's technical stack (Python/FastAPI backend, pdfplumber + python-docx for CV parsing, React TypeScript frontend, PostgreSQL on AWS RDS, S3 file storage, AWS API Gateway + WAF) is assumed from the Phase 2 input brief and has not been confirmed against Sable AI Ltd's actual codebase. Additionally, the absence of a PII-stripping pre-processing step before Anthropic API calls is assumed as the current implementation state — this has not been technically verified. | P2-Scout-System-Profile-v1.md | 🔴 Unverified | — |

| A-031 | Scout does not currently collect demographic data from candidates in any form — no voluntary equality monitoring questionnaire or mechanism exists in the current product; all Phase A monitoring relies on non-demographic aggregate analysis | P2-Scout-Bias-Monitoring-Protocol-v1.md, P2-Scout-ICO-Audit-Gap-Analysis-v1.md | 🔴 Unverified | — |
| A-032 | No completed DPIA has been filed for Scout's production deployment; only the template (L2-3.4-DPIA-Template-v1.md) exists. Completion of the DPIA against real operational data is assumed not to have occurred before live production processing [see ACT-001 in P2-Scout-ICO-Audit-Gap-Analysis-v1.md] | P2-Scout-ICO-Audit-Gap-Analysis-v1.md | 🔴 Unverified | — |
| A-033 | No Data Processing Agreement has been executed with any live customer as of the Phase 2 assessment date; the DPA templates (L4-5.1 Appendix A and Appendix B) exist and are ready for execution but no confirmed customer signing has occurred | P2-Scout-ICO-Audit-Gap-Analysis-v1.md | 🔴 Unverified | — |

*Last updated: 2026-03-01. Maintained by Assumptions Tracker (Agent 4) throughout every run.*
