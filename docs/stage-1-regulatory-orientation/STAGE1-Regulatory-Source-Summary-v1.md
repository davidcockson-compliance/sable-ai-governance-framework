# Regulatory Source Summary
**Project:** Sable AI Ltd — AI Governance Framework
**Stage:** Stage 1 — Regulatory Orientation
**Status:** Draft
**Version:** v1
**Date:** 2026-02-28
**Assumptions:** Built on outline assumptions — not verified against real Sable AI Ltd data

---

## Purpose of This Document

This document is the definitive registry of all source documents used to build the Sable AI Ltd AI Governance Framework. It is the authoritative record for what is listed in `_input/CITATIONS-REGISTER.md`.

Every document cited as authority in any stage of this framework must have an entry here. If a source is cited in a framework document but not listed here, it has not been validated and should not be relied on.

---

## Priority Classification

| Priority | Meaning |
|---|---|
| **Critical** | Source is essential to the legal and regulatory foundation of the framework. Missing or inaccurate content from these sources would invalidate core compliance conclusions. |
| **High** | Source is required for specific stages or documents. Framework is materially incomplete without these sources. |
| **Medium** | Supporting source. Adds depth or context but the framework's core conclusions are not dependent on it. |

---

## Primary Enforcement Benchmark — ICO AI in Recruitment Outcomes Report (November 2024)

**This source is the single most important document in the framework and must be treated as the primary compliance target.**

The ICO conducted proactive audits of AI providers and recruiters operating in the recruitment sector and published findings in November 2024 (S-005 below). The report:

- Identifies the specific compliance failures the ICO found in recruitment AI tools at live audit
- Establishes what remediation ICO expects to see
- Signals that ICO is conducting proactive regulatory audits of this sector — not waiting for complaints
- Directly benchmarks Scout's use case: AI-powered CV screening and candidate shortlisting

Any future ICO audit of Sable AI Ltd [ASSUMPTION A-001, A-002] or its customers [ASSUMPTION A-003] would be measured against this report's recommendations. All framework documents in Stages 3 and 4 trace compliance obligations back to this source. See `L2-3.3-ICO-Audit-Gap-Analysis-v1.md` (forthcoming) for the structured gap analysis against this report's findings.

---

## Source Registry

### Critical Sources

| ID | Document | Issuer | Date | URL | Local filename | Stage relevance | Priority |
|---|---|---|---|---|---|---|---|
| S-001 | UK GDPR (retained EU law, as amended by DUAA 2025) | UK Parliament / legislation.gov.uk | 2018, as amended 2025 | https://www.legislation.gov.uk/eur/2016/679/contents | `uk-gdpr-full-text.txt` | Stages 1–5; all framework documents | **Critical** |
| S-002 | Data Protection Act 2018 | UK Parliament | 2018 | https://www.legislation.gov.uk/ukpga/2018/12/contents | `dpa-2018.txt` | Stages 1–3; Schedule 1 conditions for special category data | **Critical** |
| S-003 | Data (Use and Access) Act 2025 | UK Parliament | 2025 | https://www.legislation.gov.uk/ukpga/2025/1/contents | `data-use-access-act-2025.txt` | Stages 1–3; ADM provisions (Arts. 22A–22D) | **Critical** |
| S-004 | ICO Guidance on AI and Data Protection | ICO | 2023 (updated) | https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/artificial-intelligence/guidance-on-ai-and-data-protection/ | `ico-ai-data-protection-guidance.txt` | Stages 1–4; lawful basis, fairness, data minimisation in AI | **Critical** |
| S-005 | ICO AI in Recruitment Outcomes Report — **PRIMARY ENFORCEMENT BENCHMARK** | ICO | November 2024 | https://ico.org.uk/about-the-ico/media-centre/news-and-blogs/2024/11/ico-publishes-outcomes-of-ai-in-recruitment-audits/ | `ico-ai-recruitment-outcomes-2024.txt` | Stages 1–5; primary audit benchmark across all stages | **Critical** |
| S-006 | ICO Guidance on Automated Decision-Making and Profiling | ICO | 2020 (updated) | https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/individual-rights/automated-decision-making-and-profiling/ | `ico-adm-profiling-guidance.txt` | Stages 1–3; Art. 22A–22C analysis and "meaningful human involvement" test | **Critical** |

---

### High Priority Sources

| ID | Document | Issuer | Date | URL | Local filename | Stage relevance | Priority |
|---|---|---|---|---|---|---|---|
| S-007 | Equality Act 2010 | UK Parliament | 2010 | https://www.legislation.gov.uk/ukpga/2010/15/contents | `equality-act-2010.txt` | Stages 1–3; protected characteristics; indirect discrimination; s.4, s.19, s.39, Sch. 1 | **High** |
| S-008 | EHRC Employment Statutory Code of Practice | Equality and Human Rights Commission | 2011 | https://www.equalityhumanrights.com/en/publication-download/employment-statutory-code-practice | `ehrc-employment-code.txt` | Stage 3; employer obligations; indirect discrimination framework; EHRC enforcement powers | **High** |
| S-009 | DSIT Responsible AI in Recruitment Guide | Department for Science, Innovation and Technology | March 2024 | https://www.gov.uk/government/publications/responsible-ai-in-recruitment-guide | `dsit-responsible-ai-recruitment-2024.txt` | Stages 1, 3, 4; bias audit methodology; governance framework; transparency requirements | **High** |

