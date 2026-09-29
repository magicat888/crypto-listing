---
title: Legal & Regulatory Framework (HK SFC)
category: i       # SFC + Laws
classification: public
owner: Head of Listing
last_reviewed: 2026-09-29
---

# 00 — Legal & Regulatory Framework (HK SFC)

> **Purpose.** Single source of truth for what the Hong Kong Securities and Futures Commission (SFC) requires of a licensed Virtual Asset Trading Platform (VATP) for token admission, ongoing trading, suspension, delisting, and client-facing disclosure. All HKbitEX policies in `TAR-app/docs/` must reconcile to this backbone. When in doubt, SFC text wins.
>
> **Binding instruments.**
> - *Securities and Futures Ordinance* (Cap. 571) — **SFO**
> - *Anti-Money Laundering and Counter-Terrorist Financing Ordinance* (Cap. 615) — **AMLO**
> - *Companies (Winding Up and Miscellaneous Provisions) Ordinance* (Cap. 32) — **C(WUMP)O**
> - *Stablecoins Ordinance* (Cap. 656) — where stablecoins are in scope
> - *Guidelines for Virtual Asset Trading Platform Operators* (SFC, June 2023) — **VATP Guidelines**
> - *Circular on expansion of products and services of virtual asset trading platforms* (SFC, 3 November 2025) — **2025 Expansion Circular**
> - *Guideline on Anti-Money Laundering and Counter-Financing of Terrorism* (for licensed corporations and SFC-licensed VASPs) — **AML Guideline**
>
> **Citation convention.** `[CIT-…]` references in this document point to verbatim text in `99-citations.md` (when present) or directly to the SFC / HKbitEX source document otherwise.

---

## 1. What kind of entity we are

A licensed VATP is, under [CIT-SFC-C-004], either:

- **(a)** a corporation licensed for **Type 1** (dealing in securities) **and Type 7** (providing automated trading services) under section 116 of the SFO; **or**
- **(b)** a corporation licensed to provide a VA service under section 53ZRK of the AMLO.

HKbitEX is *deemed-to-be-licensed* for Type 1 + Type 7 (per the HKbitEX Listing Rules preamble), which means it sits inside category (a) and is bound by the VATP Guidelines even though it is excluded from the 2025 Expansion Circular's "VATPs" definition. **For our purposes, we treat the VATP Guidelines as fully binding regardless of deemed-licence status.**

Our **Associated Entity** (the custodian) is Hong Kong Digital Asset Custody Co. Limited, incorporated in HK, wholly owned by us, holding a TCSP licence under the AMLO [CIT-HKX-TAP-001].

## 2. Mandatory governance structure: the Token Admission and Review Committee

The SFC requires that every VATP set up a committee responsible for token admission and review. The committee must [CIT-SFC-G-001]:

| Function | Required by |
|---|---|
| (a) Set, implement and enforce admission criteria | VATP Guidelines §7.1(a) |
| (b) Set, implement and enforce suspension/withdrawal criteria | §7.1(b) |
| (c) Make final decisions on admit/suspend/withdraw | §7.1(c) |
| (d) Set issuer obligations/restrictions | §7.1(d) |
| (e) Review criteria + admitted tokens **at least annually** | §7.1(e) |

Composition: at minimum, senior-management members "principally responsible for managing the key business line, compliance, risk management and information technology" [CIT-SFC-G-002]. HKbitEX implements this with 9 named seats: CEO, CBO, CTO, CIO, Technical Director, Head of Exchange, Head of Risk Management, Head of Compliance, Head of Listing [CIT-HKX-TAP-007, CIT-HKX-TAP-013].

**Operational discipline required by SFC:**

- Decisions and reasons **must be documented** [CIT-SFC-G-003].
- Committee must **report to the board monthly** and **promptly escalate critical matters** (suspension, delisting) [CIT-SFC-G-004].
- Admission/suspension/withdrawal criteria must be **published on the website** [CIT-SFC-G-005, CIT-SFC-G-018].

## 3. The admission gate: mandatory DD factors

The SFC mandates a **non-exhaustive** list of factors the committee must consider [CIT-SFC-G-006]:

