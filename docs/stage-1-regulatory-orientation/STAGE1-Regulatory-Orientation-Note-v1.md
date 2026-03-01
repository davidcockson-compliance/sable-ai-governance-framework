# Regulatory Orientation Note
**Project:** Sable AI Ltd — AI Governance Framework
**Stage:** Stage 1 — Regulatory Orientation
**Status:** Draft
**Version:** v1
**Date:** 2026-02-28
**Assumptions:** Built on outline assumptions — not verified against real Sable AI Ltd data

---

## Purpose of This Document

This note provides an orientation-level overview of the UK regulatory instruments applicable to Scout, Sable AI Ltd's AI-powered CV screening and candidate shortlisting tool. It establishes the regulatory architecture before the detailed obligation mapping in Stages 2–4.

Every claim relying on assumed characteristics of Sable AI Ltd is flagged `[ASSUMPTION]`. Areas requiring review by a qualified UK data protection or employment solicitor are flagged `[LEGAL REVIEW REQUIRED]`.

**This document does not constitute legal advice.**

---

## 1. Summary of Applicable Instruments

| Instrument | Type | Status | Primary relevance to Scout |
|---|---|---|---|
| UK GDPR (retained EU law, as amended) | Retained EU law | In force | Core data protection obligations for CV processing; ADM rules |
| Data Protection Act 2018 (DPA 2018) | UK primary legislation | In force | Implements UK GDPR; Schedule 1 conditions for special category data |
| Data (Use and Access) Act 2025 (DUAA 2025) | UK primary legislation | In force | Amends UK GDPR: replaces Art. 22 with Articles 22A–22D |
| Equality Act 2010 (EA 2010) | UK primary legislation | In force | Indirect discrimination risk; protected characteristics; reasonable adjustments |
| ICO Guidance on AI and Data Protection | Regulatory guidance | Current | ICO interpretation of UK GDPR obligations applied to AI systems |
| ICO AI in Recruitment Outcomes Report (Nov 2024) | Enforcement and audit output | Published Nov 2024 | **Primary enforcement benchmark** — live ICO audit findings from Scout's exact use case |
| DSIT Responsible AI in Recruitment Guide (Mar 2024) | Government guidance | Published Mar 2024 | Best practice methodology: bias auditing, governance, transparency |

---

## 2. Instrument-by-Instrument Analysis

### 2.1 UK GDPR (Retained EU Law, as amended)

**Status:** In force. The UK GDPR is the retained form of EU Regulation 2016/679, incorporated into UK domestic law by the European Union (Withdrawal) Act 2018. It has been amended by the Data (Use and Access) Act 2025, most significantly in the automated decision-making provisions (see Section 2.3).

**Scope:** Applies to processing of personal data by controllers or processors established in the UK, and to processing of personal data of data subjects in the UK regardless of where the controller is established.

[ASSUMPTION A-001, A-006] Sable AI Ltd is UK-established and processes UK candidate personal data on behalf of UK customers. UK GDPR applies to Scout in full.

**Key articles for recruitment AI:**

| Article | Obligation | Relevance to Scout |
|---|---|---|
| Art. 4 | Definitions: personal data, processing, profiling | CV ingestion, parsing, scoring, ranking, and trait inference all constitute processing; candidate scoring is likely profiling |
| Art. 5 | Data protection principles: lawfulness/fairness/transparency; purpose limitation; data minimisation; accuracy; storage limitation; integrity and confidentiality | All six principles apply to each processing stage of Scout's CV workflow |
| Art. 6 | Lawful basis for processing | A separate lawful basis must be identified and documented for each distinct processing operation — not a single basis for the whole system |
| Art. 9 | Special category data | Processing revealing racial/ethnic origin, health, disability, religion, or biometric data is prohibited without an Art. 9(2) exception; Scout may infer such characteristics from CV content [LEGAL REVIEW REQUIRED] |
| Art. 13 | Transparency: data collected from the data subject | Candidate-facing notice required wherever candidates submit CVs directly |
| Art. 14 | Transparency: data not collected from the data subject | Required for sourced candidates, agency CVs, and externally obtained profile data |
| Arts. 15–22 | Data subject rights | Candidates retain rights of access, rectification, erasure, restriction, objection, and portability |
| Arts. 22A–22C (inserted by DUAA 2025) | Automated decision-making safeguards | Replaces pre-DUAA Art. 22 — see Section 2.3 |
| Art. 33 | Breach notification | 72-hour notification to ICO where a breach is likely to result in risk to individuals' rights and freedoms |
| Art. 35 | Data Protection Impact Assessment | DPIA required before processing where likely high risk — ICO has confirmed AI in recruitment triggers this threshold |

