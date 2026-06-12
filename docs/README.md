# Sable AI — UK Recruitment AI Governance Framework

<p class="dc-sub">An open-source AI governance and operational architecture framework for UK HR/recruitment AI providers. Built for <strong>Sable AI Ltd</strong>, a fictional UK early-stage startup whose product, <strong>Scout</strong>, is an AI-powered CV screening and candidate shortlisting tool built on the Anthropic Claude API.</p>

<span class="dc-pill">UK GDPR</span><span class="dc-pill">DPA 2018</span><span class="dc-pill">Data (Use and Access) Act 2025</span><span class="dc-pill">Equality Act 2010</span><span class="dc-pill">ICO AI in Recruitment</span><span class="dc-pill">CC BY 4.0</span>

!!! warning "Speculative demonstration piece"

    This framework is an educational resource. It is **not a legal compliance
    certification** and must not be used as a substitute for qualified legal
    advice. Every assumption is flagged inline and logged; every open legal
    question is marked `[LEGAL REVIEW REQUIRED]`. Read the
    [Disclaimer](DISCLAIMER.md) before use.

[Start with Stage 1](stage-1-regulatory-orientation/STAGE1-Regulatory-Orientation-Note-v1.md){ .md-button .md-button--primary }
[Assumptions Log](ASSUMPTIONS-LOG.md){ .md-button }

---

## What This Framework Covers

| Regulatory instrument | Scope |
|---|---|
| UK GDPR (retained EU law, as amended) | Lawful basis, data minimisation, Arts. 22A–22D automated decision-making safeguards, data subject rights, DPIA |
| DPA 2018 | Schedule 1 special category conditions, accountability obligations |
| Data (Use and Access) Act 2025 | Amendments to UK GDPR automated decision-making provisions (Arts. 22A–22D, in force 5 February 2026) |
| Equality Act 2010 | Protected characteristics, indirect discrimination risk in algorithmic screening |
| ICO AI in Recruitment guidance | ICO November 2024 audit findings — live enforcement benchmark |
| DSIT Responsible AI in Recruitment Guide (March 2024) | Bias monitoring methodology, transparency obligations |

---

## Framework Structure

### Phase 1 — Governance Framework (14 documents)

<div class="grid cards" markdown>

-   :material-compass-outline: **Stage 1 — Regulatory Orientation**

    ---

    Situates Scout within UK law: every applicable instrument, a UK vs EU
    regulatory comparison, and a source registry.

    [:octicons-arrow-right-24: Orientation Note](stage-1-regulatory-orientation/STAGE1-Regulatory-Orientation-Note-v1.md)

-   :material-office-building-cog-outline: **Stage 2 — Governance Foundation**

    ---

    AI system inventory, risk classification, data flow map, governance
    policy, and a RACI for a 10–15 person team.

    [:octicons-arrow-right-24: AI System Inventory](stage-2-governance-foundation/L1-2.1-AI-System-Inventory-v1.md)

-   :material-scale-balance: **Stage 3 — Regulatory Alignment**

    ---

    UK GDPR compliance matrix, Equality Act 2010 discrimination risk map,
    ICO audit gap analysis, and a full DPIA template.

    [:octicons-arrow-right-24: UK GDPR Mapping Matrix](stage-3-regulatory-alignment/L2-3.1-UK-GDPR-Mapping-Matrix-v1.md)

-   :material-monitor-eye: **Stage 4 — Monitoring & Controls**

    ---

    Metrics-based monitoring framework (23 KPIs), bias monitoring protocol
    (adverse impact ratio, four-fifths rule, demographic parity testing),
    and incident response plan.

    [:octicons-arrow-right-24: Monitoring Framework](stage-4-monitoring-controls/L3-4.1-Monitoring-Framework-v1.md)

-   :material-handshake-outline: **Stage 5 — Commercial Packaging**

    ---

    Data Processing Agreement templates (agency customer with Art. 26 joint
    controller provisions + in-house HR customer), and a candidate
    transparency notice in plain English.

    [:octicons-arrow-right-24: DPA Template](stage-5-commercial-packaging/L4-5.1-Data-Processing-Agreement-Template-v1.md)

-   :material-test-tube: **Phase 2 — Scout Worked Examples**

    ---

    The Phase 1 templates populated with Scout's confirmed architecture:
    Python/FastAPI, pdfplumber/python-docx CV parsing, Anthropic Claude API,
    PostgreSQL on AWS RDS, S3, React TypeScript frontend.

    [:octicons-arrow-right-24: Scout System Profile](phase-2-worked-examples/P2-Scout-System-Profile-v1.md)

