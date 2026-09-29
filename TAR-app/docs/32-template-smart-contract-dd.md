---
title: Smart Contract DD Report Template
category: iii     # Templates + reporting
classification: private
owner: R&D + Legal
last_reviewed: 2026-09-29
---

# 32 — Smart Contract DD Report Template

> **Purpose.** Template for documenting a smart-contract audit vendor's findings and HKbitEX's response, as required by HKbitEX TAP §3.4 and HKbitEX Listing Rules LR 2.2 (Smart Contract Audit factor). Any framework not present in HKbitEX's approved policies is flagged in §10 gap addendum.

---

## How to use

1. One report per audit engagement
2. **Before Part A:** complete the **Audit Path Selection** section below (Path A vs Path B) — this is mandatory and gates the entire report
3. Vendor completes Part A; HKbitEX R&D team completes Part B response; Head of Listing signs off Part C
4. Stored in the case file for the admitted VA
5. Versioned per `42-ai-agent-document-versioning.md`; 7-year retention

---

## Audit Path Selection (Path A vs Path B)

> **Why this section is first.** HKbitEX TAP §3.4 and SFC VATP Guidelines §7.10 [CIT-SFC-G-010] both give HKbitEX two valid paths for the smart-contract audit. The path must be selected and documented before the audit report can be accepted.

### Source: SFC VATP Guidelines §7.10 (verbatim) — [CIT-SFC-G-010]

> "Before admitting any virtual assets for trading, a Platform Operator should exercise due skill, care and diligence in selecting and appointing an independent assessor to conduct a smart contract audit for smart-contract based virtual assets, **unless the Platform Operator demonstrates that it would be reasonable to rely on a smart contract audit conducted by an independent assessor engaged by a third party**. The smart contract audit should focus on reviewing that the smart contract is not subject to any contract vulnerabilities or security flaws to a high level of confidence."

### Source: HKbitEX TAP §3.4 (verbatim)

> "Exercise due skill, care and diligence in selecting and appointing an independent assessor to conduct a smart contract audit for smart contract based Virtual Assets, **unless the Company demonstrates that it would be reasonable to rely on a smart contract audit conducted by an independent auditor assessor engaged by a third party**. The smart contract audit should focus on reviewing that the smart contract is not subject to any contract vulnerabilities or security flaws to a high level of confidence."

### Decision tree

```
                  ┌───────────────────────────────────────┐
                  │   Smart contract exists for the VA    │
                  └─────────────┬─────────────────────────┘
                                │
                ┌───────────────┴────────────────┐
                │                                │
                ▼                                ▼
   ┌───────────────────────────┐    ┌──────────────────────────────────┐
   │ Path A                    │    │ Path B                           │
   │ HKbitEX engages its own   │    │ HKbitEX relies on a third-      │
   │ independent assessor      │    │ party audit (engaged by Issuer    │
   │                           │    │ / dev team / community)          │
   └────────────┬──────────────┘    └────────────────┬─────────────────┘
                │                                     │
                ▼                                     ▼
   Per CIT-SFC-G-010 + TAP §3.4:        Per CIT-SFC-G-010 + TAP §3.4:
   - HKbitEX selects + appoints         - DEMONSTRATE reasonableness
   - HKbitEX exercises due skill,       - Auditor must still be independent
     care, diligence                   - Audit must still focus on
   - Audit focuses on vulnerabilities      vulnerabilities to high
     to high confidence                  confidence

   (Default path when no third-party audit exists)
```

### Path A — HKbitEX engages its own independent assessor

When to use:

- No third-party audit exists yet
- Third-party audit exists but fails the Path B acceptance criteria (§B below)
- The VA's smart contract is materially complex / novel / high-value (Path A preferred for risk concentration)

| Step | Owner | Action |
|---|---|---|
| 1 | R&D | Identify needed expertise (chain, language, domain) |
| 2 | R&D | Shortlist 2+ qualified auditors (vendor DD per GAP-AUDIT-002 in `40-…` §5.9.1) |
| 3 | R&D + Legal | Issue RFP; evaluate proposals; check independence |
| 4 | R&D | Complete mandatory vendor DD (GAP-AUDIT-003) |
| 5 | Head of Listing | Risk assessment + approval (Low / Medium / High risk rating) |
| 6 | Legal + R&D | Contract execution (NDA + audit contract + re-audit terms) |
| 7 | R&D | Engagement management; receive findings |
| 8 | R&D + Head of Listing | Acceptance decision per §B.2 (severity-gated; GAP-AUDIT-004) |

### Path B — HKbitEX relies on a third-party audit

When to use:

- A third-party audit already exists (engaged by Issuer / dev team / grant program / community)
- The audit meets Path B acceptance criteria (below)

**Path B acceptance criteria** (all six must be satisfied for HKbitEX to rely):