---

### Medium Priority Sources

| ID | Document | Issuer | Date | URL | Local filename | Stage relevance | Priority |
|---|---|---|---|---|---|---|---|
| S-010 | ICO AI and Biometrics Strategy | ICO | 2023 | https://ico.org.uk/about-the-ico/our-information/our-strategies-and-plans/ico25-strategic-plan/ai-and-analytics/ | `ico-ai-biometrics-strategy.txt` | Stage 4; relevant if Scout incorporates biometric or image-based assessment features | **Medium** |
| S-011 | ICO DPIA Guidance | ICO | 2018 (updated) | https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/accountability-and-governance/data-protection-impact-assessments-dpias/ | `ico-dpia-guidance.txt` | Stage 3; DPIA structure and ICO requirements for `L2-3.4-DPIA-Template-v1.md` | **Medium** |

---

### Phase 2 Sources

| ID | Document | Issuer | Date | URL | Local filename | Stage relevance | Priority |
|---|---|---|---|---|---|---|---|
| S-012 | Anthropic product documentation / API documentation | Anthropic | 2024 | https://github.com/anthropics/anthropic-quickstarts | `anthropic-pm-plugin-readme.txt` | Phase 2 only; Scout worked examples requiring specific Claude API implementation detail [ASSUMPTION A-002] | **Medium** |

---

## Source Retrieval Status

| ID | Status |
|---|---|
| S-001 | 📝 External extract — Appendix A of `_input/stage-1-regulatory-extracts.md` covers Arts. 4, 5, 6, 9, 13, 14, 22, 33, 35. Full text not yet retrieved as `.txt`. |
| S-002 | 📝 External extract — Appendix B of `_input/stage-1-regulatory-extracts.md` covers Schedule 1 paras 8–10. Full text not yet retrieved as `.txt`. |
| S-003 | 📝 External extract — Section 4 of `_input/stage-1-regulatory-extracts.md` covers DUAA 2025 s.80 (Arts. 22A–22D) and Sch. 6 paras 3–5. Full text not yet retrieved as `.txt`. |
| S-004 | 📝 External extract — Section 2 of `_input/stage-1-regulatory-extracts.md` covers lawfulness, fairness, data minimisation, and inferred special category data provisions. Full text not yet retrieved as `.txt`. |
| S-005 | 📝 External extract — Sections 1A and 1B of `_input/stage-1-regulatory-extracts.md` cover the ICO news summary and full Outcomes Report recommendations. Full text not yet retrieved as `.txt`. |
| S-006 | 📝 External extract — Section 3 of `_input/stage-1-regulatory-extracts.md` covers automated decision-making and profiling provisions. Full text not yet retrieved as `.txt`. |
| S-007 | 📝 External extract — Section 5 of `_input/stage-1-regulatory-extracts.md` covers s.4, s.19, s.39, and Schedule 1 paras 2, 3, 5, 5A, 6, 8, 9. Full text not yet retrieved as `.txt`. |
| S-008 | 🔗 URL only — not yet retrieved. Required before Stage 3 Run A drafting begins. |
| S-009 | 📝 External extract — Appendix C of `_input/stage-1-regulatory-extracts.md` covers key guidance outputs. Full text not yet retrieved as `.txt`. |
| S-010 | 🔗 URL only — not yet retrieved. Required only if Stage 4 documents need biometrics-specific provisions. |
| S-011 | 🔗 URL only — not yet retrieved. Required before Stage 3 Run B (`L2-3.4-DPIA-Template-v1.md`) begins. |
| S-012 | 🔗 URL only — not required until Phase 2. |

**Status key:** 📝 External extract (provisions in stage extraction file) | 🔗 URL only (not yet retrieved) | ✅ Retrieved (`.txt` in `_input/_raw-sources/`)

---

## Retrieval Priority for Upcoming Stages

To unblock Stage 2 drafting: all Stage 2 documents can be drafted from the existing Stage 1 extraction file. No additional sources required.

To unblock Stage 3 Run A (`L2-3.1-UK-GDPR-Mapping-Matrix-v1.md` and `L2-3.2-Equality-Act-2010-Compliance-Map-v1.md`): a Stage 3 extraction file will be required, drawing primarily on S-001, S-004, S-005, S-006, S-007, and S-009.

To unblock Stage 3 Run B (`L2-3.3-ICO-Audit-Gap-Analysis-v1.md` and `L2-3.4-DPIA-Template-v1.md`): S-008 (EHRC Code) and S-011 (ICO DPIA Guidance) should be retrieved or extracted before Run B begins.

**File size warning:** S-001 (UK GDPR full text) and S-002 (DPA 2018 full text) will produce large `.txt` files estimated at 200–500KB+. These must never be read directly during a drafting run — only through pre-prepared stage extraction files. See `_input/_raw-sources/README.md` for conversion instructions.

---

## Adding New Sources

If a source is cited in any framework document but is not listed here:

1. Add an entry to this document in the appropriate priority section
2. Add a corresponding entry to `_input/CITATIONS-REGISTER.md`
3. Retrieve the source and update its status

Sources not registered here are not validated and must not be cited as authority in any framework document.

---

*This document is maintained by Assumptions Tracker (Agent 4) throughout every run. All entries were verified against `_input/CITATIONS-REGISTER.md` at the time of writing. Last updated: 2026-02-28.*
