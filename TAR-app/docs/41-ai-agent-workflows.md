---
title: TAR-app — AI Agent Workflows (Stage-by-Stage)
category: system # TAR-app architecture (not HKbitEX policy)
classification: public
owner: CTO + Head of Listing
last_reviewed: 2026-09-29
status: design-only (implementation pending approval)
---

# 41 — TAR-app AI Agent Workflows (Stage-by-Stage)

> **Purpose.** Stage-by-stage specification of TAR-app agent workflows. Three workflows are emphasised because you specifically called them out: **gap-analysis** (§4), **validation agent with weblinks** (§6), and **document versioning + approval/accepting-changes** (in `42-…`).
>
> **HKbitEX alignment.** All workflows respect HKbitEX's approved policies (docs/01-07). Where TAR-app extends beyond HKbitEX docs, the extension flows to the gap-analysis workflow (see §4).

---

## 1. Master workflow map

```
[ADMISSION PHASE — anchored to HKbitEX LR Chapter 6 + TAP §3–§5]
├─ 1.1 Intake & triage
├─ 1.2 Application log + COI gate
├─ 1.3 Document classification & extraction
├─ 1.4 DD execution (6 sections per HKbitEX TAP §3.3)
├─ 1.5 Discrepancy resolution (per HKbitEX Review of Information Sources)
├─ 1.6 Validation agent runs (weblinks attached)
├─ 1.7 Recommendation synthesis
├─ 1.8 Head of Listing review (HKbitEX TAP §4.2.5)
├─ 1.9 SFC pre-notification draft (HKbitEX TAP §4.4.1)
├─ 1.10 TARC meeting pack preparation (HKbitEX LR Chapter 3)
├─ 1.11 Post-decision: Listing Document + Listing prep + Go-live (HKbitEX TAP §4.5)

[MONITORING PHASE — anchored to HKbitEX TAP §6.3]
├─ 2.1 Continuous monitoring (alert agents — beyond HKbitEX TAP §6.3.2 daily)
├─ 2.2 Daily screening (HKbitEX TAP §6.3.2)
├─ 2.3 Monthly report generation (HKbitEX TAP Appendix D + 4010 template)

[GAP-ANALYSIS PHASE — TAR-app feature you proposed]
├─ 3.1 Continuous gap detection (per policy docs)
├─ 3.2 Manager review workflow (UI + notification)
├─ 3.3 Accept → Board approval → becomes HKbitEX policy
       OR Reject → framework removed from TAR-app behaviour
```

---

## 2. ADMISSION workflows (selected highlights)

This section summarises selected admission workflows; full detail in the per-doc gap addenda.

### 2.1 Intake & triage

| Field | Detail |
|---|---|
| **Trigger** | Email / portal / API submission |
| **Agents** | Intake Agent |
| **Human checkpoint** | Case Officer assignment |
| **HKbitEX anchor** | HKbitEX TAP §4.1; LR 6.1 |
| **Artefacts** | Application Reference; preliminary eligibility memo; case file skeleton |

### 2.2 DD execution (6 sections in parallel, per HKbitEX TAP §3.3)

| Field | Detail |
|---|---|
| **Trigger** | Structured data available; all source attributions in place |
| **Agents** | 6 DD Analyst Agents (parallel) + Smart Contract Audit Agent + Validation Agent |
| **Human checkpoint** | Each agent's output reviewed by the corresponding team lead per TAP §3.3 |
| **HKbitEX anchor** | TAP §3.2 (16-18 DD criteria); TAP §3.3 (per-department-head responsibilities) |
| **Artefacts** | 6 DD section reports; 1 smart-contract DD report; 1 validation report |
| **Guardrails** | No item marked Verified without source_url + access_date + verification_method; no silent discrepancy resolution |

### 2.3 Recommendation synthesis + Head of Listing review

| Field | Detail |
|---|---|
| **Trigger** | All 6 DD sections + smart-contract report + validation report complete |
| **Agents** | Recommendation Synthesiser |
| **Human checkpoint** | Head of Listing review (HKbitEX TAP §4.2.5) |
| **Artefacts** | DD report per `30-template-dd-checklist.md` |

### 2.4 TARC meeting pack preparation (HKbitEX LR Chapter 3)

| Field | Detail |
|---|---|
| **Trigger** | TARC meeting scheduled (≥3 BD notice per LR Chapter 3) |
| **Agents** | Meeting Pack Generator |
| **Human checkpoint** | Company Secretary + Head of Listing final review |
| **Artefacts** | Meeting pack (cover, agenda, DD report, SFC draft, COI declarations, draft resolutions, draft minutes) |
| **Guardrails** | Cannot include materials not in case file; cannot modify DD scores |

