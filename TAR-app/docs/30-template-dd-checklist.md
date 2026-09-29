---
title: Token Due Diligence Checklist Template
category: iii     # Templates + reporting
classification: private
owner: Head of Listing
last_reviewed: 2026-09-29
---

# 30 — Token Due Diligence Checklist Template

> **Purpose.** Working DD record produced for every admission application. Anchored to HKbitEX TAP §3 (DD criteria) and HKbitEX LR 2.2 (admission criteria). Any framework not present in HKbitEX's approved policies is flagged in §11 gap addendum.

---

## How to use this template

1. **Copy** to a new file named `DD-{TOK}-{YYYY-MM-DD}.md`
2. **Assign** to the four team leads: Compliance, R&D, Legal, Risk, Operations
3. **Each section** signed off by its owner; status = Verified / Pending / Rejected
4. **Submit** to Head of Listing for the DD stage of admission workflow
5. **Archive** in versioned case file (7 years)

---

# DD-{TOK}-{YYYY-MM-DD}

| Field | Value |
|---|---|
| **Token** | |
| **Symbol** | |
| **Application Reference** | e.g., TA-2026-NNN |
| **Issuer** | |
| **Review Date** | |
| **Review Team** | Pre-Listing DD Team |
| **Status** | COMPLETED / IN PROGRESS / ON HOLD |
| **Proposed Access Tier** | PI only (per HKbitEX TAP §4.1.3) |
| **Smart Contract Audit Status** | Not Required / Required / Completed / N/A |

---

## SECTION 01 — Token Basic Information

| # | Item | Information | Verification Source | Status |
|---|---|---|---|---|
| 1.1 | Project Name | | Official website (URL): https://... | |
| 1.2 | Token Symbol | | Multiple blockchain explorers | |
| 1.3 | Official Website | | Live check; SSL valid | |
| 1.4 | Whitepaper Link | | Project-controlled | |
| 1.5 | Source Code Link | | Official repo (e.g., GitHub) | |
| 1.6 | Total Supply | | Etherscan / official dashboard | |
| 1.7 | Maximum Supply | | Whitepaper / tokenomics | |
| 1.8 | Circulating Supply | | Aggregator + issuer | |
| 1.9 | Burned Token Amount | | On-chain analysis | |
| 1.10 | Token Decimals | | Contract verification | |
| 1.11 | Token Deployment Date | | Historical deployment tx | |
| 1.12 | Consensus Mechanism | | Underlying-chain docs | |
| 1.13 | Contract Address(es) | | Official project documentation | |
| 1.14 | Deployer Address | | Deployment tx | |
| 1.15 | Contract Owner Address | | On-chain verification | |
| 1.16 | Blockchain Explorer(s) | | URLs | |
| 1.17 | Project Type | | Per HKbitEX taxonomy | |
| 1.18 | Social Media Channels | | Twitter, GitHub, Discord, LinkedIn, Telegram | |
| 1.19 | Listed Exchanges | | Aggregator listings | |
| 1.20 | Target Customers | | Marketing materials | |
| 1.21 | Project Introduction | | Whitepaper; official documentation | |

**Reviewer:** Operations Team — Signature, Date

---

## SECTION 02 — Project Incorporation Information

| # | Item | Information | Verification Source | Status |
|---|---|---|---|---|
| 2.1 | Issuer Full Legal Name | | Jurisdiction corporate registry | |
| 2.2 | Jurisdiction / Registration | | | |
| 2.3 | Company Registration Number | | | |
| 2.4 | Incorporation Date | | | |
| 2.5 | Affiliated Companies | | Group structure | |
| 2.6 | Equity Structure | | Cap table | |
| 2.7 | Organizational Structure | | Org chart | |
| 2.8 | Ultimate Beneficial Owner Info | | CDD documentation | |
| 2.9 | UBO Undertaking Letters | | Signed by UBO | |
| 2.10 | Core Team Members | | Background checks | |
| 2.11 | Investors | | Public announcements | |
| 2.12 | Advisors | | | |
| 2.13 | Information Completeness | | All requested docs provided + verified | |
| 2.14 | Directors / Executives Background | | Background checks; no criminal/regulatory findings | |
| 2.15 | Primary Contact | | | |
| 2.16 | Primary Phone | | | |
| 2.17 | Primary Email | | | |