| # | Criterion | Evidence required | Source |
|---|---|---|---|
| 1 | **Auditor independence** | Auditor is independent of the Issuer / dev team (no equity, no prior engagement, no current business relationship) | SFC §7.10; HKbitEX TAP §3.4 |
| 2 | **Auditor competence** | Vendor DD passes per `12-policy-smart-contract-audit-vendor.md` §5 (GAP-AUDIT-001) | SFC §7.10; HKbitEX TAP §3.4 |
| 3 | **Scope match** | The third-party audit covers the exact smart contract(s) HKbitEX is admitting (not a subset, not an older version) | SFC §7.10 |
| 4 | **Methodology** | Audit combined automated tooling + expert manual review (not just an automated scan) | SFC §7.10 |
| 5 | **Severity classification** | Findings classified Critical / High / Medium / Low / Informational with clear remediation guidance | HKbitEX TAP §3.4 |
| 6 | **Freshness** | Audit date within 6 months (or re-audit / auditor sign-off that prior audit still applies) | Operational best practice |

**What "demonstrates it is reasonable" means** — HKbitEX must keep an **audit-reliance log** with:

| Log field | Value |
|---|---|
| VA name | |
| Third-party auditor name | |
| Audit date | |
| Scope coverage | |
| Independence evidence | |
| Vendor DD score (per `12-policy-smart-contract-audit-vendor.md`) | |
| Path B criteria 1–6 pass/fail | |
| R&D reviewer + sign-off | |
| Head of Listing sign-off | |
| Decision | APPROVE Path B / DECLINE → fall back to Path A |

### When Path B fails — fall back to Path A

If any of the six acceptance criteria fails:

- HKbitEX declines to rely on the third-party audit
- HKbitEX engages its own independent assessor (Path A) before admission
- Decline reason + supporting evidence filed in the case file (7-year retention)
- Issuer informed; given opportunity to commission a new audit that meets criteria

### Audit Path Selection record (fill in for every admission)

| Field | Value |
|---|---|
| **VA** | |
| **Application Reference** | |
| **Selection date** | |
| **Path chosen** | A (HKbitEX-engaged) / B (third-party reliance) |
| **If A — HKbitEX-selected auditor** | |
| **If A — vendor DD score** | Low / Medium / High |
| **If A — engagement contract reference** | |
| **If B — third-party auditor** | |
| **If B — Path B criterion 1 (independence)** | Pass / Fail |
| **If B — Path B criterion 2 (competence / vendor DD)** | Pass / Fail |
| **If B — Path B criterion 3 (scope match)** | Pass / Fail |
| **If B — Path B criterion 4 (methodology)** | Pass / Fail |
| **If B — Path B criterion 5 (severity classification)** | Pass / Fail |
| **If B — Path B criterion 6 (freshness)** | Pass / Fail |
| **If B — audit-reliance log entry ID** | |
| **R&D reviewer name + sign-off** | |
| **Head of Listing sign-off** | |
| **Decision** | APPROVE / DECLINE (→ Path A) |

---

# Smart Contract Audit Report — DD Summary

---

# Smart Contract Audit Report — DD Summary

| Field | Value |
|---|---|
| **Report Reference** | SC-AUDIT-{TOK or VENDOR}-{YYYY-MM-DD}-{NNN} |
| **Engagement type** | Initial / Upgrade / Re-audit / Ongoing |
| **Vendor** | |
| **Engagement dates** | Start – End |
| **Client (Project)** | |
| **Target blockchain(s)** | |
| **Smart contract language(s)** | |
| **Estimated LOC** | |

---

## PART A — Audit Findings (completed by vendor)

### A.1 Summary