1. Background of management/development team and known key members
2. Regulatory status in HK and impact on the Platform Operator's obligations
3. Supply, demand, maturity, liquidity — **including track record ≥12 months** for any VA other than a security token
4. Technical aspects
5. Development
6. Market and governance risks
7. Legal risks (VA and issuer)
8. Fraud/illegal indicators; "Ponzi-by-continuous-inflow" red flag
9. Enforceability of extrinsic rights and impact on underlying markets
10. ML/TF risks

HKbitEX's LR 2.2 and TAP §3.2 expand this to **18 enumerated factors** (see `01-listing-rules-summary.md`). The HKbitEX list is consistent with the SFC list and adds:
- accuracy of marketing materials
- reputational risk
- risk profile
- economic factors (dilution/inflation, rewards/penalties, venue incentives)
- allocation of tokens (sale, presale, founders, advisors, employees, foundation)
- relationship between issuer and VA (proprietary trading, market making)
- affiliation with the Platform
- government connection / sponsorship

**Important change from 2025 Circular** [CIT-SFC-C-001]: The 12-month track record requirement **no longer applies to professional-investor offerings** (including stablecoins) for fully-licensed VATPs. It still applies to retail offerings of non-stablecoins. It never applies to tokenised securities [CIT-SFC-C-003].

> **Catch.** Even when the track-record rule is lifted, the SFC **reiterates** that "all reasonable due diligence" must still be performed, and that "adequate disclosures" must be made when a VA offered to PIs has <12 months' track record [CIT-SFC-C-002].

> **HKbitEX status flag.** HKbitEX is **deemed-to-be-licensed** and therefore **outside the 2025 Circular's "VATPs" definition** [CIT-SFC-C-004]. Until HKbitEX's licensing conditions are varied, the 12-month track-record rule continues to apply to its PI offerings per VATP Guidelines §7.6(c).

## 4. The retail gate: layered on top

For retail-client availability of any VA, two additional gates apply [CIT-SFC-G-007]:

- **(a)** Must NOT fall within "securities" under the SFO, unless offering complies with C(WUMP)O prospectus requirements AND Part IV SFO restrictions.
- **(b)** Must be **highly liquid**.

Liquidity is tested by **Eligible Large-Cap Virtual Asset** status [CIT-SFC-G-008]: included in at least **two acceptable indices** issued by **two different index providers**, where:

- Index is investible, rules-based, with documented methodology.
- Providers are independent of each other, the issuer, and the Platform Operator.
- At least one provider complies with **IOSCO Principles for Financial Benchmarks** and has conventional-securities index experience.

**Process.** The Platform may submit a case-by-case proposal to the SFC for a VA that meets all other criteria but fails the index test [CIT-SFC-G-008, Note 3].

**Ongoing obligation.** If an admitted retail VA **falls out** of an acceptable index, automatic suspension is *not* required — but the Platform must evaluate whether to keep offering it to retail; if issues are unlikely to resolve near-term, the Platform should consider suspension or selling-only restriction [CIT-SFC-G-011, Note].

## 5. Pre-trading infrastructure requirements

| Gate | Citation |
|---|---|
| Internal controls, AML monitoring, market-surveillance tools commensurate with VA-specific risk | [CIT-SFC-G-009] |
| Smart-contract audit by independent assessor (or reliance on third-party audit demonstrated reasonable) | [CIT-SFC-G-010] |
| Prospectus / CIS / structured-product restrictions on offer of investments | [CIT-SFC-G-013] |
| Implementation of access controls preventing securities-offering-style breaches | [CIT-SFC-G-013, §7.14] |

## 6. Ongoing monitoring and legal-status tracking

Required by [CIT-SFC-G-011]:

- Continuous monitoring of each admitted VA.
- **Regular review reports** submitted to the token admission and review committee.
- Where the committee decides to suspend/withdraw: notify clients as soon as practicable, give rationale, inform of options, ensure fair treatment.

[ CIT-SFC-G-012 ] adds a **status-change tracker**: monitor for changes that may push a VA into (or out of) the SFO "securities" definition. **If a retail-offered VA becomes a security, retail trading must cease.**

