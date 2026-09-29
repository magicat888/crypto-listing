---
title: TAR-app — System Design & AI Agent Architecture
category: system # TAR-app architecture (not HKbitEX policy)
classification: public
owner: CTO + Head of Listing
last_reviewed: 2026-09-29
status: design-only (implementation pending approval)
---

# 40 — TAR-app System Design & AI Agent Architecture

> **Purpose.** Architecture and agent design for TAR-app, focused on the three features you specifically called out: **gap-analysis workflow**, **staging version control**, and **validation agent with weblinks** for DD.
>
> **Scope.** Design only. Not yet implemented. After your approval, a phased implementation plan (`41-ai-agent-workflows.md` for workflow specs; `42-ai-agent-document-versioning.md` for versioning detail) follows.
>
> **HKbitEX alignment.** Every agent behaviour either (a) is anchored to a HKbitEX source document in `docs/` (00–07 + extracts) or (b) is flagged in the relevant gap addendum.

---

## 1. Design principles

| Principle | Operational meaning |
|---|---|
| **HKbitEX-strict** | Every agent action traces to a HKbitEX source doc (00-07 + extracts) or to a binding SFC text. Frameworks not in HKbitEX docs go through gap-analysis (see `41-…` §4). |
| **Human-in-the-loop at every commit** | No agent publishes, notifies, recommends admission, or accepts a gap without a named human approver. |
| **Source-grounded with permanent links** | Every load-bearing claim in a TAR-app output has a CIT-… ID and a clickable link to a versioned snapshot of the source (see §6). |
| **Discrepancy-first** | When sources disagree, the agent surfaces the discrepancy explicitly — never silently picks one. |
| **Gap-aware** | When the agent encounters a situation not covered by HKbitEX's current policies, it routes to the gap-analysis workflow (see `41-…` §4) for manager review. |
| **Audit-trail by default** | Every action appends to an immutable, hash-chained log. |
| **Versioned staging** | Every document change flows through propose → review → accept/reject, with named approvers per stage (see `42-…`). |

## 2. Stack

