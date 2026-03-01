# Scout — Bias Monitoring Protocol (Worked Example)
**Project:** Sable AI Ltd — AI Governance Framework
**Stage:** Phase 2 — Worked Examples
**Status:** Draft
**Version:** v1
**Date:** 2026-03-01
**Assumptions:** Built on outline assumptions — not verified against real Sable AI Ltd data

---

## Purpose

This document is the Phase 2 worked example of `L3-4.2-Bias-Monitoring-Protocol-v1.md`. Where L3-4.2 defines the methodology and obligations for bias monitoring in abstract framework terms, this document specifies how those methods are implemented using Scout's actual technical architecture: Python/FastAPI backend, PostgreSQL on AWS RDS, Anthropic Claude API (`claude-sonnet-4-6`), and React frontend with recruiter decision logging.

[LEGAL REVIEW REQUIRED] — The legal constraints on demographic data collection and processing described in Section 3 of `L3-4.2-Bias-Monitoring-Protocol-v1.md` apply in full to this document. No demographic monitoring capability described in Phase B of this document may be implemented without legal confirmation that an Art. 6 lawful basis and Art. 9(2) condition are in place. This document does not reproduce those constraints in full — read `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 3 alongside this document.

---

## 1. Scout Architecture and Bias Monitoring Context

### 1.1 The prompt-engineering constraint

Scout is not a custom-trained ML model. It is a prompt-engineered AI skill built on the Anthropic Claude API [ASSUMPTION A-002]. This architectural fact has significant implications for bias monitoring that do not apply to providers operating custom or fine-tuned models:

| Traditional ML model | Scout (prompt-engineered skill) |
|---|---|
| Model weights can be inspected for feature importance | No model weights are accessible to Sable AI Ltd — only output |
| Training data can be audited for representational bias | No training data is under Sable AI Ltd's control — Claude's training is Anthropic's domain |
| Feature weighting can be adjusted by modifying the model | Scoring emphasis is adjusted through system prompt instructions only |
| "Model rollback" means reverting to a previous model version | "Model rollback" means reverting to a previous system prompt version or previous Claude API model version |
| Bias audit can inspect model internals | Bias audit must rely entirely on output-side statistical analysis and prompt review |

This means that Scout's bias monitoring programme is 100% output-based and prompt-based. Statistical monitoring approaches are the same as for any AI system. The difference is that remediation and causal investigation are constrained to what can be observed in outputs and what can be controlled through prompt engineering.

### 1.2 Scout's available monitoring infrastructure

The following data stores and systems are available for bias monitoring with Scout's existing technical architecture [ASSUMPTION A-004]:

| Data source | Contents | Monitoring use |
|---|---|---|
| **PostgreSQL RDS — screening outputs** | `candidate_id`, `job_id`, `model_version`, `processed_at`, `match_score`, `score_confidence`, `matched_criteria`, `unmet_criteria`, `flags_for_human_review` | Primary source for output-side statistical analysis |
| **PostgreSQL RDS — recruiter decisions** | Decision (shortlisted / rejected / hold), timestamp, user_id, `assessor_notes` | Override rate monitoring; recruiter challenge rate |
| **PostgreSQL RDS — audit logs** | Every screening action: upload, API call, recruiter decision, with timestamp and user_id | Completeness and process compliance monitoring |
| **PostgreSQL RDS — job records** | Job title, job description, customer_id, sector/industry | Job-category-based shortlisting analysis |
| **AWS S3 — CV files** | Raw PDF/DOCX files (encrypted, access-restricted) | Available for manual review of sampled rejected CVs; not for automated processing |
| **Scout system prompt** | Current version of the prompt defining Scout's role, scoring rubric, constraints | Subject to keyword and proxy-feature audit |

**What is not available without additional implementation:**
- Candidate demographic data — Scout does not collect this in any form [ASSUMPTION]
- Protected characteristic group membership for any candidate — cannot be inferred or assumed
- Model internals, attention weights, or feature importance from the Anthropic Claude API
- Anthropic's training data or model architecture

### 1.3 Current monitoring baseline

**As of the date of this document, Scout's bias monitoring state is as follows** (from Phase 2 input brief, Section 8):

| Monitoring type | Current status |
|---|---|
| Prompt-level constraints (instructs Claude to exclude protected characteristic proxies) | **In place** — but not independently audited |
| Automated demographic monitoring | **None** |
| Adverse impact ratio calculations | **None** |
| Keyword or language bias audit | **None performed** |
| Recruiter override rate monitoring | **Available data exists; not currently analysed** |
| External bias audit | **None conducted** |

This is the baseline from which the implementation roadmap in Section 9 of this document starts.

---

## 2. Fundamental Monitoring Constraint: No Demographic Data

The most significant constraint on Scout's bias monitoring programme is the absence of candidate demographic data and the legal complexity of collecting it. This is addressed in detail in `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 3 and is summarised here for operational context.