**Lawful basis — critical obligation:** The ICO guidance is explicit that each distinct processing operation must have its own identified lawful basis, determined before processing begins, documented, and disclosed in the privacy notice. Legitimate interests under Art. 6(1)(f) is the most commonly applicable basis for core screening activities [ASSUMPTION], but requires a three-part necessity and proportionality test. [LEGAL REVIEW REQUIRED]

---

### 2.2 Data Protection Act 2018

**Status:** In force. The DPA 2018 is the UK primary legislation that supplements and implements the UK GDPR. It cannot be read in isolation — it must be read alongside the UK GDPR as a unified framework.

**Scope:** Same territorial scope as the UK GDPR.

**Key relevance to Scout — Schedule 1 conditions for special category data:**

Article 9 UK GDPR prohibits processing special category data unless both an Art. 9(2) exception and a corresponding DPA 2018 Schedule 1 condition are met. Three Schedule 1 conditions are potentially relevant in a recruitment AI context:

| Schedule 1 Para | Condition | Relevance and limitations |
|---|---|---|
| Para 8 — Equality of opportunity or treatment | Permits processing special category data to identify or keep under review the existence or absence of equality of opportunity between groups, with a view to enabling that equality to be promoted or maintained; must not require consent and must not be likely to cause substantial damage or distress | Potentially relevant to bias monitoring — but is a narrow condition. The purpose must specifically be equality monitoring, not general screening or scoring |
| Para 9 — Racial and ethnic diversity at senior positions | Permits use of racial/ethnic origin data in identifying suitable individuals for senior positions, where necessary for promoting diversity at senior levels | Extremely narrow — does not create any general licence for ethnic-origin data use in ordinary candidate screening |
| Para 10 — Preventing unlawful acts | Permits processing for prevention or detection of an unlawful act, where processing without consent is necessary to avoid prejudicing that purpose | Relevant to fraud prevention and identity verification — not applicable to ordinary candidate scoring or shortlisting |

**Practical implication:** If Scout's model outputs or processing activities involve special category data — including characteristics inferred from CV content, such as ethnicity, disability, or health — Sable AI Ltd [ASSUMPTION A-002] must identify both an Art. 9(2) UK GDPR exception and the corresponding Schedule 1 DPA 2018 condition for each such processing activity. The ICO's November 2024 audit found that several AI providers were processing inferred special category data without any lawful basis. [LEGAL REVIEW REQUIRED]

---

### 2.3 Data (Use and Access) Act 2025 — ADM Provisions

**Status:** In force. The Data (Use and Access) Act 2025 materially amended the UK GDPR's automated decision-making framework. The operative UK rules for automated recruitment decisions are now Articles 22A to 22D of the UK GDPR (inserted by DUAA 2025 s.80), not the pre-DUAA Article 22 text.

**Scope:** Applies to "significant decisions" based entirely or partly on automated processing where there is no "meaningful human involvement" in taking the decision.

**Key provisions:**