---

## 3. MONITORING workflows (anchored to HKbitEX TAP §6.3)

### 3.1 Daily screening (HKbitEX TAP §6.3.2)

```
on daily_cron:
    for each admitted VA:
        run DailyScreeningAgent (World-Check-One + project website scrape)
        if anomaly:
            raise alert
            escalate per §3.2 (incident routing)
        log to case file
```

### 3.2 Monthly report generation

```
on monthly_cron:
    for each admitted VA:
        run MonthlyReportGenerator per `31-template-monthly-report-hol.md`
        submit draft to Monitoring Team Lead for review
        submit approved report to TARC
```

---

## 4. GAP-ANALYSIS workflow (the central TAR-app feature you proposed)

> **Status.** Design only. Implementation pending approval.

### 4.1 Overview

The gap-analysis module surfaces every instance where TAR-app uses a framework that is **not present in HKbitEX's currently approved policies**. Manager reviews and either accepts (proceeds to Board approval → becomes HKbitEX policy) or rejects (framework removed from TAR-app behaviour).

### 4.2 Gap detection (continuous)

Every agent action passes through the Gap Analysis Agent (`40-…` §5.9). On detecting a non-HKbitEX framework:

```python
def on_agent_action(framework: str, doc: str, section: str):
    if framework in HKbitEX_approved_frameworks:
        proceed
    else:
        emit_gap_record(
            gap_id=f"GAP-{category}-{auto_n}",
            reference_doc=doc,
            reference_section=section,
            framework=framework,
            hkbitex_source_status="Not in HKbitEX docs",
            suggested_action="Manager review via TAR-app gap-analysis"
        )
        proceed  # but framework is flagged as advisory
```

The HKbitEX approved-framework corpus is built from `docs/02..07` (HKbitEX Listing Rules v3.1, TAP v1.0, monthly report template, LC minutes, Appendix 3 monitoring xlsx, Review of Information Sources on DD). Anything not in those documents is a gap.

### 4.3 Gap record schema

```yaml
gap_id: GAP-{CATEGORY}-{NN}                # e.g., GAP-COI-001
reference_doc: { path within docs/ }
reference_section: { section anchor }
framework: { description of the framework used }
hkbitex_source_status: Not in HKbitEX docs | Contradicts HKbitEX docs | Partial coverage
suggested_action: Manager review via TAR-app gap-analysis | Urgent | Defer
status: PENDING | UNDER_REVIEW | ACCEPTED | REJECTED
created_at: { UTC }
created_by: { agent ID }
manager_decision:
  decided_at: { UTC }
  decided_by: { manager user ID }
  rationale: { free text }
board_approval:                        # only if Accepted
  approved_at: { UTC }
  approved_by: { board resolution reference }
supersedes_policy_version: { pointer }   # if Accepted + replaces existing
```

### 4.4 Manager review workflow (UI + notification)

```
[PENDING gap]
   │
   ▼
[Manager notification]
   │   TAR-app sends notification via Slack/email/console
   │
   ▼
[Manager reviews gap]
   │   - Reads gap description
   │   - Reads reference doc + section
   │   - Considers: is this framework needed? Can we operate without it?
   │
   ▼
[Decision]
   ├─ ACCEPT ──→ [Board approval queue] ──→ [APPROVED] (Board vote) ──→ [Published as HKbitEX policy]
   │                                                                             │
   │                                                                             ▼
   │                                                              Policy doc updated; version committed; framework
   │                                                              status = "APPROVED HKbitEX policy"
   │
   └─ REJECT ──→ [REJECTED] (with rationale) ──→ framework removed from TAR-app behaviour
```

#### 4.4.1 Manager UI (sketch)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ TAR-app Gap Analysis Queue                                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  GAP-MON-002 — Monitoring                                                 │
│  Reference: 23-procedure-ongoing-monitoring.md §1.2 (added by Monitoring)   │
│  Framework: "4-pillar weighted scoring (Regulatory 40% / Security 30% /     │
│             Market 20% / Governance 10%) with 1–5 scale"                   │
│  HKbitEX source status: Not in HKbitEX docs                                │
│  Used by: Monitoring Agents → 12 times last month                          │
│  Created: 2026-09-15 by ContinuousAlertAgent                                │
│                                                                             │
│  Reference link: [link to §1.2 of 23-…]                                    │
│  Weakening: 2.0% of all decisions in September                              │
│                                                                             │
│  [ Accept → Board approval ]  [ Reject (remove framework) ]  [ Defer ]    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 4.5 Pre-loaded gaps (seeded at system launch)