**Scout cannot currently perform:**
- Individual-level demographic parity testing
- Adverse impact ratio calculation by protected characteristic group
- Four-fifths rule analysis per ICO methodology

**Why:** Scout does not collect demographic data from candidates. Inferring demographic data (e.g., ethnicity from name) is unreliable and in most cases unlawful under UK GDPR Art. 9 per the ICO's November 2024 audit findings. No Art. 9(2) condition has been confirmed for voluntary monitoring purposes [LEGAL REVIEW REQUIRED].

**This means Scout's bias monitoring programme is necessarily divided into two phases:**

**Phase A — Available immediately, no new data collection:**
Aggregate statistical analysis of shortlisting outputs using existing data. Cannot detect individual-level protected characteristic bias, but can detect systemic anomalies and proxy feature patterns.

**Phase B — Available after lawful basis confirmed:**
Full demographic parity testing using voluntary equality monitoring data collected from candidates with explicit consent. Requires legal sign-off on Art. 6(1)(a) + Art. 9(2)(a) route and frontend/data model changes to collect and store monitoring data.

The implementation roadmap in Section 9 reflects this two-phase structure.

---

## 3. Phase A Monitoring: Immediately Available Controls

### 3.1 Recruiter override rate monitoring

**What it measures:** The proportion of Scout's shortlisting recommendations that recruiters override. A high override rate indicates that recruiter judgement consistently diverges from Scout's scoring — this is a signal that Scout's outputs may be unreliable, biased, or poorly calibrated to the specific role or customer context.

**Why it matters for bias:** Override rate by itself does not confirm bias; however, patterns in override behaviour — e.g., consistently overriding low scores for candidates in a particular job category or role type — can surface proxy relationships worth investigating through keyword audit or manual CV sampling (Section 3.3).

**Implementation using existing Scout data:**

Scout's recruiter decision log in PostgreSQL records each recruiter decision against each AI screening output. The override rate can be derived by comparing the recruiter's decision (shortlist / reject / hold) against Scout's `match_score` rank position.

Define "override" as: a recruiter selecting a candidate for progression who scored in the bottom quartile of Scout's output for that role, OR rejecting a candidate who scored in the top quartile. [ASSUMPTION — exact definition should be confirmed with Product and established in the monitoring framework before first calculation.]

The monitoring query logic requires:
- Selecting recruiter decisions grouped by job_id
- Matching recruiter decision to Scout's ranked output position
- Calculating the percentage of decisions that diverge from rank-order expectation
- Aggregating by job_category, customer_id, and time period for trend analysis

**Frequency:** Monthly, using the previous calendar month's data.