| Provision | Rule | Relevance to Scout |
|---|---|---|
| Art. 22A(1)(a) | A decision is "based solely on automated processing" if there is **no meaningful human involvement** in taking it | Whether Scout's shortlisting outputs involve genuine meaningful human review is the central ADM compliance question [ASSUMPTION A-007] |
| Art. 22A(1)(b) | A decision is "significant" if it produces a legal effect or a **similarly significant effect** for the data subject | Hiring shortlists, rejection decisions, and interview-invite determinations are likely to produce similarly significant effects on candidates' access to employment |
| Art. 22A(2) | In assessing meaningful human involvement, the degree of profiling involved must be considered | Scout's scoring and ranking are profiling activities — a higher profiling component reduces the likelihood that nominal sign-off constitutes genuine meaningful involvement |
| Art. 22B(1) | Significant decisions based entirely or partly on special category processing may not be taken solely by automated processing unless statutory conditions are met | Where Scout's outputs rely on inferred ethnicity, disability, or health data, this prohibition applies [LEGAL REVIEW REQUIRED] |
| Art. 22C(1) | Where significant decisions are based solely on automated processing, the controller must ensure statutory safeguards are in place | The controller is the customer (recruiter / HR team) [ASSUMPTION A-003]; Sable AI Ltd is processor — but Sable AI's product design must enable customers to meet their controller obligations |
| Art. 22C(2)(a)–(d) | Safeguards must: provide information to the data subject; enable representations; enable human intervention; enable the data subject to contest the decision | Minimum statutory floor — any Scout deployment producing significant automated decisions must have all four safeguards in place |
| Art. 22D | Secretary of State may make regulations on "meaningful human involvement", "similarly significant effect", and additional safeguard requirements | Framework is subject to secondary legislation — governance structures must be reviewed as regulations are published |

**Critical question:** The "meaningful human involvement" test under Art. 22A(1)(a) is substantive, not formal. A recruiter who receives a Scout shortlist and passes it to a client without independent assessment has not provided meaningful involvement. [ASSUMPTION A-007 — mandatory human review is assumed but its quality and substantiveness are unverified.] [LEGAL REVIEW REQUIRED]

---

### 2.4 Equality Act 2010

**Status:** In force. The EA 2010 is the primary UK legislation on discrimination, harassment, and victimisation in protected-characteristic contexts. It operates independently of the data protection regime — a processing activity may be UK GDPR-compliant but still constitute unlawful discrimination. Both regimes must be satisfied simultaneously.

**Scope:** Applies to employers (s.39) making arrangements for deciding to whom to offer employment. Applies directly to Sable AI Ltd's customers making hiring decisions [ASSUMPTION A-003], and potentially to Sable AI Ltd as a technology enabler depending on the legal relationship.

**Protected characteristics (s.4):**

Age — disability — gender reassignment — marriage and civil partnership — pregnancy and maternity — race — religion or belief — sex — sexual orientation.

**Key provisions:**

| Section | Provision | Relevance to Scout |
|---|---|---|
| s.19(1)–(2) | Indirect discrimination: applying a provision, criterion, or practice (PCP) that puts persons with a protected characteristic at a particular disadvantage compared to others, where that PCP cannot be justified as a proportionate means of achieving a legitimate aim | Screening criteria, keyword filters, scoring thresholds, and model training choices are all potential PCPs. A facially neutral requirement may cause disproportionate disadvantage to a protected group |
| s.39(1) | An employer must not discriminate in the arrangements made for deciding to whom to offer employment, or by not offering employment | AI-assisted shortlisting is squarely within "arrangements for deciding" — the EA 2010 applies even where no single human makes an autonomous decision |
| s.39(5) | Duty to make reasonable adjustments applies to employers in recruitment | Online assessments, timed requirements, AI interview tools, and CV parsing features must accommodate disabled applicants |
| Sch. 1, para 2(1) | An impairment is long-term if it has lasted or is likely to last at least 12 months | Defines when disability protection applies — relevant to determining which candidates fall within the protected characteristic |
| Sch. 1, para 5 | A disability is assessed as if no treatment or aids are taken to correct it | Relevant where performance or capability data is gathered from candidates managing a condition |
| Sch. 1, para 5A(2) | Disability analysis includes the ability to participate fully and effectively in working life on an equal basis | Directly relevant to AI recruitment tools that assess "suitability" or "fit" |

**Proportionality justification:** Even where an AI criterion causes disparate impact on a protected group, a justified business aim may prevent it being unlawful — but the justification must be evidenced. Bias audit records and testing results are the primary evidence base. [LEGAL REVIEW REQUIRED]

---

### 2.5 ICO Guidance on AI and Data Protection

**Status:** Current regulatory guidance. Not a legally binding statute, but represents the ICO's authoritative interpretation of UK GDPR obligations as they apply to AI — and therefore the standard against which the ICO assesses compliance.