| Layer | Choice | Why |
|---|---|---|
| **Agent framework** | [eve.dev](https://eve.dev) | Durable agent framework; "like Next.js for agents"; supports skills (Markdown playbooks), tools (TypeScript), sandbox (isolated execution), channels (Slack/Teams/web), connections (auth) |
| **LLM backend** | MiniMax | For long-context document ingestion and synthesis |
| **Frontend** | Next.js + Tailwind + shadcn/ui on Vercel | Canonical Vercel stack |
| **Backend API** | Vercel serverless functions (orchestrating eve agents) | Stateless, scales, deploys via Vercel |
| **Data store** | MongoDB Atlas | Document model fits case files; agents share state |
| **Vector store** | Pinecone serverless | Embeddings for citation lookup and RAG |
| **File storage** | S3 (or Vercel Blob) | Versioned snapshots of source documents |
| **OCR** | TBD (AWS Textract / Google Document AI / Tesseract) | For image-based PDFs (e.g., some audit reports) |
| **Auth** | Corporate SSO (Okta / Azure AD) | Single-tenant HKbitEX |
| **CI/CD** | GitHub Actions + Vercel auto-deploy | Standard |
| **Observability** | OpenTelemetry + structured logs | Cost-per-case dashboards |

## 3. The three document categories (per your spec)

The TAR-app RAG index uses three primary categories aligned with HKbitEX's operational taxonomy:

| Category | Description | Classification | Current docs |
|---|---|---|---|
| **i. SFC + Laws** | Binding regulatory text + HKbitEX methodology derived from SFC | **public** | `00-legal-framework-hk-sfc.md`, `01-listing-rules-v3.1.md` (see note), `99-citations.md` |
| **ii. HKbitEX Policies** | HKbitEX-internal governance: LR v3.1, TAP v1.0, monthly report template, LC minutes, Review of Information Sources | **private** | `02-token-admission-policies-and-procedures-v1.0.md`, `03-…`, `04-…`, `05-…`, `06-…`, `07-…` |
| **iii. Working Templates** | DD checklist, monthly report template, smart-contract DD template, listing document template | **private** | `30-…`, `31-…`, `32-…`, `33-…` |

> Note: doc `01-listing-rules-v3.1.md` is an HKbitEX-source extract but appears under category i in this layout because it's used as a reference for SFC-bound regulatory framework; the i/ii/iii split is operational (what TAR-app does with the doc), not taxonomic.

## 4. System overview

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                          External / Source Layer                              │
│                                                                              │
│  - Issuer / Applicant submissions (email, portal, API)                       │
│  - HKbitEX policy corpus (docs/02..07 — verbatim extracts)                   │
│  - SFC + Law corpus (docs/00, 99-citations)                                  │
│  - HKbitEX internal monitoring data (Appendix 3 xlsx)                        │
│  - On-chain data (Etherscan, Solscan, Nansen, Chainalysis)                  │
│  - Off-chain data (CoinMarketCap, CoinGecko, Bloomberg)                      │
│  - Sanctions / KYC (World-Check-One, OFAC, EU, UN)                           │
│  - Smart-contract audit reports                                               │
└──────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                       Ingestion + Normalisation                               │
│  - Document parser (PDF via MarkItDown; DOCX via python-docx)               │
│  - On-chain ingestion (event listeners, periodic snapshot)                    │
│  - Source registry (per HKbitEX Review of Information Sources)               │
│  - Evidence store (versioned, tamper-evident; per-version snapshots)         │
│  - Citation registry (HKbitEX + SFC citations, versioned)                     │
│  - **HKbitEX gap-detection module** (flags frameworks not in HKbitEX docs)   │
└──────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                       Agent Layer (orchestrated by eve.dev)                  │
│                                                                              │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐              │
│  │ Intake     │  │ DocClass & │  │ DD         │  │ Discrepancy│              │
│  │ Agent      │→ │ Extract    │→ │ Analyst    │→ │ Resolution │              │
│  │            │  │ Agent      │  │ Agents     │  │ Agent      │              │
│  └────────────┘  └────────────┘  └────────────┘  └────────────┘              │
│        │               │              │               │                     │
│        │               │              ▼               │                     │
│        │               │      ┌────────────┐         │                      │
│        │               │      │ Smart-     │         │                      │
│        │               │      │ Contract   │         │                      │
│        │               │      │ Audit Agent│         │                      │
│        │               │      └────────────┘         │                      │
│        │               │              │               │                     │
│        │               ▼              ▼               ▼                     │
│        │      ┌──────────────────────────────────────────┐                  │
│        │      │   Compliance / Legal / Risk Agents      │                  │
│        │      │   (specialist, per HKbitEX TAP §3.3)    │                  │
│        │      └──────────────────────────────────────────┘                  │
│        │                          │                                        │
│        ▼                          ▼                                        │
│  ┌────────────────────────────────────────────┐                            │
│  │   Recommendation Synthesiser + Report       │                            │
│  │   Generator (proposes; never decides)      │                            │
│  └────────────────────────────────────────────┘                            │
│                                      │                                      │
│                                      ▼                                      │
│  ┌────────────────────────────────────────────┐                            │
│  │   Validation Agent (4 dimensions; see §6)   │                            │
│  │   - Source quality (URL + access date +      │                            │
│  │     methodology per HKbitEX Review)         │                            │
│  │   - Cross-source consistency                │                            │
│  │   - Numerical sanity (totals reconcile)     │                            │
│  │   - Regulatory cross-reference              │                            │
│  │   - **Every claim has a clickable weblink** │                            │
│  └────────────────────────────────────────────┘                            │
│                                      │                                      │
│                                      ▼                                      │
│  ┌────────────────────────────────────────────┐                            │
│  │   Gap Analysis Agent (continuous)          │                            │
│  │   - Detects situations where TAR-app uses a │                            │
│  │     framework NOT in HKbitEX docs          │                            │
│  │   - Routes to gap-analysis workflow        │                            │
│  │     (review → accept/reject)                │                            │
│  └────────────────────────────────────────────┘                            │
│                                      │                                      │
│                                      ▼                                      │
│  ┌────────────────────────────────────────────┐                            │
│  │   Monitoring Agents (post-listing)          │                            │
│  │   - Continuous Alert (HKbitEX TAP §6.3.2)   │                            │
│  │   - Daily Screening (HKbitEX TAP §6.3.2)    │                            │
│  │   - Monthly Report Generator                │                            │
│  └────────────────────────────────────────────┘                            │
└──────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                       Decision Layer (HUMAN)                                 │
│                                                                              │
│  - Case Officer (Listing Dept)                                               │
│  - Head of Listing                                                          │
│  - TARC members (9 seats per HKbitEX LR Chapter 3)                            │
│  - Compliance / Legal / Risk approval gates                                 │
│  - Manager (gap-analysis accept/reject)                                     │
└──────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌──────────────────────────────────────────────────────────────────────────────┐
│                       Output / Action Layer                                  │
│                                                                              │
│  - Versioned DD reports (per `42-…`)                                        │
│  - TARC meeting packs (auto-generated)                                       │
│  - SFC pre-notification letters (drafted; human-reviewed)                    │
│  - Client communications (drafted; human-approved)                          │
│  - Listing Document drafts (human-approved)                                  │
│  - **Gap-analysis alerts (manager-facing; review → accept/reject)**          │
│  - Audit trail / event log (hash-chained, tamper-evident)                    │
└──────────────────────────────────────────────────────────────────────────────┘
```

## 5. Agent roles

Each TAR-app agent maps to an eve.dev "agent" — a folder with `instructions.md` (role + behaviour), optional `agent.ts` (model + runtime config), optional `skills/*.md` (playbooks), optional `tools/*.ts` (callable tools), optional `channels/*.ts` (notification delivery), optional `connections/*.ts` (auth).

### 5.1 Intake Agent

| Aspect | Detail |
|---|---|
| **eve definition** | `agents/intake/` |
| **Skills** | HKbitEX LR Chapter 6; HKbitEX TAP §4.1 |
| **Tools** | Email ingestion; DOCX/PDF parser; COI declaration distractor; SLA timer |
| **Outputs** | Application Reference (`TA-YYYY-NNN`); preliminary eligibility memo; case file skeleton |
| **Guardrails** | Cannot approve/decline; cannot issue Application Reference without fee; cannot override COI gate |

### 5.2 Document Classification & Extraction Agent

| Aspect | Detail |
|---|---|
| **eve definition** | `agents/doc-class/` |
| **Skills** | Recognised document types; field schemas per type |
| **Tools** | MarkItDown (DOCX); pdftotext + MarkItDown (PDF); OCR for image-based PDFs; embedding + retrieval |
| **Outputs** | Structured per-document data; document-class tags; source-quality flags; missing-field flags |
| **Guardrails** | Cannot invent missing fields; cannot accept image-based PDFs without OCR confidence ≥ 0.85 per field; cannot skip source attribution |

### 5.3 DD Analyst Agents (specialists per HKbitEX TAP §3.3)

| Agent | Section in 30-template-dd-checklist.md | Owner |
|---|---|---|
| **Token Basic Info Agent** | §01 | Operations |
| **Project Incorporation Agent** | §02 | Compliance |
| **Security Risk Agent** | §03 | R&D |
| **Legal Risk Agent** | §04 | Legal |
| **Compliance Risk Agent** | §05 | Compliance |
| **Governance & Market Agent** | §06 | Risk |

Each specialist agent:
- **Inputs:** extracted data + external sources for its domain
- **Outputs:** completed DD section with status per item (Verified / Pending / Rejected); per-item evidence citations
- **Guardrails:**
  - Cannot mark Verified without source_url + access_date + verification_method
  - Cannot silently resolve discrepancies
  - Cannot invent missing fields
  - Cannot propose admission/decline — only section completion

### 5.4 Smart Contract Audit Agent

| Aspect | Detail |
|---|---|
| **eve definition** | `agents/smart-contract/` |
| **Skills** | Finding-severity normalisation (vendor scale → standard scale); remediation tracking |
| **Tools** | DOCX/PDF parser; on-chain contract code retrieval; audit report parsing |
| **Outputs** | Findings table (per `32-template-smart-contract-dd.md`); severity-gated acceptance decision; remediation tracking |
| **Guardrails** | Cannot declare finding remediated without re-audit or source-code diff; cannot downgrade Critical findings without TARC sign-off |

### 5.5 Discrepancy Resolution Agent

| Aspect             | Detail                                                                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **eve definition** | `agents/discrepancy/`                                                                                                                                               |
| **Skills**         | Discrepancy classification (Methodology / Timing / Inclusion / Definition / Genuine Contradiction) per HKbitEX Review of Information Sources §3                     |
| **Tools**          | Source validators; data-provenance tracker; cross-source reconciler                                                                                                 |
| **Outputs**        | Discrepancy record; recommended resolution; impact assessment                                                                                                       |
| **Guardrails**     | Cannot silently pick one source; cannot declare resolved without Head of Listing approval; cannot resolve regulatory-implication discrepancies without Legal review |


### 5.6 Validation Agent (one of the three you asked for)

| Aspect | Detail |
|---|---|
| **eve definition** | `agents/validation/` |
| **Skills** | Source-quality checklist; numerical-sanity rules; regulatory cross-reference patterns |
| **Tools** | Citation-registry lookup; permanent-link resolver (versioned snapshots); arithmetic verifier |
| **Outputs** | Validation report with PASS/FAIL per claim + clickable weblink |
| **Guardrails** | Cannot approve a claim without a weblink; cannot silently fix discrepancies |

#### 5.6.1 Validation dimensions (4)

| # | Dimension | Check |
|---|---|---|
| a | **Source quality** | Every load-bearing claim has verifiable URL + access date + methodology (per HKbitEX Review of Information Sources §2 "General Criteria") |
| b | **Cross-source consistency** | Detect when two sources disagree; never silently pick one; flag discrepancy (per HKbitEX Review §3) |
| c | **Numerical sanity** | Totals reconcile; percentages of base; market cap = price × supply; volumes aggregate; reserves vs supply |
| d | **Regulatory cross-reference** | Every policy clause traces to a CIT-… (99-citations.md) or a direct SFC paragraph |

#### 5.6.2 Weblink per claim (your explicit requirement)

Each validated claim is rendered with a clickable link to a **versioned snapshot of the source** stored in the TAR-app evidence store. The link is permanent: source removal, modification, or relocation does not break the link.

Implementation:

| Aspect | Detail |
|---|---|
| **Storage** | Evidence store holds `sha256:<hash>` content-addressed snapshots of every fetched source |
| **Link format** | `https://tar-app.example.com/evidence/<sha256>` (or local URL during dev) |
| **Lifetime** | Permanent for Class A/B documents; 7 years for Class C/D |
| **Rendering** | Every claim in DD report / monthly report / listing document shows weblink as `[CIT-SFC-G-006 ¶7.6(a)](https://tar-app.example.com/evidence/<sha256>)` |

### 5.7 Recommendation Synthesiser

| Aspect | Detail |
|---|---|
| **eve definition** | `agents/synthesiser/` |
| **Outputs** | DD report (per `30-template-dd-checklist.md`); recommendation |
| **Guardrails** | Cannot override domain lead sign-off; cannot publish without Head of Listing approval |

### 5.8 Listing Document Generator

| Aspect | Detail |
|---|---|
| **eve definition** | `agents/listing-doc/` |
| **Skills** | Schedule 2 verbatim-block injection; Section-by-section content scaffolding per `33-template-listing-document.md` |
| **Guardrails** | Cannot alter Schedule 2 verbatim block (CIT-SFC-G-020); cannot publish without Head of Listing approval |

### 5.9 Gap Analysis Agent (the central feature you asked for)

| Aspect             | Detail                                                                                                               |
| ------------------ | -------------------------------------------------------------------------------------------------------------------- |
| **eve definition** | `agents/gap-analysis/`                                                                                               |
| **Trigger**        | Continuous — runs on every agent action that uses a framework, score, label, or procedure                            |
| **Skills**         | Catalog of HKbitEX-approved frameworks (extracted from docs/02..07 + LR v3.1 + TAP v1.0); comparison logic           |
| **Tools**          | HKbitEX corpus lookup; framework fingerprinting; gap-record writer                                                   |
| **Outputs**        | Gap record (gap ID; reference doc + section; framework description; HKbitEX source status; suggested action; status) |
| **Workflow**       | Routes gap record to gap-analysis module → manager review → accept/reject (see `41-…` §4)                            |

#### 5.9.1 Pre-loaded gaps (seeded from policy docs)

When TAR-app is first launched, the gap-analysis module is pre-populated with **85 gaps across 12 categories**. Each gap is a framework that TAR-app uses (or may use) that is **NOT present in HKbitEX's approved policies** (`docs/00-07`). The TAR-app gap-analysis workflow (see `41-…` §4) routes each gap to managers for review → accept (becomes HKbitEX policy via Board approval) or reject (framework removed).

**Full gap inventory:**

| Gap category | Count | HKbitEX anchor (what's actually in HKbitEX docs) |
|---|---|---|
| **GAP-COI-001..011** | 11 | HKbitEX TAP §2.3.5 + §4.1.6 + LR Chapter 3 (TARC member COI declaration + PAD + abstention counting toward quorum) |
| **GAP-TARC-001..008** | 8 | HKbitEX TAP Appendix B (TARC Terms of Reference) + LR Chapter 3 |
| **GAP-AUDIT-001..010** | 10 | HKbitEX TAP §3.4 + LR 2.2 (smart contract audit factor) |
| **GAP-ADM-001..007** | 7 | HKbitEX TAP §3-§5 + LR Chapter 6 (admission procedure) |
| **GAP-SUS-001..006** | 6 | HKbitEX LR 8 + TAP §7 (halt/suspension/delisting procedure) |
| **GAP-INC-001..011** | 11 | HKbitEX LR 8.1 + TAP §6.3.4 + §7.2 + §8.5 (incident escalation fragments) |
| **GAP-MON-001..011** | 11 | HKbitEX TAP §6.3 (daily + monthly monitoring) |
| **GAP-DDC-001..003** | 3 | HKbitEX TAP §3 + Retail Appendix 2 (DD structure) — see `30-template-dd-checklist.md` §11 |
| **GAP-MR-001..004** | 4 | HKbitEX TAP Appendix D + 4010 (monthly report structure) — see `31-template-monthly-report-hol.md` §14 |
| **GAP-SCD-001..007** | 7 | HKbitEX TAP §3.4 (audit acceptance) — see `32-template-smart-contract-dd.md` §10 |
| **GAP-LD-001..005** | 5 | HKbitEX TAP §5 + Schedule 2 (listing document structure) — see `33-template-listing-document.md` gap section |
| **GAP-IS-001..002** | 2 | HKbitEX Review of Information Sources §2 (criteria) — see live gap log (TAR-app runtime) |
| **Total** | **85** | |

Each individual gap ID is detailed in the per-doc gap addenda at the end of the relevant `docs/` file (where applicable — `30`, `31`, `32`, `33` currently host their own; others live in the TAR-app gap log at runtime in MongoDB Atlas).

**Internal contradiction (separate from framework gaps):**

- **GAP-PI-001** (HIGH PRIORITY): HKbitEX TAP §4.1.3 states that the Platform admits VAs "which apply to PI Client only" (i.e., PI-only admission). The 4010 monthly report (`docs/03-…`) lists BTC and ETH as available to "Professional + Retail" (i.e., retail access). Either the policy is wrong or the operational listing was unauthorised. This contradiction:
  - Affects every monthly report (`31-…`)
  - Affects admission classification at every admission
  - Affects the retail/PI gating policy (which itself is a gap since HKbitEX has no standalone retail/PI gating doc — only TAP §4.1.3's PI-only admission rule)
  - **Action required:** resolve before TAR-app gap-analysis workflow is built, since the gap-analysis module cannot reconcile a contradiction in HKbitEX's own docs.

**Why a gap log and not an inline section per doc:**

TAR-app's gap log is the **single source of truth** for all framework gaps. It is updated continuously by the Gap Analysis Agent (§5.9). Per-doc gap tables (e.g., `30-template-dd-checklist.md` §11) are **static snapshots** that ship with each doc version; the live gap log in MongoDB Atlas always has the latest state. Managers and the Head of Listing reconcile the two at every policy-doc revision.

#### 5.9.2 Trigger model (multi-trigger, not single-trigger)

The Gap Analysis Agent runs on **five distinct triggers**. Each catches a different class of gap; together they cover the full surface area.

| # | Trigger | What it catches | Source of trigger | When it runs |
|---|---|---|---|---|
| 1 | **Continuous (per agent action)** | New framework introduced by code change or new agent capability | Auto (eve.dev hook) | Every time any TAR-app agent invokes a framework |
| 2 | **Regulatory change** | SFC VATP Guidelines revision; new SFC circular; SFO/AMLO amendment; licensing-condition variation | SFC scraper + HKbitEX Legal team | On detection of any regulatory text change |
| 3 | **HKbitEX internal policy change** | Listing Rules / TAP / Appendix 3 / Review of Information Sources / other HKbitEX docs updated | Head of Listing (or delegated) signals via TAR-app | On commit to private repo / explicit trigger |
| 4 | **Case-event** | New admission application, TARC decision (especially overturn), material event surfaced | Case lifecycle | On each case state transition |
| 5 | **Periodic scheduled** | Drift, accumulated gaps, internal contradiction re-check | TAR-app scheduler | Quarterly (lightweight) + annual (comprehensive) |
| 6 | **Manual** | Ad-hoc manager-initiated review of a specific framework / doc area | Manager via TAR-app UI | On demand |

Any one trigger alone misses things:

- **Manual only** → framework drift accumulates silently; no-one remembers to check
- **Regulatory change only** → misses internal HKbitEX policy drift and new agent capabilities
- **Continuous only** → too noisy; slow on every agent action; misses "missing entirely" frameworks (agent never invokes them)

The five triggers together cover all four gap-creation vectors.

##### 5.9.2.1 Trigger 1 — Continuous (per agent action)

Every agent action that uses a framework not in the HKbitEX-approved list triggers a gap record:

```
on agent_action(framework):
    if framework IN HKbitEX_approved_policies:
        proceed
    else:
        emit_gap_record(
            gap_id=auto,
            reference_doc=current_doc,
            reference_section=current_section,
            framework=framework,
            hkbitex_source_status="Not in HKbitEX docs",
            suggested_action="Manager review via TAR-app gap-analysis",
            trigger_source="continuous"
        )
        proceed (with framework flagged as advisory)
```

**Performance guardrail:** the HKbitEX-approved framework lookup uses a Bloom filter backed by the versioned corpus, so per-action lookup is < 1ms.

##### 5.9.2.2 Trigger 2 — Regulatory change

Sources of regulatory text change that TAR-app monitors:

| Source | Method | Cadence |
|---|---|---|
| SFC VATP Guidelines page | Hash check + content diff | Daily |
| SFC circulars page | Hash check + content diff | Daily |
| SFO / AMLO / C(WUMP)O amendments | HKbitEX Legal team publishes updates to TAR-app | On receipt |
| HKbitEX licensing-condition variations | Head of Listing publishes via TAR-app | On variation |
| Stablecoins Ordinance updates | HKbitEX Legal team | On receipt |

On detecting a change:

```
on regulatory_text_changed(source, old_version, new_version):
    trigger_id = hash(source + new_version)
    affected_sections = diff_extract_affected_sections(old_version, new_version)
    for each affected_section:
        emit_gap_record(
            gap_id=f"GAP-REG-{trigger_id}-{seq}",
            reference_doc=source,
            reference_section=affected_section,
            framework="Updated SFC / HKbitEX regulatory text",
            hkbitex_source_status="Updated",
            suggested_action="Re-run gap analysis; flag any new framework divergence",
            trigger_source="regulatory_change"
        )
    notify Head of Listing + Compliance via Slack/email
```

##### 5.9.2.3 Trigger 3 — HKbitEX internal policy change

When HKbitEX updates an internal policy (LR v3.1, TAP v1.0, monthly report template, etc.), the change is reflected in the private repo first; TAR-app detects the change via:

- Git webhook on the private repo (push event)
- OR explicit signal from Head of Listing via TAR-app UI

On detection:

```
on hkbitex_policy_changed(doc, old_version, new_version):
    # Re-evaluate all gaps that reference this doc
    affected_gaps = gap_log.find({reference_doc: doc})
    for each gap in affected_gaps:
        if new_version resolves gap:
            gap.status = "POTENTIALLY_RESOLVED"
            gap.resolved_by_version = new_version
            notify_manager(gap)
        else:
            gap.framework_version_at_last_check = new_version
            gap.status = "RECHECK_NEEDED"
            notify_manager(gap)
    # Emit new gaps for any new framework introduced by the doc update
    new_frameworks = extract_frameworks(new_version) - extract_frameworks(old_version)
    for each new_framework in new_frameworks:
        emit_gap_record(
            gap_id=auto,
            reference_doc=doc,
            reference_section=...,
            framework=new_framework,
            hkbitex_source_status="Newly added in HKbitEX policy update",
            suggested_action="Review",
            trigger_source="policy_change"
        )
```

##### 5.9.2.4 Trigger 4 — Case-event

Case-lifecycle events that may surface gaps:

| Event | Action |
|---|---|
| New admission application logged | Run gap analysis on the specific applicant/asset area (e.g., "stablecoin admission") |
| TARC vote (any outcome) | If vote is a rejection or significant override, flag as potential framework gap |
| Material event in monitoring | If the response required a framework not in HKbitEX docs, emit gap record |
| Discrepancy Agent flags Class E (genuine contradiction) | Emit gap record (the contradiction may indicate a policy gap) |

##### 5.9.2.5 Trigger 5 — Periodic scheduled

| Cadence | Scope | Purpose |
|---|---|---|
| **Quarterly** | All 85 pre-loaded gaps + any new gaps since last review | Catch framework drift; refresh gap status |
| **Annual** | Comprehensive re-audit + SFC + HKbitEX policy corpus re-fingerprint | Annual policy review per [CIT-SFC-G-001(e)] + HKbitEX TAP §2.3.1(e) |

##### 5.9.2.6 Trigger 6 — Manual

Any user with the **Manager** role or higher can trigger gap analysis on demand:

- **By document**: re-scan `docs/02-07` against current TAR-app behaviour
- **By framework**: re-scan all usages of a specific framework (e.g., "all uses of ST Label system")
- **By case**: re-scan a specific case file
- **By category**: e.g., "re-scan all GAP-COI-* gaps"

Manual triggers are logged; outputs go through the standard gap-analysis workflow (review → accept/reject).

### 5.10 Monitoring Agents (post-listing per HKbitEX TAP §6.3)

| Agent | Cadence | Function | HKbitEX anchor |
|---|---|---|---|
| Continuous Alert Agent | Real-time | On-chain anomaly; sanctions deltas; exchange surveillance | TAP §6.3.2 (daily monitoring) |
| Daily Screening Agent | Daily | World-Check-One + project website scrape | TAP §6.3.2 |
| Monthly Report Generator | Monthly | Full per-VA report per `31-template-monthly-report-hol.md` | TAP §6.3.2 + Appendix D |

**Guardrails:** Monitoring agents flag only; they do not change ST Labels (gap GAP-MON-003) or trigger TARC votes autonomously.

## 6. Shared state

| Store | Purpose | Tech |
|---|---|---|
| **Case store** | One per application; contains all docs, DD findings, scores, decisions, versions | MongoDB Atlas |
| **Evidence store** | All source documents with versioned snapshots; checksum-verified; serves permanent links | S3 + content addressing |
| **Version store** | All document versions; supports approve/accept workflow (per `42-…`) | Git-backed |
| **Citation registry** | Maps every load-bearing claim → CIT-ID + source URL + access date | MongoDB Atlas; populated from `99-citations.md` |
| **Discrepancy log** | All detected discrepancies; resolution status | MongoDB Atlas |
| **Gap log** | All detected HKbitEX policy gaps; status (Pending / Accepted / Rejected) | MongoDB Atlas |
| **Agent memory** | Per-agent context (recent actions, conversation state) | Vector store (read-only after session) |
| **Decision log** | Immutable record of every decision (who, when, what, on what basis) | Append-only ledger with hash chain |

## 7. Cross-cutting guardrails

| Guardrail | Implementation |
|---|---|
| No autonomous admission decisions | All admission/decline recommendations require Head of Listing sign-off |
| No autonomous TARC votes | TARC vote is a human event; agents prepare the meeting pack only |
| No autonomous client communications | Drafts only; BD Team + Head of Listing approves |
| No autonomous SFC filings | Drafts only; Head of Listing + CCO signs |
| **Citation required for every claim** | Output validator fails any claim lacking a citation reference |
| **Weblink required for every claim** | Per Validation Agent §5.6.2: system produces versioned snapshot; weblink rendered in output |
| Discrepancy must be surfaced | If two sources disagree, output must include the discrepancy explicitly |
| **Gap must be surfaced** | If framework is not in HKbitEX docs, gap-analysis module is notified |
| Audit trail | Every agent action logged with input, output, version, approver |
| Reversibility | No agent can perform an irreversible action without approval |
| Rate limits | Per-agent limits prevent runaway cost or abuse |
| PII / confidentiality | COI data isolated; access controls per HKbitEX TAP §2.4.2 |

## 8. Deployment topology

| Environment | Purpose |
|---|---|
| **Dev / Sandbox** | All agents; mock external sources; free-form experimentation |
| **Staging / Pre-prod** | Real external sources in read-only mode; no production actions; full audit log |
| **Production** | Real external sources; full agent capabilities; **all guardrails active**; per-user audit trail |

**Access model:**
- Agents run as **services** on Vercel
- Humans interact via the **Listing Department Console** (web UI; integration with email, document management, TARC meeting workflow)
- All actions go through a **policy decision point** that enforces guardrails

## 9. Operating model

| Owner | Responsibility |
|---|---|
| **Head of Listing** | Final approval on every admission-related output; final accept/reject on gaps; owner of `42-…` versioning rules |
| **CCO** | Override authority on compliance-related agent actions |
| **Head of Risk Management** | Override authority on risk-related agent actions |
| **CTO** | Agent platform reliability; incident response |
| **Engineering** | Build + operate the agent platform |
| **TARC** | Receives agent-prepared meeting packs; votes; binding decisions |

## 10. Metrics

| Metric | Target |
|---|---|
| Admission cycle time (inquiry → listing) | Reduced from baseline |
| DD analyst hours per application | Reduced |
| Discrepancy detection rate | ≥ 95% of known sources-agree / disagree cases correctly classified |
| Gap-detection rate | Every use of non-HKbitEX framework flagged |
| Citation coverage (load-bearing claims) | 100% |
| Weblink coverage (per claim) | 100% |
| Audit-trail completeness | 100% |

## 11. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Agent hallucination in DD report | Citation + weblink-required output validation; sample audit by humans |
| Bias from training data | Diverse source coverage; explicit discrepancy + gap surfacing |
| Insider misuse (insider trading via early signal) | PAD integration (per HKbitEX TAP §4.1.6); access logging; alerting on case-data exfiltration |
| Regulatory drift (SFC rule change) | Quarterly review of citation registry against SFC website; auto-flag if `last_verified` > 6 months |
| Single-LLM-provider failure | Multi-provider fallback for critical workflows |
| Vendor lock-in | LLM-agnostic interface; portable data model |

## 12. Implementation roadmap (high-level — see `41-…` for detail)

| Phase | Scope | Status |
|---|---|---|
| **0 — Foundations** | Document ingest pipeline; citation registry; evidence store; human-only DD continues | **Design only** |
| **1 — Co-pilot** | Listing Document Generator, Intake Agent, DocClass & Extract Agent | **Design only** |
| **2 — DD automation** | 6 DD Analyst Agents + Smart Contract Audit Agent + Discrepancy Resolution + Validation Agent | **Design only** |
| **3 — End-to-end** | Recommendation Synthesiser + full workflow + Gap Analysis Agent + Validation Agent at scale | **Design only** |
| **4 — Monitoring** | Monitoring Agents; monthly report automation | **Design only** |
| **5 — Continuous improvement** | Feedback loop from TARC outcomes | **Design only** |

## 13. Cross-references

- `41-ai-agent-workflows.md` — workflow specs (including gap-analysis, discrepancy, validation)
- `42-ai-agent-document-versioning.md` — staging version control + approval/accepting-changes flow
- `00-legal-framework-hk-sfc.md` — SFC binding regulation
- `99-citations.md` — citation registry
- HKbitEX source docs (`01-…` through `07-…`) — extracted text used as ground truth