| Field | Detail |
|---|---|
| Total findings | |
| (Vendor's own severity breakdown, if used) | |

### A.2 Findings Detail (per finding)

Repeat this block for each finding:

#### Finding {NNN} — {Title}

| Field | Value |
|---|---|
| **Severity (vendor)** | Vendor's scale (e.g., Critical / High / Medium / Low) |
| **Status** | Open / Acknowledged / Resolved / Disputed |
| **Affected contract(s)** | File path + line numbers |
| **Description** | [Full description of the vulnerability] |
| **Impact** | [What could happen if exploited] |
| **Likelihood** | High / Medium / Low |
| **Recommendation** | [Specific fix or mitigation] |
| **Vendor's exploit scenario** | [How the vulnerability could be weaponised] |
| **References** | [CWE, similar past exploits, academic papers] |

### A.3 Architectural Observations

[Higher-level observations about the protocol design, not tied to specific findings. May include systemic risk patterns.]

### A.4 Tooling and Methodology

| Tool / Method | Used | Notes |
|---|---|---|
| Slither | Y/N | |
| Mythril | Y/N | |
| Echidna | Y/N | |
| Foundry | Y/N | |
| Manual review | Y/N | |
| Formal verification | Y/N | |
| Other: | | |

### A.5 Vendor sign-off

| Field | Value |
|---|---|
| **Auditor** | |
| **Lead auditor** | |
| **Report date** | |
| **Vendor signature** | |

---

## PART B — Platform Response (completed by HKbitEX R&D)

### B.1 Per-finding Platform response

For each finding, document:

| Field | Value |
|---|---|
| **Finding reference** | |
| **Accepted?** | Yes / No / Partially |
| **Remediation status** | Remediated / Mitigated / Acknowledged / Disputed |
| **Remediation description** | [What was changed; PR or commit reference] |
| **Re-audit required?** | Yes / No |
| **Re-audit reference** | |
| **Notes** | |

### B.2 HKbitEX audit acceptance criteria (per HKbitEX TAP §3.4)

Per HKbitEX TAP §3.4, the audit must focus on:
> "reviewing that the smart contract is not subject to any contract vulnerabilities or security flaws to a high level of confidence."

Acceptance requires that the smart contract be free from contract vulnerabilities and security flaws to a high level of confidence. Critical or High findings must be remediated, mitigated with explicit TARC sign-off, or explicitly accepted by TARC with documented rationale.

### B.3 Re-audit verification

| Field | Value |
|---|---|---|
| Re-audit completed? | Yes / No / Pending |
| Re-audit vendor | (may differ from initial) |
| Re-audit date | |
| Re-audit findings status | All remediated / Some outstanding |
| Outstanding findings rationale | |

### B.4 R&D sign-off

| Field | Value |
|---|---|
| **R&D Lead** | |
| **Date** | |
| **Signature** | |

---

## PART C — Platform Risk Acceptance (completed by Head of Listing)

### C.1 Listing readiness

| Gate | Status |
|---|---|
| All Critical findings remediated (or TARC-approved for mitigation) | ☐ |
| All High findings remediated (or TARC-approved for mitigation) | ☐ |
| Re-audit completed (if any High/Critical findings existed pre-fix) | ☐ |
| Smart-contract addresses finalised and verified on-chain | ☐ |
| Multi-sig and timelock configuration verified | ☐ |
| Upgrade path understood and documented | ☐ |

### C.2 Listing recommendation

| Outcome | Tick |
|---|---|
| **READY for listing** | ☐ |
| **READY with conditions** (specify below) | ☐ |
| **NOT READY** — additional remediation required | ☐ |

### C.3 Conditions (if applicable)

| # | Condition | Owner | Deadline |
|---|---|---|---|
| 1 | | | |
| 2 | | | |

### C.4 Sign-off

| Role | Name | Signature | Date |
|---|---|---|---|
| **Head of Listing** | | | |
| TARC Chairperson (if escalated) | | | |

---

## APPENDIX A — Vendor's full report

[Attach vendor's full audit report as Appendix A.]

---

## APPENDIX B — Findings status tracking (long-term)

For each finding tracked through the lifecycle:

| Finding ID | First seen | Severity | Status | Last update | Closed date |
|---|---|---|---|---|---|
| | | | | | |

---

## Cross-references

- HKbitEX Listing Rules LR 2.2 — Smart Contract Audit factor
- HKbitEX TAP §3.4 — smart contract audit requirement
- [CIT-SFC-G-010] SFC VATP Guidelines §7.10 — independent-assessor requirement
- `20-procedure-token-admission.md` — when audit is required (see live gap log)
- `23-procedure-ongoing-monitoring.md` — periodic re-audit trigger (see live gap log)
- `22-procedure-incident-escalation.md` — incident-driven re-audit (see live gap log)
- `30-template-dd-checklist.md` Section 03 — Security Risk checklist that consumes this report
- `42-ai-agent-document-versioning.md` — versioned storage

---

## 10. HKbitEX policy gaps (for TAR-app gap-analysis feature)

| Gap ID | Reference | Framework | HKbitEX source status | Action required |
|---|---|---|---|---|
| GAP-SCD-001 | §A.1 severity classification | Standardised Critical / High / Medium / Low / Informational taxonomy | HKbitEX does not specify a severity taxonomy; vendor's scale is used | Manager review (vendor-specific) |
| GAP-SCD-002 | §A.4 mandatory tooling list | Required static-analysis + dynamic + manual tools | HKbitEX TAP §3.4 does not specify tools | Manager review |
| GAP-SCD-003 | §A.2 finding ID convention | Vendor-neutral finding ID for cross-engagement tracking | Not in HKbitEX docs | Manager review |
| GAP-SCD-004 | §B.2 explicit acceptance thresholds | Severity-gated blocking rules (Critical = block; High = block unless TARC-approved mitigation; etc.) | HKbitEX TAP §3.4 does not specify thresholds | Manager review |
| GAP-SCD-005 | §B.3 re-audit triggers and cadence | When a re-audit is required (post-remediation, periodic refresh, incident-driven) | Not in HKbitEX docs | Manager review |
| GAP-SCD-006 | §C.1 standard readiness checklist | Standard listing-readiness gates | HKbitEX TAP §3.4 does not specify a checklist | Manager review |
| GAP-SCD-007 | TAR-app automation | Vendor report parsing, finding extraction, severity normalisation | TAR-app system feature | Future TAR-app implementation |