**Alert threshold (per `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 8):**

| Override rate | Alert level | Action |
|---|---|---|
| >20% override of top-quartile candidates | **AMBER** | Review job description quality; assess whether scoring criteria are well-calibrated; manual CV sample |
| >40% override of top-quartile candidates | **RED** | Immediate CTO review; potential prompt miscalibration; escalation to Engineering Lead |
| Non-zero override of any candidate | **Log only** | Aggregate monthly; surface in monitoring report |

**Scout-specific note:** Scout's `assessor_notes` field (populated by the recruiter after review) is a potential qualitative source. Where recruiters document their reasons for overrides, these notes should be sampled quarterly for patterns. This is currently an unstructured field — a future enhancement could add a structured rejection reason dropdown to improve monitoring value.

---

### 3.2 Score distribution analysis

**What it measures:** Changes in the statistical distribution of Scout's `match_score` outputs over time. Sudden shifts in score distribution — e.g., a significant increase in the proportion of candidates scoring below 50 — may indicate model drift, prompt drift, or a change in the incoming candidate pool that warrants investigation.

**Implementation using existing Scout data:**

Query the `match_score` field in PostgreSQL for all screening outputs within a defined period. Calculate:
- Mean, median, standard deviation
- 10th and 90th percentile cutoffs
- Proportion of candidates scoring below 40, 40–60, 60–80, 80–100

Compare against the equivalent statistics from the previous three reporting periods to identify statistically significant shifts.

This analysis does not require demographic data and can be performed entirely on existing structured output data.

**Frequency:** Monthly. Quarterly summary report comparing 12-month trend.

**Alert threshold:**

| Signal | Threshold | Alert level | Action |
|---|---|---|---|
| Mean match score shift | >10 percentage points vs prior 3-month average | AMBER | Investigate: job description quality, Anthropic model version change, candidate pool change |
| <40 score proportion | Increases by >15 percentage points vs baseline | AMBER | Investigate root cause — potential over-restrictive prompt constraint or quality change |

**Segmentation (where volume allows):** Where Scout has sufficient volume per job category, score distribution analysis should be run separately for each job category to detect category-specific anomalies that aggregate analysis would obscure.

---

### 3.3 Keyword and proxy feature audit of outputs

**What it measures:** Whether Scout's output text (`summary` field, `flags_for_human_review`, `matched_criteria`, `unmet_criteria`) contains language patterns associated with demographic proxies or protected characteristic bias.

**Why it matters:** Even if Scout's system prompt explicitly prohibits protected characteristic inference, Claude's underlying model may produce output text that reflects implicit associations. An audit of output language patterns can detect this.

**Scout-specific implementation:**

*Step 1 — Output text audit (quarterly):*

Extract a random sample of minimum 50 completed screening outputs from PostgreSQL, stratified by job category. Review:
- `summary` field: scan for language patterns that correlate with proxy demographics (e.g., frequent references to employment gaps, institution names, naming conventions, language register comments)
- `flags_for_human_review`: assess whether flags disproportionately reference features that could be protected characteristic proxies
- `unmet_criteria`: check whether commonly-cited unmet criteria could constitute indirect discrimination against a group (e.g., "no leadership experience" as a criterion applied differentially)

This is a manual review process. The CTO or a designated reviewer reads the sampled output texts and records observations. Structured output makes this feasible — the review is of the `summary` and flag fields, not free text.

*Step 2 — System prompt audit (annually and pre-deployment):*

Review Scout's system prompt for the following:
- Any instructions that could act as a proxy for a protected characteristic (e.g., "preference for candidates with continuous employment history" as a proxy for pregnancy/maternity/disability)
- Language constraints: are the "do not consider" instructions clear and comprehensive?
- Scoring rubric: are all evaluation criteria job-requirement-linked rather than cultural preference-linked?

The system prompt is Sable AI Ltd's trade secret and is not reproduced in this document. The CTO is the designated reviewer. External prompt audit is recommended at annual minimum (see Section 7).

*Step 3 — Job description review (per submission, where accessible):*

Where Sable AI Ltd has visibility of job descriptions submitted by customers (stored in PostgreSQL), review each for biased language before or shortly after Scout processes it:
- Gendered language: "rockstar", "ninja", "aggressive", "nurturing"
- Age-coded language: "young and dynamic", "digital native", "experienced professional"
- Cultural/socioeconomic proxy language: "prestigious university", specific institution names as requirements

Notify the customer of problematic language identified. [ASSUMPTION — this customer notification process requires an operational procedure to be established.]

**Alert threshold:**

Any proxy feature audit that identifies Scout output language correlating with a protected characteristic pattern → **RED alert** → Immediate prompt review → Remediation per Section 6.

---

### 3.4 A/B outcome tracking for prompt changes

**What it measures:** Whether changes to Scout's system prompt, scoring rubric, or Anthropic model version introduce bias that was not present in the previous version.

**Why it matters for Scout specifically:** Because Scout has no model weights to inspect, the primary intervention point for bias control is the system prompt. Every prompt change is therefore a potential bias introduction event. Testing before and after prompt changes is the most operationally accessible bias control available to Scout.

**Pre-change procedure:**

1. Before any material change to Scout's system prompt, scoring criteria, or Anthropic model version upgrade:
   - Establish a test CV set: minimum 20 CVs covering a range of proxy demographic profiles (CVs with employment gaps; CVs with names drawn from diverse cultural origins; CVs with varying educational institution types; CVs with varying career timeline patterns)
   - Run the current (pre-change) Scout version against this test CV set
   - Record baseline: match scores, confidence ratings, flags_for_human_review, summary content for each test CV

2. This canary test set should be maintained as a version-controlled test fixture, updated as Scout's typical input profile evolves [ASSUMPTION A-023].

**Staging validation procedure:**

1. Deploy the proposed change to a staging environment (AWS staging equivalent of production stack)
2. Run the identical canary test CV set in staging with the changed prompt or model version
3. Compare staging outputs against baseline:
   - Overall score distribution: should remain within ±10 percentage points of baseline mean
   - Per-test-CV score: flag any test CV where score shifts by >20 percentage points vs baseline
   - Flags and summary content: manual review for new proxy feature language

4. If staging tests pass all checks: proceed to production deployment
5. If any test fails: do not deploy; escalate to Engineering Lead and CTO; investigate root cause before proceeding

**Post-deployment monitoring (first 30 days):**

Following any prompt change in production, increase monitoring frequency on the score distribution analysis (Section 3.2) to weekly for the first four weeks.

**Alert threshold:**

| Signal | Threshold | Alert Level | Action |
|---|---|---|---|
| Staging: test CV score shift | >20 pp vs baseline for any test CV | AMBER | Investigate before deployment |
| Staging: proxy language in outputs | Any new proxy pattern detected | RED | Do not deploy; prompt redesign |
| Post-deployment: score mean shift | >10 pp within 30 days of change | AMBER | Elevated monitoring; investigation within 5 business days |

---

## 4. Phase B Monitoring: Voluntary Demographic Monitoring (Pending Lawful Basis)

### 4.1 Preconditions for Phase B

Phase B monitoring — demographic parity testing using candidate-provided equality monitoring data — requires the following to be in place before implementation:

1. **Legal confirmation** of Art. 6(1)(a) consent basis and Art. 9(2)(a) explicit consent condition for voluntary equality monitoring (ACT-006 in `P2-Scout-ICO-Audit-Gap-Analysis-v1.md`) [LEGAL REVIEW REQUIRED]
2. **Customer agreement** to offer voluntary equality monitoring to candidates within their recruitment process — this is a customer-side operational obligation requiring their consent and appropriate candidate-facing communications
3. **Frontend implementation** in Scout's React UI: an optional, clearly-separated equality monitoring questionnaire presented to candidates at the point of CV submission (or by the customer's ATS prior to Scout processing) — genuinely voluntary with no detriment for non-completion
4. **Data model extension** in PostgreSQL: a separate `equality_monitoring` table with restricted access controls, linked to `candidate_id`, containing voluntary demographic data collected under confirmed lawful basis
5. **DPIA update** to reflect the addition of special category data processing for monitoring purposes

Phase B monitoring must not be implemented until all five preconditions are satisfied. [LEGAL REVIEW REQUIRED]

### 4.2 Adverse impact ratio calculation (when Phase B is active)

When voluntary demographic monitoring data is available, apply the four-fifths rule methodology from `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 5 directly.

**Scout-specific implementation:**

1. Query `equality_monitoring` table joined to `screening_outputs` and `recruiter_decisions` tables on `candidate_id`
2. Calculate shortlisting rate per monitored demographic group (as defined by the voluntary questionnaire categories)
3. Calculate AIR = shortlisting rate of group G ÷ shortlisting rate of highest-performing group
4. Apply thresholds from `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 5.3 (RED: AIR <0.80; AMBER: 0.80–0.85; GREEN: ≥0.85)

**Minimum sample size:** 50 candidates per monitored group per reporting period. Below this threshold, note the limitation and apply Phase A aggregate monitoring only for that group.

**Data access controls:** The `equality_monitoring` table must have access restricted to named individuals (CTO + designated monitoring analyst only). It must not be accessible from Scout's core screening workflow or from any customer-facing view. Ensure access is logged.

**Frequency:** Quarterly when sufficient sample sizes exist.

---

## 5. Alert Thresholds Summary (Scout-Specific)

| Signal | Source | Threshold | Alert Level | Action |
|---|---|---|---|---|
| Override rate — top-quartile candidates | Recruiter decision log | >20% | AMBER | Review job description calibration; manual CV sample |
| Override rate — top-quartile candidates | Recruiter decision log | >40% | RED | CTO review; potential prompt issue |
| Score mean shift | Screening outputs | >10 pp vs 3-month baseline | AMBER | Investigate: prompt change, model version, candidate pool |
| Keyword/proxy audit — output text | Output sample | Any proxy pattern confirmed | RED | Immediate prompt review; remediation |
| System prompt audit | System prompt | Any proxy feature in scoring criteria | RED | Prompt redesign before next deployment |
| Staging test: score shift vs baseline | Canary test | >20 pp for any test CV | AMBER | Investigate before deployment |
| Staging test: proxy language in outputs | Canary test | Any new proxy pattern | RED | Do not deploy |
| AIR (Phase B) — live monitoring | Equality monitoring | <0.80 for any group | RED | Escalation; bias investigation; remediation within 30 days |
| AIR (Phase B) — live monitoring | Equality monitoring | 0.80–0.85 for any group | AMBER | Increased frequency; review within 30 days |
| Candidate complaint citing bias | Incident register | Any | AMBER minimum | Trigger reviewer investigation; log in incident register (`L3-4.3-Incident-Response-Plan-v1.md`) |

---

## 6. Remediation Options (Scout-Specific)

Because Scout is a prompt-engineered skill rather than a trained model, the remediation options from `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 10 translate to the following Scout-specific actions:

| Option | Scout-specific action | When to use |
|---|---|---|
| **Prompt adjustment** | Revise the system prompt to remove or reduce the weight of an identified proxy feature; re-run canary test suite immediately post-change | Primary first-line option — accessible, reversible, and testable |
| **Exclusion or de-weighting** | Add identified proxy terms to an explicit "do not reference" or "ignore" instruction in the system prompt | When a specific proxy term or feature is confirmed in output analysis |
| **Human override injection** | Require mandatory human review for all candidates meeting the profile of the affected group until root cause is resolved; flag in Scout UI that AI output requires enhanced review for this screening run | Immediate interim measure — available now; requires no technical change |
| **Prompt version rollback** | Revert to the previous stable system prompt version (version-controlled in code repository) | When bias was introduced by a specific prompt change and cannot be immediately remediated forward |
| **Anthropic model version rollback** | If a Claude API model version upgrade introduced the bias, pin to previous model version in the API configuration | When bias correlates with a model upgrade; note this requires confirmation with Anthropic that the previous model version remains available |
| **Screening suspension** | Suspend Scout shortlisting for the affected role or customer while investigation continues | Last resort — high regulatory risk of continued discriminatory output |

**Post-remediation validation:** After any remediation action, run the full canary test CV set and confirm that the bias signal is resolved before returning to standard monitoring frequency. Document all actions taken.

---

## 7. External Audit

Following the cadence and scope defined in `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 11, applied to Scout's context:

**Annual external audit scope for Scout:**

1. **System prompt review:** Independent assessment of Scout's current system prompt for proxy feature bias, clarity of exclusion instructions, and calibration of scoring criteria against ICO and DSIT guidance
2. **Output sample analysis:** Statistical analysis of Scout's shortlisting outputs across the preceding 12 months — score distribution, flag patterns, output language
3. **Process review:** Assessment of Scout's internal monitoring processes (Sections 3–4 of this document) for adequacy against ICO expectations
4. **Adverse impact analysis** (Phase B prerequisite or post-implementation): Once voluntary monitoring data exists, external auditors should conduct independent AIR calculations

**External auditor access requirements:**

Consistent with `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 11.2, provide:
- Scout's current and previous system prompt versions
- Canary test suite CV set and results
- Score distribution analysis reports from preceding 12 months
- Recruiter override rate monitoring reports
- Incident log entries relating to bias (where any exist)
- Bias monitoring procedure documentation (this document)

Do not provide: raw candidate CV files (S3), individual candidate records, or customer-identifying data without prior consent and legal review.

**Recommended first audit:** Given Scout's current compliance posture, commission an initial external prompt audit before the end of the first quarter of live production customer use. This establishes a baseline and demonstrates proactive compliance to any ICO enquiry. [ASSUMPTION A-023]

---

## 8. Evidence Retention (Scout-Specific)

Store in a designated controlled-access folder in AWS S3 (separate from candidate CV storage) or in a secure document management system. Retain per the periods in `L3-4.2-Bias-Monitoring-Protocol-v1.md` Section 12:

| Record | Storage location | Retention |
|---|---|---|
| Monthly override rate monitoring reports | Secure internal folder | 3 years |
| Score distribution analysis reports (per period) | Secure internal folder | 3 years |
| Canary test CV set and results (per version) | Version-controlled repository alongside prompt versions | 3 years per version |
| System prompt versions | Version-controlled code repository (e.g., Git) | 3 years per version; active prompt indefinitely |
| Keyword/output audit reports | Secure internal folder | 3 years |
| External audit reports | Secure internal folder | 3 years minimum |
| Phase B — equality monitoring data (if implemented) | Separate restricted RDS table (`equality_monitoring`) | Minimum retention; delete when no longer needed for monitoring purpose; not to be retained beyond role closure + 6 months without specific justification [LEGAL REVIEW REQUIRED] |

**PostgreSQL audit log retention:** Scout already retains audit logs for 2 years (Brief Section 7). The screening outputs required for bias monitoring analysis should be retained consistent with the 6-month candidate data retention period — but the *aggregated statistics* derived from those outputs (score distributions, override rates, AIR calculations) should be retained separately for 3 years as accountability evidence.

---

## 9. Implementation Roadmap

A phased implementation plan for Sable AI Ltd, ordered by accessibility and priority:

### Immediate (before live production processing)

| Action | Owner | Technical requirement |
|---|---|---|
| Establish canary test CV set (minimum 20 CVs, proxy-diverse profiles) | CTO | Manual compilation; store as test fixtures |
| Run baseline canary test against current prompt | CTO / Engineering Lead | Execute Scout against test fixtures; record and version outputs |
| Confirm override decision data is structured in PostgreSQL (not free text only) | Engineering Lead | Check recruiter_decisions table schema; add structured `decision` enum field if not already present |
| Conduct initial keyword audit of current system prompt | CTO | Manual review against proxy feature checklist from Section 3.3 |

### Within 30 days

| Action | Owner | Technical requirement |
|---|---|---|
| Implement override rate monitoring query and monthly reporting | Engineering Lead | SQL query on `recruiter_decisions` joined to `screening_outputs`; monthly scheduled report |
| Implement score distribution analysis query and monthly reporting | Engineering Lead | SQL query on `match_score` field; statistical summary; trend comparison |
| Establish monitoring report template and review meeting cadence | CTO | Template document; schedule monthly review |

### Within 90 days

| Action | Owner | Technical requirement |
|---|---|---|
| Conduct first output text audit (50-CV sample) | CTO | Manual review using structured query to extract `summary`, `flags_for_human_review`, `unmet_criteria` |
| Commission external prompt audit | Founder / CTO | Engage external consultant; provide prompt access and output sample |
| Establish canary test in staging environment pipeline | Engineering Lead | Integrate test fixture execution into pre-deployment process |

### After lawful basis confirmed (Phase B)

| Action | Owner | Dependency |
|---|---|---|
| Add voluntary equality monitoring questionnaire to Scout frontend (React) | Engineering Lead | Legal confirmation (ACT-006 in `P2-Scout-ICO-Audit-Gap-Analysis-v1.md`) |
| Create `equality_monitoring` table in PostgreSQL with restricted access | Engineering Lead | Legal confirmation; DPIA update |
| Update DPIA to reflect special category data processing | CTO / Founder | DPIA completion (ACT-001) + legal confirmation |
| Implement quarterly AIR calculation using voluntary monitoring data | Engineering Lead | Phase B data available; minimum sample size achieved |

---

## 10. What This Document Adds Over L3-4.2

For any organisation adapting this framework, the value of the Phase 2 worked example is in demonstrating that:

1. **Option B monitoring is immediately accessible** for Scout using existing PostgreSQL data — no new collection, no legal complexity, and no infrastructure changes required. A company at Scout's stage can implement meaningful (if limited) bias monitoring within weeks.

2. **Prompt-engineering architectures require prompt-level audit** as a first-class bias monitoring activity — this is specific to Scout's architecture and is not a default component of bias monitoring frameworks designed for trained models.

3. **The demographic data constraint is manageable in Phase A** — the absence of demographic data means Scout cannot perform ICO-recommended adverse impact analysis, but this does not mean no monitoring is possible. Override rate, score distribution, and keyword analysis provide a defensible baseline while the lawful basis question for Phase B is resolved.

4. **Remediation is more accessible than for trained models** — prompt changes are faster and more reversible than model retraining. This is a genuine compliance advantage of Scout's architecture, even if the opacity of model internals is a constraint.

---

## Cross-References

| Document | Relevance |
|---|---|
| `L3-4.2-Bias-Monitoring-Protocol-v1.md` | Framework-level methodology — read alongside this document |
| `L3-4.1-Monitoring-Framework-v1.md` | Metrics M-13–M-15; monitoring governance and dashboard design |
| `L3-4.3-Incident-Response-Plan-v1.md` | Escalation when bias finding meets incident threshold |
| `L2-3.2-Equality-Act-2010-Compliance-Map-v1.md` | Protected characteristics and Equality Act 2010 discrimination risk |
| `L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` | Art. 9 lawful basis analysis for demographic data |
| `P2-Scout-ICO-Audit-Gap-Analysis-v1.md` | ACT-005 through ACT-010 — operational action register this document implements |
| `P2-Scout-System-Profile-v1.md` | Technical architecture detail (stack, workflow, data flows) |