## 7. Client-facing obligations

For non-institutional, non-qualified-corporate-PI clients [CIT-SFC-G-014, CIT-SFC-G-015, CIT-SFC-G-016]:

- Assess VA knowledge before account opening (or train the investor).
- Risk-profile and suitability assessment.
- Per-client exposure limit tied to net worth and personal circumstances.

Jurisdictional access controls [CIT-SFC-G-017]: prevent marketing and access from jurisdictions that ban VA trading; detect and prevent VPN-based circumvention.

**Website disclosure (minimum)** [CIT-SFC-G-018]:

1. **Trading and operational rules**, including **admit/suspend/withdraw criteria**.
2. **Admission and trading fees**, with illustrative worked examples.
3. **Per-VA information** enabling clients to appraise their position.

**Per-VA disclosure (minimum)** [CIT-SFC-G-019]:

- Price & volume (24h, since-listing)
- Mgmt/dev team background
- Issuance date
- Material terms & features
- Platform–issuer affiliation
- Official website & whitepaper link
- Smart-contract audit / bug report link
- Voting-rights handling

## 8. Mandatory risk disclosure wording

Schedule 2 of the VATP Guidelines requires that clients receive the **exact** wording in [CIT-SFC-G-020] — twelve risk statements covering VA risk, legal-status uncertainty, absence of regulatory scrutiny, ICF non-coverage, lack of legal-tender status, irreversibility, value-at-risk if market disappears, volatility, regulatory change, timing of execution, fraud/cyber risk, and access risk.

> **Implementation rule.** Any platform-facing document (client agreement, marketing, listing document) that references risks must reproduce the Schedule 2 statements verbatim.

## 9. What the 2025 Expansion Circular changed

The 3 November 2025 SFC Circular [CIT-SFC-C-001..005] made the following changes effective for **fully-licensed VATPs**:

| Topic | Before | After (3 Nov 2025) for fully-licensed VATPs |
|---|---|---|
| 12-month track record for PI offerings | Required | **Lifted** |
| 12-month track record for retail (non-stablecoin) | Required | **Still required** |
| Stablecoin (HKMA-licensed issuer) — retail | Required | **Permitted without 12-month rule** |
| Stablecoin (other) — PI | Required | **Lifted** |
| Tokenised securities / digital securities | 12-month rule applied | **Never applies** |
| DD obligation when track record lifted | n/a | **Continues — all-reasonable-DD plus disclosure** |
| Custody of non-traded digital assets via associated entity | Not permitted | **Permitted with licensing-condition modification** |
| Distribution of digital-asset-related products + tokenised securities | Not explicit | **Explicitly permitted (modified licence conditions)** |

**HKbitEX is a deemed-to-be-licensed VATP**, **excluded** from this circular's scope [CIT-SFC-C-004]. Until HKbitEX's licensing conditions are varied, the 12-month track-record rule **still applies** to HKbitEX's PI offerings as written in VATP Guidelines §7.6(c).

## 10. SFC §7.6 DD factors — full enumerated list (verbatim from source)

For HKbitEX operational reference, the full non-exhaustive list of factors per VATP Guidelines §7.6 [CIT-SFC-G-006]:

| § | Factor | Operator must consider, where applicable |
|---|---|---|
| (a) | Background of management / development team | The background of the management or development team of a virtual asset or any of its known key members (if any) |
| (b) | Regulatory status in HK | The regulatory status of a virtual asset in Hong Kong and whether its regulatory status would also affect the regulatory obligations of the Platform Operator |
| (c) | Track record | The supply, demand, maturity and liquidity of a virtual asset, including its track record, where the virtual asset (except for a security token) should be issued for at least 12 months |
| (d) | Technical aspects | The technical aspects of a virtual asset |
| (e) | Development | The development of a virtual asset |
| (f) | Market and governance risks | The market and governance risks of a virtual asset |
| (g) | Legal risks | The legal risks associated with the virtual asset and its issuer (where applicable) |
| (h) | Fraud / illegal indicators | Whether the utility offered, the novel use cases facilitated, technical, structural or cryptoeconomic innovation, or the administrative control exhibited by the virtual asset clearly appears to be fraudulent or illegal, or whether the continued viability of the virtual asset depends on attracting continuous inflow into the virtual asset |
| (i) | Extrinsic rights | The enforceability of any rights extrinsic to the virtual asset (for example, rights to any underlying assets) and the potential impact of the virtual asset's trading activity on the underlying markets |
| (j) | ML/TF risks | The money laundering and terrorist financing risks associated with the virtual asset |

