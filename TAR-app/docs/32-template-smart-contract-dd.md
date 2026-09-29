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
2. Vendor completes Part A; HKbitEX R&D team completes Part B response; Head of Listing signs off Part C
3. Stored in the case file for the admitted VA
4. Versioned per `42-ai-agent-document-versioning.md`; 7-year retention

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