When TAR-app launches, the gap log is pre-populated with the ~80 gaps already documented in the policy/template docs (see `40-…` §5.9.1 for the full list). Each pre-loaded gap enters the PENDING state and routes to the appropriate manager.

### 4.6 Continuous monitoring for new gaps

The Gap Analysis Agent runs on every agent action. As HKbitEX's policies evolve and TAR-app gains new capabilities, new gaps will surface. The gap log is the persistent record.

### 4.7 Gap acceptance workflow detail

Acceptance is equivalent to a Class A change to a HKbitEX policy doc — it requires Board approval:

```
1. Manager marks gap as "Accept for Board review"
   ↓
2. Head of Listing reviews and concurs
   ↓
3. Policy doc updated to incorporate the accepted framework
   ↓
4. New version of policy doc committed (per `42-…` versioning)
   ↓
5. TAR-app behaviour updated to use the now-approved framework
   ↓
6. Gap log entry marked ACCEPTED + linked to new policy version
   ↓
7. Policy doc published to HKbitEX staff + Board report
```

If Board does not approve:

```
1. Manager marks gap as "Accept for Board review"
   ↓
2. Head of Listing reviews and concurs
   ↓
3. Board review at next meeting
   ↓
4a. APPROVED → proceed with steps 3-7 above
4b. NOT APPROVED → gap reverts to PENDING; manager must re-decide (accept/reject/defer)
```

---

## 5. DISCREPANCY RESOLUTION workflow

Per HKbitEX *Review of Information Sources on DD for Virtual Asset* (14 October 2025) — `docs/07-…`.

### 5.1 Discrepancy classes

| Class | Description | Example |
|---|---|---|
| **A — Methodology difference** | Sources use different methodologies; both may be correct | CoinMarketCap vs CoinGecko supply (different inclusion sets) |
| **B — Snapshot timing** | Sources captured at different times | Reserve attestation (Dec 5) vs on-chain proof (Dec 6) |
| **C — Inclusion set** | Different exchanges / addresses included | Liquidity from "spot only" vs "spot + derivatives" |
| **D — Definition difference** | Sources define terms differently | Top holders: EOA only vs EOA + contracts |
| **E — Genuine contradiction** | Sources conflict on a fact that should be the same | Issuer claims 1:1 reserves; on-chain proof shows different |

### 5.2 State machine

```
[DETECT]
   │   Discrepancy Resolution Agent detects disagreement
   │
   ▼
[VALIDATE]
   │   For each source:
   │     - Is it recognised (per HKbitEX Review §4 — Appendix A)?
   │     - Is methodology documented?
   │     - Is the data current?
   │   Output: per-source credibility score
   │
   ▼
[CLASSIFY]
   │   Class A / B / C / D / E
   │
   ▼
[RECONCILE]
   │   For auto-resolvable classes (A–D):
   │     - Pick the most authoritative source (per HKbitEX Review §2 "General Criteria")
   │     - OR pick conservative estimate
   │     - Document methodology choice
   │
   ▼
[ESCALATE if Class E OR regulatory implication OR material impact]
   │   - Prepare escalation report
   │   - Notify Case Officer + Head of Listing
   │   - Optionally consult Legal (if regulatory implication)
   │
   ▼
[DECISION]
   │   Human decides:
   │     a. Adopt alternative source
   │     b. Wait for third source / new attestation
   │     c. Apply conservative estimate
   │     d. Contact Issuer for clarification
   │     e. Disclose uncertainty in DD report
   │     f. Escalate to TARC
   │
   ▼
[DOCUMENT]
   │   Discrepancy record with resolution rationale + approver
   │
   ▼
[STORE]
   │   Append-only record in case file; tamper-evident
   │
   ▼
[FEEDBACK]
   Recurring discrepancy patterns → review source list, methodology, agent config
```

### 5.3 Discrepancy categories that **must** escalate (no auto-resolve)

- Reserve backing deviations >0.5% (for stablecoins)
- Total supply deviations >2%
- Holder concentration deviations >5%
- Regulatory status (security classification) differences
- License/registration validity differences
- Sanctions screening matches
- Smart-contract audit finding status

## 6. VALIDATION AGENT workflow (with weblinks)

### 6.1 Position in the workflow

The Validation Agent runs **after** each DD Analyst Agent completes its section, and **before** the Recommendation Synthesiser aggregates. Output: a validation report attached to the DD report.