## 11. SFC Schedule 2 — verbatim mandatory risk disclosures

These 12 statements must appear verbatim in any platform-facing document that references VA risks [CIT-SFC-G-020]:

| # | Risk category | Wording (verbatim) |
|---|---|---|
| (a) | VA risk | "virtual assets are highly risky and investors should exercise caution in relation to the products;" |
| (b) | Legal-property status | "a virtual asset may or may not be considered 'property' under the law, and such legal uncertainty may affect the nature and enforceability of a client's interest in such a virtual asset;" |
| (c) | No regulatory scrutiny | "the offering documents or product information provided by the issuer have not been subject to scrutiny by any regulatory body;" |
| (d) | ICF non-coverage | "the protection offered by the Investor Compensation Fund does not apply to transactions involving virtual assets (irrespective of the nature of the tokens);" |
| (e) | Not legal tender | "a virtual asset is not a legal tender, ie, it is not backed by the government and authorities;" |
| (f) | Irreversibility | "transactions in virtual assets may be irreversible, and, accordingly, losses due to fraudulent or accidental transactions may not be recoverable;" |
| (g) | Market-disappearance value-at-risk | "the value of a virtual asset may be derived from the continued willingness of market participants to exchange fiat currency for a virtual asset, which means that the value of a particular virtual asset may be completely and permanently lost should the market for that virtual asset disappear. There is no assurance that a person who accepts a virtual asset as payment today will continue to do so in the future;" |
| (h) | Volatility | "the extreme volatility and unpredictability of the price of a virtual asset relative to fiat currencies may result in a total loss of the investment over a short period of time;" |
| (i) | Legislative/regulatory change | "legislative and regulatory changes may adversely affect the use, transfer, exchange and value of virtual assets;" |
| (j) | Execution timing | "some virtual asset transactions may be deemed to be executed only when recorded and confirmed by the Platform Operator, which may not necessarily be the time at which the client initiates the transaction;" |
| (k) | Fraud / cyberattack | "the nature of virtual assets exposes them to an increased risk of fraud or cyberattack; and" |
| (l) | Platform access | "the nature of virtual assets means that any technological difficulties experienced by the Platform Operator may prevent clients from accessing their virtual assets." |

## 12. SFC §7.10 Smart-contract audit requirement (verbatim)

> "Before admitting any virtual assets for trading, a Platform Operator should exercise due skill, care and diligence in selecting and appointing an independent assessor to conduct a smart contract audit for smart-contract based virtual assets, unless the Platform Operator demonstrates that it would be reasonable to rely on a smart contract audit conducted by an independent assessor engaged by a third party. The smart contract audit should focus on reviewing that the smart contract is not subject to any contract vulnerabilities or security flaws to a high level of confidence."
— *SFC VATP Guidelines, §7.10*

## 13. SFC Eligible Large-Cap Virtual Asset — index-based liquidity test (§7.8)

The Eligible Large-Cap Virtual Asset status [CIT-SFC-G-008] requires:

| Requirement | Detail |
|---|---|
| Index count | ≥ 2 acceptable indices |
| Provider count | ≥ 2 different providers |
| Provider independence | Providers are separate from each other, the issuer, and the Platform Operator |
| Provider quality | At least one provider complies with **IOSCO Principles for Financial Benchmarks** and has conventional-securities index experience |
| Index quality (Note 1) | (a) Investible — constituents sufficiently liquid; (b) objectively calculated and rules-based; (c) provider has expertise and technical resources; (d) methodology well documented, consistent, transparent |

## 14. SFC §9.27 website disclosure — minimum content