**Reviewer:** Compliance Team — Signature, Date

---

## SECTION 03 — Security Risk

| # | Item | Assessment | Findings | Status |
|---|---|---|---|---|
| 3.1 | Unauthorized Token Minting Risk | LOW/MED/HIGH | Multi-sig required; role-based access | |
| 3.2 | Third-Party Smart-Contract Audit | YES (multiple) / NO / N/A | Auditors; report URLs | |
| 3.3 | Contract Ownership Status | Retained / Renounced / N/A | Owner address; multi-sig | |
| 3.4 | Hidden Owner Existence | YES / NO | Full contract analysis | |
| 3.5 | Code Publicly Accessible | YES / NO | GitHub | |
| 3.6 | Blacklist / Sanctions Function | YES / NO / N/A | Implementation; control | |
| 3.7 | Fake Deposit Risk | LOW/MED/HIGH | Transfer events; integration test | |
| 3.8 | Governance Protection Mechanism | Multi-sig + Timelock / other | Details | |
| 3.9 | Token Recovery | YES / NO | Recovery function; restrictions | |
| 3.10 | Self-Destruct Function | YES / NO | Verification | |
| 3.11 | Owner Token Holdings | % of supply | On-chain | |
| 3.12 | Top Holder Concentration | % of supply | Etherscan / Nansen | |
| 3.13 | On-Chain Lockup / Unlock | | Reconciliation with off-chain | |
| 3.14 | Honeypot Risk | YES / NO | Standard ERC-20? | |
| 3.15 | Smart Contract Code Public | YES / NO | (mirror of 3.5) | |
| 3.16 | Smart Contract Security Issues | None critical / minor / major | Audit findings | |
| 3.17 | On-Chain Reconciliation | | Automated/manual | |
| 3.18 | Penetration Testing | | Frequency; firm | |
| 3.19 | API Security / Encryption | | TLS version; cryptography standards | |
| 3.20 | 51% Attack Protection | | Underlying chain | |
| 3.21 | DDoS Protection + IR | | SOC; documented plan | |
| 3.22 | Historical Hacks / Incidents | | None / minor / major | |

**Reviewer:** R&D Team — Signature, Date

---

## SECTION 04 — Legal Risk

| # | Item | Assessment | Findings | Status |
|---|---|---|---|---|
| 4.1 | Not SFC "Securities" | YES / NO | HK legal opinion | |
| 4.2 | Subject to SFO / Other HK Regs | | HK legal opinion | |
| 4.3 | Regulatory Warnings / Bans | | None / advisory / penalty | |
| 4.4 | Issuer Legal Status Clear & Active | YES / NO | Good-standing certificates | |
| 4.5 | Unlicensed Financial Products | YES / NO / N/A | Issuer license mapping | |
| 4.6 | Other Jurisdictional Requirements | | Compliance mapping | |
| 4.7 | Legal Opinion Provided | YES / NO | Counsel name; date; scope | |
| 4.8 | Core Member Undertakings | YES / NO | Signed | |
| 4.9 | IP / Copyright Disputes | YES / NO | | |
| 4.10 | Token Issuance & Sales Compliance | | KYC/AML on subscribers; no public ICO | |
| 4.11 | Major Litigation / Arbitration | | Class action; regulatory inquiry | |
| 4.12 | Illegal Fundraising / Disguised | YES / NO | | |
| 4.13 | Legal & Regulatory Risk Disclosure | | Adequate | |
| 4.14 | Issuance Document Consistency | | Whitepaper, terms, opinions consistent | |
| 4.15 | False Advertising / Community Hype | | None / misleading | |
| 4.16 | Past Project Failures / Controversies | | None / disclosed | |

**Reviewer:** Legal Team — Signature, Date

---

## SECTION 05 — Compliance Risk