```
[DD Analyst Agent completes section]
   │
   ▼
[Validation Agent runs on section]
   │   - Source quality check
   │   - Cross-source consistency check
   │   - Numerical sanity check
   │   - Regulatory cross-reference check
   │   - **Attach weblink per claim**
   │
   ▼
[Validation report attached to DD report]
   │
   ▼
[Recommendation Synthesiser aggregates]
```

### 6.2 The 4 dimensions (per `40-…` §5.6.1)

For each claim in the DD section, validate:

| # | Dimension | Check | Pass criterion |
|---|---|---|---|
| a | Source quality | URL exists + accessible; access date ≤ 90 days; methodology documented; source in recognised list (HKbitEX Review §4 Appendix A) | All 4 sub-criteria pass |
| b | Cross-source consistency | For load-bearing claims: at least 2 independent sources; no unresolved Class E discrepancy | 2+ sources + no E flag |
| c | Numerical sanity | Totals reconcile; percentages of base; market cap = price × supply; reserves vs supply | All arithmetic checks pass |
| d | Regulatory cross-reference | Claim traces to a CIT-… (in `99-citations.md`) or to a direct SFC paragraph in `00-legal-framework-hk-sfc.md` | Trace exists |

### 6.3 Weblink generation (your explicit requirement)

Each validated claim in the DD report is rendered with a clickable weblink to a **versioned snapshot** of the source. Implementation:

| Aspect | Detail |
|---|---|
| **Storage** | Evidence store holds `sha256:<hash>` content-addressed snapshots of every fetched source |
| **Link format** | `[CIT-SFC-G-006 ¶7.6(a)](https://tar-app.example.com/evidence/<sha256>)` |
| **Lifetime** | Permanent for Class A/B docs (7 years minimum); permanent for SFC public docs |
| **Render rule** | Every load-bearing claim in DD report / monthly report / listing document must include a weblink in the rendered output |

### 6.4 Example validation report entry

```markdown
§3.2 Third-party Smart-Contract Audit | ✅ VERIFIED
   Claim: USDC has 4+ independent audits including Trail of Bits (2023, 2021), Quantstamp (2023, 2021), Halborn (2022), Certora (2022)
   Source: GitHub — https://github.com/circlefin/stablecoin-evm/tree/master/audits
   Weblink: https://tar-app.example.com/evidence/sha256:abc123...
   Access date: 2026-09-28
   Validation: ✅ Source quality; ✅ Cross-source (4 audits); ✅ Numerical (4 cited); ✅ Reg cross-ref
```

This allows any reviewer to **click the weblink** and verify the source themselves.

### 6.5 Validation failure handling

If a claim fails any of the 4 dimensions:

- **Fail source quality** → flag in DD report; do not block other validation
- **Fail cross-source** → invoke Discrepancy Resolution Agent (see §5); if unresolved, escalate to Head of Listing
- **Fail numerical sanity** → flag in DD report with the arithmetic error; reviewer must verify
- **Fail regulatory cross-reference** → flag in DD report; LLM may have invented the claim; remove if unrecoverable

A failed validation does not necessarily block the DD report; it surfaces a flag for human review.

---

## 7. SLAs (advisory; not in HKbitEX docs)

| Workflow | SLA |
|---|---|
| Initial intake response | 10 BD |
| COI gate | 5 BD |
| Document classification (per doc) | 30 min |
| DD section completion | 5 BD per section |
| Discrepancy detection (after sources gathered) | Real-time |
| Discrepancy resolution (Class A-D) | 1 BD |
| Discrepancy escalation (Class E) | 24 hours |
| Validation agent run (per section) | < 30 min |
| Scoring + recommendation synthesis | 1 BD after DD complete |
| Head of Listing review | 5 BD |
| SFC pre-notification draft | 2 BD |
| TARC meeting pack | 2 BD before meeting |
| Daily screening log | T+1 day |
| Monthly monitoring report | T+10 BD |
| **Gap detection** | **Real-time per agent action** |
| **Gap acceptance to Board** | **Next Board meeting** |

## 8. Cross-references

- `40-ai-agent-design.md` — agent roles, shared state, guardrails
- `42-ai-agent-document-versioning.md` — versioning + approval/accepting-changes flow (staging)
- `00-legal-framework-hk-sfc.md` — SFC binding regulation
- `99-citations.md` — citation registry backing every claim
- `30-template-dd-checklist.md` — the artefact the agents produce
- `31-template-monthly-report-hol.md` — monthly report template
- HKbitEX source docs (`02..07`) — extracted text used as ground truth