**Scope:** Applies to all UK organisations using AI that processes personal data. Not sector-specific, but directly relevant to recruitment AI.

**Key guidance positions relevant to Scout:**

- **Lawful basis per processing operation:** Each distinct processing step — CV ingestion, parsing, scoring, shortlisting, bias monitoring, model improvement — requires its own identified lawful basis, determined before processing starts and not swappable afterwards.
- **Legitimate interests test:** The ICO guidance confirms that demonstrating legitimate interests requires necessity and proportionality — and that the "mere possibility that some data might be useful for a prediction is not by itself sufficient to demonstrate that processing this data is necessary."
- **Inferred special category data:** Where an AI system intentionally draws an inference about a characteristic that falls within the special category definition — such as inferring ethnicity from a name — the ICO treats that inferred data as special category data subject to Art. 9 restrictions.
- **Data minimisation applies to AI features:** The data minimisation principle applies to model inputs, training data, and inference features — not only to records retained after processing.
- **Procurement due diligence:** Organisations buying in AI should treat data minimisation requirements as part of the procurement process — not a post-purchase consideration.
- **Fairness and discrimination:** Processing that leads to unjust discrimination between people violates the Art. 5(1)(a) fairness principle regardless of the technical mechanism producing that discrimination.
- **Development vs deployment purposes:** Where a third party developed the system, the customer deploying it is processing for a different purpose — and may need to identify a different lawful basis from the developer.

---

### 2.6 ICO AI in Recruitment Outcomes Report (November 2024)

**Status:** Published enforcement and audit output. This is the primary enforcement benchmark for Scout's compliance baseline. The ICO conducted proactive audits of AI recruitment tool providers and recruiters using such tools and published its findings in November 2024.

**Why this is the primary benchmark:** The audited tools are directly analogous to Scout. Any future ICO audit of Sable AI Ltd or its customers would be measured against the findings and recommendations in this report. It represents live regulatory expectations, not theoretical guidance.

**Key audit findings and framework implications:**

| Finding area | ICO finding | Framework implication for Scout |
|---|---|---|
| Discriminatory filtering | Some tools allowed recruiters to filter candidates by protected characteristics | Scout must not include any feature enabling positive or negative selection by protected characteristic [ASSUMPTION A-002] |
| Inferred characteristics | Several providers estimated or inferred gender and ethnicity from CV content or names — the ICO confirmed inferred data is still special category data | Sable AI Ltd must assess whether Scout infers protected characteristics and, if so, must have both Art. 6 and Art. 9 lawful bases [ASSUMPTION A-002, LEGAL REVIEW REQUIRED] |
| Unlawful special category processing | Inferred characteristics were frequently processed without lawful basis and without candidates' knowledge | Art. 9(2) exception + Schedule 1 DPA 2018 condition are both required — not optional |
| Transparency failures | Candidates and recruiters were often unaware AI was being used or how it functioned | Customer-facing contractual transparency obligations and candidate-facing notices must both be addressed (see `L4-5.2-Candidate-Transparency-Notice-v1.md`, forthcoming) |
| Human review quality | ICO reviewed whether Art. 22/22A compliance was genuine — found that random and risk-based reviews are required, not token sign-off | Scout's human review [ASSUMPTION A-007] must be substantively meaningful — review design must be documented and auditable |
| Bias monitoring | Providers should test regularly for fairness and bias, report KPIs, and retain test results and evidence of remediation | Regular bias testing with documented results is a core operational requirement, not an aspirational good practice (see `L3-4.2-Bias-Monitoring-Protocol-v1.md`, forthcoming) |
| DPIA | DPIAs must be completed prior to processing where likely high risk | ICO confirmed AI in recruitment triggers Art. 35 — DPIA is mandatory before Scout is deployed (see `L2-3.4-DPIA-Template-v1.md`, forthcoming) |
| Controller/processor clarity | AI provider role as controller or processor must be correctly identified; where provider builds candidate databases, provider may be a controller or joint controller | Sable AI Ltd's role in the data supply chain must be correctly designated for each customer type [LEGAL REVIEW REQUIRED] |
| Data supply chain | Complex data supply chains require documented data protection responsibilities in contracts | DPAs with customers and sub-processor documentation (Anthropic) required (see `L4-5.1-Data-Processing-Agreement-Template-v1.md`, forthcoming) |