| # | Item | Assessment | Findings | Status |
|---|---|---|---|---|
| 5.1 | CDD Verification (Company, Controllers, UBOs) | YES / NO | Full KYC | |
| 5.2 | High-Risk Jurisdictions | YES / NO | Operating jurisdictions | |
| 5.3 | PEP / Sanctions Screening | YES / NO MATCHES | WorldCheck; OFAC; UN; EU | |
| 5.4 | Nominee / Proxy Issuance | YES / NO | Direct issuance | |
| 5.5 | Historical Violations / Sanctions | YES / NO | | |
| 5.6 | Issuer Cooperation with Compliance | Excellent / Good / Poor | | |
| 5.7 | Offshore Complex Structures | | Disclosed; legitimate purpose | |
| 5.8 | Anonymous Team / Investors | YES / NO | All identified | |
| 5.9 | Token Issued by Legal Entity | YES / NO | | |
| 5.10 | OFAC / FATF / Interpol Blacklisting | YES / NO | | |
| 5.11 | Anonymity / Privacy Features | YES / NO | zk-SNARKs, ring sigs | |
| 5.12 | High ML/TF Risk Marking | LOW/MED/HIGH | Chainalysis score | |
| 5.13 | Chain Analysis Compatibility | YES / NO | | |
| 5.14 | High-Risk Industry Usage | | Gambling / darknet exposure | |
| 5.15 | Contract Interaction with Illicit Addresses | | Blacklist function active | |
| 5.16 | Historical Market Manipulation | YES / NO | | |

**Reviewer:** Compliance Team — Signature, Date

---

## SECTION 06 — Governance & Market Maturity

| # | Item | Assessment | Findings | Status |
|---|---|---|---|---|
| 6.1 | Governance Entity | Corporate / Foundation / DAO | | |
| 6.2 | Governance Model Public | YES / NO | URL | |
| 6.3 | Governance Roles & Responsibilities Clear | YES / NO | | |
| 6.4 | Governance Concentration | Moderate / Distributed | | |
| 6.5 | Governance Proposal / Decision Process Transparent | YES / NO | | |
| 6.6 | >12 Months Operation | YES / NO | Required for retail under HKbitEX status; not currently required for PI admission but should be documented | |
| 6.7 | Governance Decision Traceability | YES / NO | | |
| 6.8 | Governance Disputes / Forks / Conflicts | YES / NO | | |
| 6.9 | Trading Depth & Liquidity | Excellent / Good / Adequate / Poor | | |
| 6.10 | Compliant VASP / VATP Support | | # exchanges / VASPs | |
| 6.11 | 12-Month Trading Volume Trend | | Monthly volume range | |
| 6.12 | Market / Technical Disruptions | | Past incidents | |
| 6.13 | GitHub Code Updates | | Frequency | |
| 6.14 | Previous Delistings | YES / NO | | |
| 6.15 | Reasonable Bid-Ask Spread | YES / NO | | |
| 6.16 | 30-Day Volatility vs Peers | | | |
| 6.17 | Community Engagement | Active / Moderate / Low | | |
| 6.18 | Social Media Activity & Media Coverage | | | |
| 6.19 | Price Chart Health | | Manipulation patterns | |

**Reviewer:** Risk Management Team — Signature, Date

---

## STABLECOIN SUPPLEMENTARY CHECKLIST (if applicable)

| # | Item | Assessment | Findings | Status |
|---|---|---|---|---|
| SC.1 | Underlying Asset Disclosure | | Composition by % | |
| SC.2 | Independent Attestation / Audit | | Frequency; auditor | |
| SC.3 | Redemption Policy & Liquidity | | 1:1 par; T+? | |
| SC.4 | Legal Opinion (Non-Security) | | Counsel | |
| SC.5 | Unauthorized Financial Activities | | None | |
| SC.6 | Reserve Composition Detail | | Cash % / Treasuries % / other | |
| SC.7 | Reserve Custody | | Segregated at qualified custodian | |
| SC.8 | Algorithmic / Non-Collateralized | | Must be fully collateralized | |

**Reviewer:** Compliance Team — Signature, Date

---

## HKbitEX-Specific Quantitative Requirements