Per [CIT-SFC-G-018], the website must include at minimum:

| § | Disclosure |
|---|---|
| (a) | Trading and operational rules + **token admission and removal rules and criteria** |
| (b) | **Admission and trading fees and charges**, with illustrative examples |
| (c) | **Relevant information for each VA admitted for trading** to enable clients to appraise their investments |

## 15. SFC §9.28 per-VA disclosure — minimum content

Per [CIT-SFC-G-019], for each admitted VA, the website must include:

| § | Disclosure |
|---|---|
| 1 | Price and trading volume (24h and since-listing) |
| 2 | Background of management / development team |
| 3 | Issuance date |
| 4 | Material terms and features |
| 5 | Platform–Issuer affiliation |
| 6 | Official website + Whitepaper link |
| 7 | Smart-contract audit + other bug report link |
| 8 | Voting-rights handling (where applicable) |

## 16. SFC VATP Guidelines — Chapter VII (Operations) structure

| § | Title | Purpose |
|---|---|---|
| 7.1 | Token admission and review committee | Functions (a)–(e) |
| 7.2 | Transparent criteria | Must disclose admission/suspension/withdrawal criteria |
| 7.3 | Committee composition | Senior management: business / compliance / risk / IT |
| 7.4 | Documentation | Decisions + reasons |
| 7.5 | Reporting to board | Monthly; escalate critical matters |
| 7.6 | Due diligence on virtual assets | 10-factor non-exhaustive list |
| 7.7 | Retail-specific | Non-securities + high liquidity |
| 7.8 | Eligible Large-Cap | ≥2 acceptable indices |
| 7.9 | Internal controls | Commensurate with VA risk |
| 7.10 | Smart-contract audit | Independent assessor |
| 7.11 | Ongoing monitoring | + retail-specific consideration |
| 7.12 | Status-change tracking | Cease retail if becomes security |
| 7.13 | Offering requirements | C(WUMP)O + Part IV SFO + CIS |
| 7.14 | Access controls | Prevent public from viewing securities-offering-style materials |
| 7.15+ | Order recording and handling | Audit, fairness, surveillance |

## 17. SFC VATP Guidelines — Chapter IX (Dealing with Clients) key sections

| § | Title | Applies to |
|---|---|---|
| 9.1 | Accurate representations | All clients |
| 9.2 | Advertisements | All clients |
| 9.3 | Jurisdictional restrictions | All clients |
| 9.4 | Knowledge assessment | Non-IPI / non-QCPI clients |
| 9.5 | KYC identity | Non-IPI / non-QCPI clients |
| 9.6 | Risk profiling | Non-IPI / non-QCPI clients |
| 9.7 | Exposure limits | Non-IPI / non-QCPI clients |
| 9.8 | Origination / beneficiaries | All clients |
| 9.11 | Client agreement | All clients |
| 9.26 | Schedule 2 risk disclosure | Non-IPI / non-QCPI clients |
| 9.27 | Website disclosure | All clients |
| 9.28 | Per-VA disclosure | All clients |

## 18. SFC 2025 Expansion Circular — definition of "VATPs"

Per [CIT-SFC-C-004], "VATPs" in the 2025 Circular means:

| Sub-category | Licensing basis |
|---|---|
| (a) | Corporation licensed for Type 1 (dealing in securities) **and** Type 7 (providing automated trading services) under section 116 of the SFO |
| (b) | Corporation licensed to provide a VA service under section 53ZRK of the AMLO |
| **Excluded** | VATPs which are deemed-to-be-licensed |

> **HKbitEX impact.** HKbitEX is deemed-to-be-licensed and therefore **excluded** from the 2025 Expansion Circular's definition. The 12-month track-record lift for PI offerings does not automatically apply.

## 19. SFC 2025 Expansion Circular — definition of "digital assets"

Per [CIT-SFC-C-005]:

| Term | Includes |
|---|---|
| **Digital assets** | Virtual assets + tokenised securities + stablecoins |
| **Tokenised securities** | Traditional financial instruments that are "securities" as defined in section 1 of Part 1 of Schedule 1 to the SFO which utilise distributed ledger technology (DLT) or similar technology in their security lifecycle |
| **Stablecoin** | As defined in section 3 of the Stablecoins Ordinance (Cap. 656) — irrespective of whether issued by an HKMA-licensed issuer |
| **Digital-asset-related products** | Investment products relating to digital assets |

## 20. HKbitEX obligations map

| SFC requirement | Operationalised in |
|---|---|
| §7.1–7.5 token admission committee | HKbitEX LR Chapter 3 + TAP §2.3 + Appendix B |
| §7.6 DD factors (10 enumerated) | HKbitEX TAP §3.2 (18 factors; this doc §10 above) |
| §7.7 retail-only gates | Not currently active for HKbitEX (PI-only per TAP §4.1.3); would activate on retail expansion |
| §7.8 Eligible Large-Cap test | Would activate on retail expansion |
| §7.10 smart-contract audit | HKbitEX TAP §3.4 |
| §7.11–7.12 ongoing monitoring + status tracking | HKbitEX TAP §6.3 |
| §7.13 / Part IV SFO / C(WUMP)O offering rules | HKbitEX Listing Rules Schedule 1 + 2 |
| §9.26–9.27 website disclosure | HKbitEX TAP §6.1.3 + §6.1.4 |
| Schedule 2 risk disclosures | This doc §11 above (verbatim); required in any platform-facing doc |
| Monthly report to Board | HKbitEX TAP Appendix D + `03-monthly-report-head-of-listing.md` |
| Track-record 12-month rule | Currently binding for HKbitEX (deemed-to-be-licensed); would lift with SFC licence-condition variation |

## 21. Cross-reference to HKbitEX source documents

| This document section | SFC source | HKbitEX source |
|---|---|---|
| §2 governance | VATP Guidelines §§7.1–7.5 | HKbitEX LR Chapter 3 + TAP Appendix B (see `02-…`) |
| §3 admission DD | VATP Guidelines §7.6 | HKbitEX TAP §3.2 (18 factors) |
| §4 retail gate | VATP Guidelines §§7.7–7.8 | HKbitEX TAP §4.1.3 (PI-only admission) |
| §6 ongoing monitoring | VATP Guidelines §§7.11–7.12 | HKbitEX TAP §6.3 (daily + monthly; see `06-ongoing-monitoring-of-each-admitted-va.md`) |
| §7 client-facing | VATP Guidelines §§9.3–9.7, 9.27–9.28 | HKbitEX TAP §6.1 |
| §8 risk disclosure | VATP Guidelines Schedule 2 | HKbitEX TAP §6.1.2 |
| §9 2025 Circular | SFC Circular 3 Nov 2025 | Not yet reflected in HKbitEX docs (deemed-to-be-licensed) |

## 22. What this document is NOT

This document does not cover:

- AML/CFT specifics (separate AML Guideline)
- Custody specifics (separate custody framework; out of scope this round)
- Auditor obligations under Part XV of the VATP Guidelines (covered by separate audit policy)
- Cross-border offerings under Part IV SFO (legal-interpretation territory)

When any of these intersect with token admission, the relevant SFC text must be reviewed independently and incorporated by reference.

## 23. Source

| Source | Path | Last verified |
|---|---|---|
| SFC VATP Guidelines (June 2023) | `../reference/SFC/Guidelines-for-Virtual-Asset-Trading-Platform-Operators.pdf` | 28 Sep 2026 |
| SFC Circular on Expansion of Products and Services (3 Nov 2025) | `../reference/SFC/Circular on expansion of products and services of virtual asset trading platforms.pdf` | 28 Sep 2026 |
| HKbitEX Listing Rules v3.1 | `../reference/HKbitEX/4001_20240603_HKbitEX_Listing Rules_v3.1 1.docx` | 28 Sep 2026 |

*All quotes in §3, §4, §5, §6, §7, §8, §9, §10, §11, §12, §13, §18, §19, §20 above are paraphrased or summarised from the SFC VATP Guidelines and 2025 Expansion Circular. For verbatim quotes used in any load-bearing argument, the reviewer must re-verify against the current source document page directly.*