**Enforcement posture:** The November 2024 report signals that ICO is conducting proactive audits of recruitment AI providers — not waiting for complaints. Non-compliance in this sector is a live enforcement risk.

---

### 2.7 DSIT Responsible AI in Recruitment Guide (March 2024)

**Status:** Government-published guidance. Published by the Department for Science, Innovation and Technology. Not statutory — but sets government expectations and informs what "responsible" deployment looks like.

**Scope:** Best practice guidance for organisations deploying AI across the full recruitment lifecycle: sourcing, screening, interview, and selection.

**Key guidance outputs relevant to Scout:**

- AI deployment should adhere to five UK AI regulatory principles: safety, security and robustness; appropriate transparency and explainability; fairness; accountability and governance; contestability and redress.
- Organisations should establish in advance how they will maintain effective human oversight of AI outputs.
- A DPIA is required for all development and deployment of AI systems involving personal data.
- Bias audits should be conducted before deployment and repeated at regular intervals after the system goes live. The guide's worked example evaluates model performance across sex, ethnicity, age, and disability — noting that neurodivergent applicants may be systematically disadvantaged.
- The use of AI should be clearly signposted to applicants before the system launches.
- Applicants cannot contest AI-influenced decisions unless they know AI was used — transparency and contestability are linked.
- A minimum feedback system should enable users to report issues, flag severity, and identify whether the issue prevented an applicant from progressing.
- An AI governance framework should assign accountability, establish escalation paths, set transparency commitments, and specify how applicants are informed about AI use.

**Assessment:** The DSIT guide provides the methodological foundation for Scout's bias monitoring requirements (Stages 3–4) and frames the governance structure required at Stage 2. It is best read as a practical implementation companion to the ICO's November 2024 enforcement output.

---

## 3. Regulatory Priority and Interaction

The regulatory instruments applicable to Scout interact as follows:

1. **UK GDPR + DPA 2018** — the primary legal framework. Non-compliance creates direct ICO enforcement liability.
2. **Data (Use and Access) Act 2025** — amends the UK GDPR ADM provisions. The current operative rules are Articles 22A–22D. Pre-DUAA Article 22 wording is no longer the operative text for UK processing.
3. **Equality Act 2010** — operates independently of the data protection regime. A processing activity may be UK GDPR-compliant but still constitute unlawful indirect discrimination. Both regimes must be satisfied simultaneously.
4. **ICO AI in Recruitment Outcomes Report (Nov 2024)** — primary enforcement benchmark. Establishes what the ICO will look for when auditing recruitment AI providers. Should be treated as the compliance target.
5. **DSIT Responsible AI in Recruitment Guide (Mar 2024)** — implementation methodology. Provides practical guidance on governance, bias monitoring, human oversight, and transparency.

**The four critical compliance questions that flow through Stages 2–4:**

1. Does Scout's shortlisting output constitute a "significant decision" under Art. 22A UK GDPR?
2. Does Scout's processing produce or rely on inferred special category data (particularly ethnicity, disability, or health)?
3. Is the mandatory human review [ASSUMPTION A-007] substantively meaningful — or merely formal?
4. Is each customer type (recruitment agency; in-house HR) correctly designated as controller or processor in the data supply chain?

---

## 4. UK vs EU Regulatory Architecture — Comparison Table

*For readers familiar with the EU GDPR and EU AI Act frameworks. Note: both architectures are actively evolving.*

