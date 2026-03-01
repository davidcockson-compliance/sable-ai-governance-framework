# Scout AI System Profile — Worked Example
**Project:** Sable AI Ltd — AI Governance Framework
**Stage:** Phase 2 — Worked Examples
**Status:** Draft
**Version:** v1
**Date:** 2026-03-01
**Assumptions:** Built on outline assumptions — not verified against real Sable AI Ltd data

---

## How to Use This Document

This is the **worked example version** of `L1-2.1-AI-System-Inventory-v1.md`. Where the template provides field headings and instructions, this document provides the specific technical content for Scout as assumed for this framework.

Every field that relies on an unverified assumption is marked `[ASSUMPTION: A-XXX]`. Before using this profile operationally, all assumptions must be validated against real Sable AI Ltd data and reviewed by qualified legal counsel.

**Cross-references:** `L1-2.1-AI-System-Inventory-v1.md` (inventory template) | `L1-2.3-Data-Flow-Map-v1.md` (data flow template) | `P2-Scout-UK-GDPR-Mapping-v1.md` (GDPR obligations worked example)

---

## 1. System Overview

| Field | Value |
|---|---|
| **System name** | Scout |
| **System owner** | Sable AI Ltd |
| **Version** | Production (current as at 2026-03-01) |
| **Purpose** | AI-assisted CV screening and candidate shortlisting. Analyses candidate CVs against job descriptions and produces a ranked shortlist with match scores, matched/unmet criteria, human-review flags, and a one-paragraph candidate summary. |
| **AI type** | Prompt-engineered AI skill — not a custom ML model. Scout is a structured wrapper around the Anthropic Claude API. No proprietary model training, no fine-tuning, no model weights owned by Sable AI Ltd. |
| **Underlying model** | `claude-sonnet-4-6` via Anthropic Claude API (commercial API access) [ASSUMPTION: A-002] |
| **Model ownership** | Anthropic, PBC (San Francisco, CA, USA). Sable AI Ltd owns the system prompt, workflow orchestration, and user interface — not the underlying AI model. |
| **Hosting** | AWS eu-west-2 (London region) throughout [ASSUMPTION: A-004]. All compute, storage, and database resources are UK-region. |
| **Sub-processors** | Anthropic, PBC — processes extracted CV text and job descriptions on every API call [ASSUMPTION: A-005]. AWS — compute (EC2/Fargate), storage (S3), database (RDS), gateway (API Gateway + WAF). |
| **Data subjects** | Job candidates whose CVs are uploaded to Scout for screening |
| **Customer segments** | (1) UK recruitment agencies — screening candidates on behalf of employer clients [ASSUMPTION: A-003]; (2) UK in-house HR teams — direct hire screening [ASSUMPTION: A-003] |
| **Intended user** | Recruiter or HR professional at a customer organisation. Scout is a B2B tool; candidates do not interact with Scout directly. |
| **Human review requirement** | Mandatory. No candidate is contacted before a recruiter has reviewed and approved the AI's assessment [ASSUMPTION: A-007]. Human review is built into the Scout workflow and enforced at the UI level. |
| **Risk classification** | High — see `L1-2.2-Risk-Classification-Framework-v1.md` for full classification rationale |
| **DPIA required** | Yes — ICO confirms AI in recruitment triggers Art. 35 DPIA. Template at `L2-3.4-DPIA-Template-v1.md`. Deployment-specific DPIA not yet completed [ASSUMPTION: A-021]. |
| **Last review date** | 2026-03-01 (initial version — framework document) |
| **Next review due** | 2026-09-01 [ASSUMPTION: A-023 — review cycle not yet confirmed] |

---

## 2. Architecture Note

Scout is **not a custom AI model**. This distinction has significant compliance implications.

Scout operates as a **prompt-engineered skill** layered on top of the Anthropic Claude API — analogous to a structured use of Claude for a specific domain task. The AI capability (language understanding, reasoning, structured output) is provided entirely by Anthropic's model infrastructure. Sable AI Ltd provides:

- The system prompt (defines Scout's role, scoring rubric, output schema, and operating constraints — a Sable AI Ltd trade secret)
- Backend orchestration (document parsing, API call construction, output storage)
- Recruiter-facing UI
- Workflow logic (upload → screen → review → decision)

**Compliance implications of this architecture:**

| Implication | Detail |
|---|---|
| Candidate data reaches Anthropic on every API call | Extracted CV text (including PII) is sent to Anthropic's API as the user message payload. There is currently no pre-processing step to strip PII before transmission. This is a compliance gap against UK GDPR Art. 5(1)(c) (data minimisation). |
| No control over model internals | Sable AI Ltd cannot audit, inspect, or modify Anthropic's model weights or training data. Prompt engineering is the sole mechanism for bias control and output structuring at Sable AI Ltd's disposal. |
| No model training on candidate data | Anthropic processes candidate data in-context for API responses only. Candidate data is not used to train or fine-tune any model [ASSUMPTION: A-005]. A Data Processing Agreement with Anthropic is assumed to be in place [ASSUMPTION: A-005]. |
| International transfer | Anthropic is a US-incorporated company. API calls may traverse to US infrastructure. The applicable UK international transfer mechanism has not been confirmed [ASSUMPTION: A-027]. |

---

## 3. Technical Stack

| Layer | Component | Detail |
|---|---|---|
| **AI core** | Anthropic Claude API | Model: `claude-sonnet-4-6`. System prompt + structured user message per screening request. No fine-tuning. [ASSUMPTION: A-002] |
| **Backend** | Python / FastAPI | API server handling document parsing, Anthropic API orchestration, output storage, and recruiter-facing API endpoints [ASSUMPTION: A-030] |
| **Document parsing** | `pdfplumber` (PDF) + `python-docx` (DOCX) | Extracts plain text from candidate CV files server-side before sending to Claude. Raw CV files are not forwarded to Anthropic — only extracted text is sent. [ASSUMPTION: A-030] |
| **Frontend** | React (TypeScript) | Recruiter web UI: upload job descriptions and CVs; view ranked shortlist; read AI summaries; log review decisions; view raw AI output JSON [ASSUMPTION: A-030] |
| **Database** | PostgreSQL on AWS RDS | Stores job postings, candidate records, AI output JSON, recruiter decisions, and audit logs. All data in eu-west-2. [ASSUMPTION: A-004] [ASSUMPTION: A-030] |
| **File storage** | AWS S3 | Stores raw CV documents (PDF/DOCX). Encrypted at rest (AES-256). Access restricted to backend service role. [ASSUMPTION: A-004] [ASSUMPTION: A-030] |
| **Hosting** | AWS eu-west-2 throughout | All compute, storage, and database resources in UK region [ASSUMPTION: A-004] |
| **API gateway** | AWS API Gateway + WAF | Controls external access to backend; rate limiting; request logging [ASSUMPTION: A-030] |

---

## 4. End-to-End Screening Workflow

The following describes Scout's processing flow for a single screening run. Each step is numbered for reference against the data flow map in Section 6.

| Step | Actor | Action | Data involved |
|---|---|---|---|
| 1 | Recruiter | Creates a new role in Scout UI. Enters or pastes job description (role title, required skills, experience level, responsibilities, nice-to-haves). | Job description text |
| 2 | Recruiter | Uploads one or more CV files (PDF or DOCX). Batch uploads supported. | Raw CV files (PDF/DOCX) |
| 3 | Scout backend | Extracts plain text from each CV using `pdfplumber` / `python-docx`. Extraction performed server-side. Raw files stored in S3; extracted text held transiently in memory. | Extracted CV text (including PII) |
| 4 | Scout backend | Constructs an Anthropic API call for each CV: **System message** — Scout's operating constraints and output schema. **User message** — job description text + extracted CV text. | System prompt; extracted CV text; job description text → all sent to Anthropic |
| 5 | Anthropic Claude API | Processes the input and returns a structured JSON response per the schema in Section 5. | AI output JSON returned to Scout backend |
| 6 | Scout backend | Stores JSON output in PostgreSQL against the candidate record, with timestamp, model version, and an input hash (for audit). Extracted CV text is not stored in raw form post-call. | AI output JSON; audit metadata |
| 7 | Scout UI | Displays ranked shortlist to recruiter. For each candidate: match score, matched/unmet criteria, flags for human review, one-paragraph summary. Full raw AI output JSON is also accessible. | AI output JSON; job data |
| 8 | Recruiter | Reviews each candidate. Selects to progress, reject, or hold. Logs decision in UI. **No candidate is contacted before this step is completed.** [ASSUMPTION: A-007] | Recruiter decision; assessor notes |
| 9 | Scout backend | Logs all actions (upload, API call, recruiter decision) to audit log with timestamp and user ID. | Audit log entries |

---

## 5. AI Output Schema

Each Anthropic API call returns a structured JSON object. The schema below is the canonical output format. The system prompt instructs Claude to produce only this format; non-conforming outputs are rejected by the backend.

```json
{
  "candidate_id": "[internal UUID]",
  "job_id": "[internal UUID]",
  "model_version": "claude-sonnet-4-6",
  "processed_at": "[ISO 8601 timestamp]",
  "match_score": 74,
  "score_confidence": "high",
  "matched_criteria": [
    "5+ years Python experience",
    "Experience with REST API design",
    "Prior work in regulated industry"
  ],
  "unmet_criteria": [
    "No evidence of team leadership experience"
  ],
  "flags_for_human_review": [
    "CV contains a gap in employment history 2021–2022 — recruiter to explore at interview"
  ],
  "summary": "Strong technical match for the role. Python and API experience well-evidenced. No leadership experience visible in CV — this may warrant a conversation. Recommend progressing to phone screen.",
  "assessor_notes": null
}
```

**Field notes:**

| Field | Compliance relevance |
|---|---|
| `match_score` | Numerical output of AI assessment — subject to human review before any candidate action. Not used as a sole decision mechanism. |
| `score_confidence` | Low-confidence outputs trigger additional human review flags. |
| `flags_for_human_review` | Surfaced explicitly to recruiter for all aspects Claude cannot confidently assess. Supports Art. 22C right to human review of AI-assisted assessments. |
| `assessor_notes` | Populated by recruiter post-review. Creates an auditable record of the human decision separate from the AI output. |
| `model_version` | Records which Claude model version produced this output. Important for reproducibility and audit. |

---

## 6. Data Flows — What Reaches Each System

The table below maps each data element to the systems that process or store it. This is the specific Scout realisation of the flow described in `L1-2.3-Data-Flow-Map-v1.md`.

| Data element | Scout backend (AWS) | Anthropic Claude API | AWS S3 | AWS RDS PostgreSQL |
|---|---|---|---|---|
| Raw CV file (PDF/DOCX) | ✓ received, parsed | ✗ | ✓ stored encrypted (AES-256) | ✗ |
| Extracted CV text | ✓ transiently (in-memory during processing) | ✓ as part of user message | ✗ | ✗ not stored in raw form |
| Candidate PII (name, email, phone, address) | ✓ extracted fields stored | ✓ embedded in CV text — full PII reaches Anthropic | ✗ | ✓ extracted fields stored |
| Job description text | ✓ | ✓ as part of user message | ✗ | ✓ stored |
| System prompt | ✓ | ✓ as system field | ✗ | ✗ |
| AI output JSON | ✓ | ✓ returned | ✗ | ✓ stored |
| Recruiter decision + assessor notes | ✓ | ✗ | ✗ | ✓ stored |
| Audit log entries | ✓ | ✗ | ✗ | ✓ stored |

**Key compliance gap — data minimisation:** Candidate PII (name, email, phone, address) is embedded in CV text and reaches Anthropic as part of every API call. There is currently **no pre-processing step to strip PII before sending to Anthropic**. This is a compliance gap against UK GDPR Art. 5(1)(c) (data minimisation principle) and is recorded in the action register at `P2-Scout-UK-GDPR-Mapping-v1.md`. [ASSUMPTION: A-030 — the absence of a PII-stripping step is assumed as the current implementation state]

---

## 7. Retention Schedule (Proposed — Not Legally Confirmed)

[ASSUMPTION: A-022] The following retention periods are proposed for Scout's data lifecycle. They have not been confirmed through a formal retention review or legal sign-off.

| Data | System | Proposed retention period | Deletion trigger | Status |
|---|---|---|---|---|
| Raw CV files | AWS S3 | 6 months after role filled or application closed | Automated deletion job | Not implemented |
| Extracted CV text | Anthropic API (in-context only) | Not retained post-call | Anthropic processes in-context only [ASSUMPTION: A-005] | Assumed — not verified |
| AI output JSON | AWS RDS | 6 months after role filled or application closed | Automated deletion job | Not implemented |
| Recruiter decisions + notes | AWS RDS | 6 months after role filled or application closed | Automated deletion job | Not implemented |
| Audit logs | AWS RDS | 2 years | Regulatory retention for accountability | Not implemented |
| Job description records | AWS RDS | Duration of customer contract + 1 year | Contract management | Not implemented |

**Action required:** Retention periods must be legally confirmed, implemented as automated deletion processes, and communicated to data subjects via the candidate transparency notice (`L4-5.2-Candidate-Transparency-Notice-v1.md`).

---

## 8. System Prompt — Compliance-Relevant Constraints

The full Scout system prompt is a Sable AI Ltd trade secret. The constraints relevant to compliance are described below [ASSUMPTION: A-030].

**Instructions given to Claude:**
- Score candidates against stated job requirements only
- Produce output strictly in the JSON schema defined in Section 5
- Flag employment gaps, unexplained career changes, and any aspect of the CV it cannot confidently assess — all such items appear in `flags_for_human_review`
- Recommend human review where `score_confidence` is `"low"`

**Explicit prohibitions:**
- Do not consider, reference, or infer from any information that could be a proxy for a protected characteristic under the Equality Act 2010 (age, gender, nationality, religion, disability, pregnancy, race, sexual orientation, marriage/civil partnership)
- Do not use candidate name, address, or personal contact details as scoring inputs
- Do not produce output outside the defined JSON schema

**Known limitations of prompt-level bias control:**

| Limitation | Implication |
|---|---|
| Implicit associations in model training | Claude cannot guarantee that implicit associations in its training data do not influence scoring, even when explicitly instructed not to use protected characteristics. |
| Proxy correlation | The instruction not to use protected characteristics does not prevent their influence where they correlate with other CV features (e.g., institution names, career timelines, naming conventions, geographic indicators). |
| No demographic monitoring | No automated demographic monitoring is in place to detect disparate impact in shortlisting outputs. |
| No independent audit | Prompt-level bias controls have not been independently audited. |

These limitations are addressed further in `P2-Scout-Bias-Monitoring-Protocol-v1.md`.

---

## 9. Adaptation Notes for Real Organisations

This worked example is built on assumed Sable AI Ltd data. Before using it as a real system profile, the following must be verified or replaced:

| Item | Action required |
|---|---|
| Anthropic model version | Confirm current production model. Model versions change; profiles should be updated on model version changes. |
| Technical stack | Confirm actual technology choices (Python/FastAPI, pdfplumber, PostgreSQL, React). |
| AWS region | Confirm all resources are genuinely in eu-west-2 (or update if different). |
| Anthropic DPA status | Confirm a Data Processing Agreement with Anthropic is executed and covers UK GDPR obligations. Confirm the international transfer mechanism. |
| Human review enforcement | Confirm how the "no candidate contact before human review" obligation is technically enforced — not just a policy commitment. |
| Retention periods | Confirm legally reviewed retention periods and implement automated deletion. |
| System prompt text | Review full system prompt for compliance — the summary in Section 8 is not a substitute for a formal prompt review. |

[LEGAL REVIEW REQUIRED] This profile should be reviewed by a qualified UK data protection lawyer before being used in an operational context.

---

*This document is a worked example for educational purposes. It is not a legal compliance certification. All assumptions must be verified against real Sable AI Ltd data before operational use.*