Per HKbitEX LR 5.2(e):

| Check | Required value | Verified |
|---|---|---|
| Market capitalisation at time of listing | ≥ HK$8,000,000 | |
| Transferability | Via blockchain technology, subject to regulatory requirements | |
| Encumbrance | None | |
| Issuer authorisation | Validly authorised by Issuer under constitutional documents | |
| Issuer issuance | Validly issued under applicable law | |

Per HKbitEX TAP Appendix C (BD-sponsorship minimum requirements):

| Check | Required value | Verified |
|---|---|---|
| Market capitalisation | ≥ HKD 8 million or equivalent | |
| Team pseudonymity | Not pseudonymous (except BTC) | |
| Token type exclusions | Not DeFi / Governance / Algorithmic stablecoin | |

---

## OVERALL ASSESSMENT

### Final Recommendation

| Outcome | Tick |
|---|---|
| **APPROVE** for PI Client (per TAP §4.1.3) | |
| APPROVE WITH CONDITIONS (specify below) | |
| DEFER pending additional information | |
| REJECT | |

### Recommendation Rationale

[Brief rationale based on factors and any material findings]

### Conditions (if applicable)

| # | Condition | Owner | Deadline |
|---|---|---|---|
| 1 | | | |
| 2 | | | |

### Key Strengths

- ✅ [Strength 1]
- ✅ [Strength 2]

### Areas for Ongoing Monitoring

- 🔄 [Area 1]
- 🔄 [Area 2]

### Checklist Completion

| Section | Status |
|---|---|
| 01 Token Basic Information | 100% / partial |
| 02 Project Incorporation | 100% / partial |
| 03 Security Risk | 100% / partial |
| 04 Legal Risk | 100% / partial |
| 05 Compliance Risk | 100% / partial |
| 06 Governance & Market Maturity | 100% / partial |
| Stablecoin Supplement | 100% / partial / N/A |
| HKbitEX-Specific Quantitative Requirements | 100% / partial |
| Critical Issues Identified | None / [list] |

---

## APPROVALS

| Role | Name | Signature | Date |
|---|---|---|---|
| DD Lead (Operations) | | | |
| Compliance Team Lead | | | |
| Risk Management Team Lead | | | |
| R&D Team Lead | | | |
| Legal Team Lead | | | |
| **Head of Listing** | | | |
| **TARC Chairperson** (if escalated) | | | |

---

## APPENDIX A — Documents Verified

- [ ] Issuer Certificate of Incorporation
- [ ] License copies (all jurisdictions)
- [ ] Legal opinion (HK; additional jurisdictions if relevant)
- [ ] Smart contract audit reports
- [ ] Monthly attestations (last 12 months, if applicable)
- [ ] Team identification + background checks
- [ ] AML/CFT policy documentation
- [ ] Terms of service + risk disclosures
- [ ] GitHub repository access
- [ ] Corporate structure diagram
- [ ] UBO undertaking letters
- [ ] Project whitepaper

**Documentation Custodian:** Listing Department
**Archival Period:** 7 years minimum
**Storage:** Tamper-evident per `42-ai-agent-document-versioning.md`

---

## 11. HKbitEX policy gaps (for TAR-app gap-analysis feature)

| Gap ID | Reference | Framework | HKbitEX source status | Action required |
|---|---|---|---|---|
| GAP-DDC-001 | §"QUANTITATIVE SCORING" section removed | 4-pillar weighted scoring with 1–5 scale, sub-scores per section, weighted overall, threshold-based decision (Approve / Conditional / Defer / Reject), Critical-section gate | Not in HKbitEX docs | Manager review |
| GAP-DDC-002 | Header "Proposed Access Tier" | Currently HKbitEX only admits for PI (TAP §4.1.3); template hardcoded accordingly | Matches HKbitEX status | No action (consistent) |
| GAP-DDC-003 | Stablecoin supplementary checklist | Items SC.1–SC.8 are not in HKbitEX docs | Not in HKbitEX docs | Manager review |

---

*End of template. To convert to working format: replace all `{...}` placeholders, fill every row, obtain all signatures.*