| Dimension | UK | EU |
|---|---|---|
| **Primary data protection law** | UK GDPR (retained EU law, as amended by DUAA 2025) | EU GDPR (Regulation 2016/679), as complemented by the EU AI Act |
| **AI-specific legislation** | None enacted as of 2026. UK takes a pro-innovation, sector-led approach. ICO is the primary de facto AI regulator via data protection enforcement powers. | EU AI Act (Regulation 2024/1689). In force August 2024; most obligations apply from August 2026. |
| **Recruitment AI — classification** | No statutory AI risk classification. Recruitment AI is treated as high-risk under data protection principles (DPIA, Art. 35) and ICO enforcement practice. | EU AI Act Annex III, Category 4: AI used in employment and workers management, including candidate selection and evaluation, is classified as **high-risk**. Mandatory obligations apply. |
| **Automated decision-making rules** | Articles 22A–22D UK GDPR (inserted by DUAA 2025). Key test: "meaningful human involvement". Safeguards: information, representations, human intervention, and contestation. | EU GDPR Article 22: data subjects have the right not to be subject to solely automated decisions with legal or significant effects. EU AI Act overlays additional human oversight, transparency, and logging obligations for high-risk systems. |
| **Special category data + ADM** | Art. 22B UK GDPR: significant automated decisions may not be taken solely by automated processing if based on special category data unless statutory conditions are met | EU GDPR Art. 22(4): explicit consent or substantial public interest required for ADM involving special category data. EU AI Act reinforces this through mandatory conformity assessment for high-risk systems. |
| **Equality / anti-discrimination** | Equality Act 2010 — indirect discrimination via PCP framework (s.19). EHRC enforcement. | EU Member State national equality laws implementing Anti-Discrimination Directives (2000/43/EC, 2000/78/EC). No single equivalent to the UK EA 2010. |
| **Regulator(s)** | ICO (data protection); EHRC (equality). No dedicated AI regulator or AI Act equivalent. | National supervisory authorities (GDPR). European AI Office (established 2024, EU AI Act). |
| **Enforcement sanctions** | UK GDPR: up to £17.5m or 4% of global annual turnover (whichever is higher). EA 2010: employment tribunal awards (uncapped for discrimination); EHRC injunctions. | EU GDPR: up to €20m or 4% of global annual turnover. EU AI Act: up to €30m or 6% of global annual turnover for most serious violations. |
| **Recruitment-AI-specific guidance** | ICO AI in Recruitment Outcomes Report (Nov 2024); DSIT Responsible AI in Recruitment Guide (Mar 2024). Sector-specific but non-statutory. | EU AI Act obligations for high-risk employment AI are statutory and mandatory: conformity assessment, technical documentation, EU database registration, human oversight plan. More prescriptive than UK approach. |
| **Key divergence for Scout** | UK requires no mandatory conformity assessment, CE marking, or EU AI database registration. Compliance is enforced primarily through ICO data protection powers and EHRC equality enforcement. | If Sable AI Ltd [ASSUMPTION] were to process EU resident candidates or serve EU-established customers, the EU AI Act would apply in full — requiring conformity assessment and registration before placing the system on the EU market. |

---

## 5. Conclusion

Scout operates in one of the ICO's identified high-risk AI use cases — AI-assisted candidate screening — where live proactive enforcement is under way. The regulatory architecture is multi-layered: a data protection regime (UK GDPR + DPA 2018 + DUAA 2025 amendments), an equality regime (EA 2010), and an increasingly detailed body of guidance and enforcement precedent (ICO Nov 2024 report, DSIT March 2024 guide).

The critical compliance questions identified above are addressed across Stages 2–4. Stage 2 (Governance Foundation) establishes the risk classification, data flows, and governance policy. Stage 3 (Regulatory Alignment) produces the detailed obligation matrices, DPIA template, and ICO audit gap analysis. Stage 4 (Monitoring Controls) addresses ongoing bias monitoring and incident response.

---

*All references to Sable AI Ltd characteristics are built on unverified assumptions — see `ASSUMPTIONS-LOG.md`. This document does not constitute legal advice. Items flagged `[LEGAL REVIEW REQUIRED]` must be reviewed by a qualified UK data protection solicitor before operational reliance.*

*Cross-references: `L2-3.4-DPIA-Template-v1.md` (forthcoming); `L2-3.3-ICO-Audit-Gap-Analysis-v1.md` (forthcoming); `L3-4.2-Bias-Monitoring-Protocol-v1.md` (forthcoming); `L4-5.1-Data-Processing-Agreement-Template-v1.md` (forthcoming); `L4-5.2-Candidate-Transparency-Notice-v1.md` (forthcoming).*