</div>

---

## Document Index

| Document | Stage |
|---|---|
| [Regulatory Orientation Note](stage-1-regulatory-orientation/STAGE1-Regulatory-Orientation-Note-v1.md) | Stage 1 |
| [Regulatory Source Summary](stage-1-regulatory-orientation/STAGE1-Regulatory-Source-Summary-v1.md) | Stage 1 |
| [AI System Inventory](stage-2-governance-foundation/L1-2.1-AI-System-Inventory-v1.md) | Stage 2 |
| [Risk Classification Framework](stage-2-governance-foundation/L1-2.2-Risk-Classification-Framework-v1.md) | Stage 2 |
| [Data Flow Map](stage-2-governance-foundation/L1-2.3-Data-Flow-Map-v1.md) | Stage 2 |
| [Governance Policy](stage-2-governance-foundation/L1-2.4-Governance-Policy-v1.md) | Stage 2 |
| [Roles and Responsibilities](stage-2-governance-foundation/L1-2.5-Roles-and-Responsibilities-v1.md) | Stage 2 |
| [UK GDPR Mapping Matrix](stage-3-regulatory-alignment/L2-3.1-UK-GDPR-Mapping-Matrix-v1.md) | Stage 3 |
| [Equality Act 2010 Compliance Map](stage-3-regulatory-alignment/L2-3.2-Equality-Act-2010-Compliance-Map-v1.md) | Stage 3 |
| [ICO Audit Gap Analysis](stage-3-regulatory-alignment/L2-3.3-ICO-Audit-Gap-Analysis-v1.md) | Stage 3 |
| [DPIA Template](stage-3-regulatory-alignment/L2-3.4-DPIA-Template-v1.md) | Stage 3 |
| [Monitoring Framework](stage-4-monitoring-controls/L3-4.1-Monitoring-Framework-v1.md) | Stage 4 |
| [Bias Monitoring Protocol](stage-4-monitoring-controls/L3-4.2-Bias-Monitoring-Protocol-v1.md) | Stage 4 |
| [Incident Response Plan](stage-4-monitoring-controls/L3-4.3-Incident-Response-Plan-v1.md) | Stage 4 |
| [Data Processing Agreement Template](stage-5-commercial-packaging/L4-5.1-Data-Processing-Agreement-Template-v1.md) | Stage 5 |
| [Candidate Transparency Notice](stage-5-commercial-packaging/L4-5.2-Candidate-Transparency-Notice-v1.md) | Stage 5 |
| [Scout System Profile](phase-2-worked-examples/P2-Scout-System-Profile-v1.md) | Phase 2 |
| [Scout UK GDPR Mapping](phase-2-worked-examples/P2-Scout-UK-GDPR-Mapping-v1.md) | Phase 2 |
| [Scout ICO Audit Gap Analysis](phase-2-worked-examples/P2-Scout-ICO-Audit-Gap-Analysis-v1.md) | Phase 2 |
| [Scout Bias Monitoring Protocol](phase-2-worked-examples/P2-Scout-Bias-Monitoring-Protocol-v1.md) | Phase 2 |

---

## Who This Is For

- **UK recruitment AI providers** building compliance frameworks for ICO review
- **In-house HR teams** evaluating AI screening tools against UK GDPR and Equality Act obligations
- **Compliance and legal professionals** needing a structured starting point for AI governance documentation
- **Developers and founders** at early-stage AI startups who need to understand their regulatory obligations before scaling

---

## Limitations and Legal Disclaimer

All documents are built on assumed characteristics of Sable AI Ltd — a fictional company. Every assumption is flagged with `[ASSUMPTION]` inline and logged in [ASSUMPTIONS-LOG.md](ASSUMPTIONS-LOG.md).

Every document requiring qualified legal review is flagged `[LEGAL REVIEW REQUIRED]`. These flags are not decoration — they mark genuine legal questions that cannot be resolved by a framework document and require a qualified UK lawyer.

See the full [Disclaimer](DISCLAIMER.md).

---

## Licence

[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

You are free to use, adapt, and redistribute this framework with attribution. If you adapt it for commercial use, retain the disclaimer and assumption flags.

---

## Related

- [Pickles GmbH AI Governance Framework](https://github.com/davidcockson-compliance/pickles-gmbh-ai-governance-framework) — EU/German jurisdiction equivalent (EU AI Act, GDPR, BDSG)
- Built by [David Cockson](https://davidcockson.com)
