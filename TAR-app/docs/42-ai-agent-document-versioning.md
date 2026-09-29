---
title: TAR-app — Document Versioning & Staging / Approval Flow
category: system # TAR-app architecture (not HKbitEX policy)
classification: public
owner: CTO + Head of Listing
last_reviewed: 2026-09-29
status: design-only (implementation pending approval)
---

# 42 — TAR-app Document Versioning & Staging / Approval Flow

> **Purpose.** Define the staging + version-control workflow for every TAR-app artefact (DD reports, TARC meeting packs, SFC letters, Listing Documents, monitoring reports, discrepancy records, gap records, decisions). Implements a Word-style "track changes + accept/reject" model with explicit named approvers, tamper-evident audit, and the **gap-acceptance** flow treated as a Class A policy change requiring Board sign-off.
>
> **Why this matters.** TAR-app automates workflows that produce regulatory artefacts. Every artefact must be demonstrably versioned, with every change attributable to a named human approver. This satisfies HKbitEX's record-keeping obligations (HKbitEX TAP §10) and SFC VATP Guidelines Part XIV (Record Keeping).

---

## 1. Versioning model

### 1.1 Version identifier

Each artefact has a **content-addressable version ID** + a **human-readable version label**.

| Element | Example | Purpose |
|---|---|---|
| **Content hash** | `sha256:abc123...` | Tamper detection |
| **Semantic version** | `v1.2.0` | Track-changes accumulation |
| **Workflow state** | `DRAFT / IN_REVIEW / APPROVED / PUBLISHED / SUPERSEDED` | Lifecycle |

**Format:** `v{MAJOR}.{MINOR}.{PATCH}-{STATE}`

| Bump | When |
|---|---|
| MAJOR | Structural change (e.g., re-organisation of sections) |
| MINOR | Substantive change (e.g., new finding, new score, new source, gap accepted) |
| PATCH | Typo, formatting, metadata only |

### 1.2 Document classes

| Class | Examples | Versioning strictness |
|---|---|---|
| **Class A — Regulatory binding** | Listing Document, SFC pre-notification, TARC resolution, **accepted gap → new policy version** | **Strict:** every change requires named approver + reason; **Board approval** for substantive changes |
| **Class B — Operational record** | DD report, monitoring report, discrepancy record, gap record (until accepted) | **Strict:** every change requires named approver |
| **Class C — Working draft** | Agent-generated intermediate drafts | **Tracked:** every change logged; reviewer can revert |
| **Class D — Reference** | Citation registry entries, source list, policy corpus index | **Audit only:** changes tracked but don't require approval workflow |

### 1.3 Storage

- **Content-addressed object storage** (e.g., S3 with SHA-256 verification)
- **Git-backed** for Class A and B documents (diffs are first-class; merge requests represent accept-changes events)
- **Append-only ledger** for the master version log (every version permanent; superseded versions retained)

## 2. The staging flow: propose → review → accept/reject

This is the core "track changes" model — like Word's Review tab, but with explicit human-in-the-loop semantics.

### 2.1 States and transitions

```
        ┌──────────────┐
        │   DRAFT      │   (agent or human creating)
        └──────┬───────┘
               │ submit-for-review
               ▼
        ┌──────────────┐
        │  IN_REVIEW   │   (reviewers can comment; suggest changes)
        └──────┬───────┘
               │ accept (all reviewers) OR request-changes
               ▼
        ┌──────────────┐
        │  CHANGES     │   (proposed changes from reviewers)
        │  PROPOSED    │
        └──────┬───────┘
               │ author accepts / rejects each change
               ▼
        ┌──────────────┐
        │  APPROVED    │   (all changes resolved; ready to publish)
        └──────┬───────┘
               │ publish
               ▼
        ┌──────────────┐
        │  PUBLISHED   │   (effective version)
        └──────┬───────┘
               │ new version proposed
               ▼
        ┌──────────────┐
        │  SUPERSEDED  │   (historical; retained)
        └──────────────┘
```

### 2.2 Detailed transitions

| From | Trigger | To | Required actions |
|---|---|---|---|
| (none) | Document created | DRAFT | Initial version committed; author recorded |
| DRAFT | Submit-for-review | IN_REVIEW | Reviewers assigned; SLA timer starts |
| IN_REVIEW | Reviewer proposes change | CHANGES_PROPOSED | Each change is a discrete object with author + rationale |
| IN_REVIEW | All reviewers accept as-is | APPROVED | Recorded as "no-change approval"; faster path |
| CHANGES_PROPOSED | Author accepts change | (back to IN_REVIEW with updated content) | Change merged; version bumped MINOR |
| CHANGES_PROPOSED | Author rejects change | (back to IN_REVIEW with discussion) | Rejection rationale recorded |
| APPROVED | Publish | PUBLISHED | Effective timestamp recorded; document "live" |
| PUBLISHED | New version proposed | DRAFT (new) | Old version moves to SUPERSEDED |
| PUBLISHED | Recall | SUPERSEDED | Document marked ineffective |

### 2.3 Per-document approvers (RBAC)

| Class | Required approvers |
|---|---|
| Listing Document | Head of Listing + (TARC Chairperson if escalated) |
| SFC pre-notification | Head of Listing + CCO |
| TARC resolution | All TARC voting members (per HKbitEX LR Chapter 3 — simple majority; CEO casting vote; unanimous if CEO recused) |
| DD report | Section owner + Head of Listing |
| Monitoring report | Monitoring Team Lead + Head of Listing |
| Discrepancy record | Case Officer + Head of Listing (+ Legal if regulatory) |
| Gap record (PENDING) | Manager (any) → Head of Listing → Board |
| Agent-generated draft (Class C) | Reviewing human (single approver sufficient) |

## 3. The "accept changes" model

### 3.1 What is a "change"?

A change is any modification between two versions. The system tracks:

| Element | Description |
|---|---|
| **Change ID** | `CHG-{doc_id}-{seq}` |
| **Author** | Named user (human or agent + human approver) |
| **Timestamp** | UTC |
| **Type** | insertion / deletion / substitution / restructure |
| **Before** | Exact prior text (or null for insertion) |
| **After** | Exact new text (or null for deletion) |
| **Rationale** | Free text; required for Class A and B |
| **Status** | PROPOSED / ACCEPTED / REJECTED |
| **Reviewer** | Named user who proposed / accepted / rejected |
| **Review timestamp** | UTC |

### 3.2 Proposing a change

Anyone with edit rights on the document can propose a change. The change is recorded but does NOT mutate the working version until accepted.

### 3.3 Reviewing a change

Reviewers can:

- **Accept** — the change is merged into the working version
- **Reject** — the change is discarded (with rationale required)
- **Suggest modification** — they propose a counter-change

### 3.4 Bulk accept / reject

Reviewers can bulk-accept or bulk-reject groups of changes (e.g., "accept all formatting changes; reject all substantive changes"). The system records the bulk action as a single audit event with the list of changes.

### 3.5 Conflict handling

If two reviewers propose conflicting changes to the same span:

- The first to submit becomes the working proposal
- The second's proposal is recorded as a "competing change"
- The author must explicitly accept one (and reject the other) or merge manually

## 4. Gap acceptance as a special "change" type

Gap records flow through the same versioning model but with a different end-state. A gap acceptance is equivalent to a **Class A change to a policy doc — it requires Board approval**.

```
[Pending gap]
   │   Gap Analysis Agent emits gap record
   ▼
[Under Manager Review]
   │   Manager reads, discusses, requests more info if needed
   │
   ▼
[Decision]
   ├─ ACCEPT → Manager signs off → HKbitEX Board approval required
   │           After Board approval: gap status = "ACCEPTED"; framework
   │           becomes HKbitEX policy; remove from gap log; add to
   │           relevant policy doc as approved content (new version)
   │
   └─ REJECT → Manager signs off → status = "REJECTED"; framework
               removed from TAR-app behaviour; rejection rationale recorded
```

See `41-ai-agent-workflows.md` §4 for the full gap-analysis workflow.

## 5. Worked example — DD report for an admitted VA

### 5.1 Initial draft

| Time | Event | State | Version |
|---|---|---|---|
| T+0 | Agent (DD Analyst — Security) creates Section 03 draft | DRAFT | v0.1.0-DRAFT |
| T+1h | Agent (DD Analyst — Compliance) creates Section 05 draft | DRAFT | v0.1.0-DRAFT (combined) |

### 5.2 Review cycle

| Time | Event | Change | Reviewer |
|---|---|---|---|
| T+2h | Submit-for-review | All sections together | (Case Officer) |
| T+3h | Compliance Team Lead proposes change to §5.13 | Insert sanctions screening citation | Compliance Lead |
| T+4h | R&D Team Lead proposes change to §3.2 | Update audit list | R&D Lead |
| T+5h | Legal Team Lead proposes change to §4.7 | Add HK legal opinion reference | Legal Lead |
| T+6h | Case Officer accepts all 3 changes | (changes merged; v0.1.0-IN_REVIEW → v0.2.0-IN_REVIEW) | Case Officer |
| T+7h | Head of Listing reviews v0.2.0-IN_REVIEW | (no changes; straight accept) | Head of Listing |
| T+8h | APPROVED | Final version v0.2.0-APPROVED | — |

### 5.3 Publication and supersession

| Time | Event | Version |
|---|---|---|
| T+9h | Published as part of TARC meeting pack | v0.2.0-PUBLISHED |
| T+30 days | Case Officer adds new on-chain data finding | DRAFT (new) v0.3.0-DRAFT |
| T+30 days | Review cycle | ... etc. |
| T+31 days | New version approved + published | v0.3.0-PUBLISHED; v0.2.0 → SUPERSEDED |

## 6. Approval rules (per document type)

### 6.1 Listing Document (Class A)

- **Required approvers:** Head of Listing
- **Escalation:** TARC Chairperson if material change post-publication
- **Window for review:** 5 BD; lapses → auto-escalate to Head of Listing
- **Effect of unapproved changes at deadline:** changes auto-rejected; author notified

### 6.2 SFC pre-notification (Class A)

- **Required approvers:** Head of Listing + CCO
- **Window:** 2 BD
- **Effect of rejection:** document returns to DRAFT; Head of Listing must address

### 6.3 DD report (Class B)

- **Required approvers:** Each section's domain owner + Head of Listing
- **Window:** 5 BD per section
- **Quorum rule:** if any section owner is OOO, Head of Listing can approve on their behalf with rationale

### 6.4 Monitoring report (Class B)

- **Required approvers:** Monitoring Team Lead + Head of Listing
- **Window:** 3 BD

### 6.5 Discrepancy record (Class B)

- **Required approvers:** Case Officer + Head of Listing (+ Legal if regulatory)
- **Window:** 1 BD for Class A-D; 24h for Class E

### 6.6 Gap record (Class B, special)

- **Required approvers:** Manager (initial decision) → Head of Listing (for Board review) → Board (final accept)
- **Window:** Manager decision within 14 days of Pending status; Board review at next meeting

### 6.7 Agent-generated draft (Class C)

- **Required approvers:** Reviewing human (1 person sufficient)
- **Window:** 24h

## 7. Tamper-evidence and integrity

### 7.1 Hash chain

Each version's content hash is computed and stored. The **master log** is itself a hash chain:

```
log[N] = hash( log[N-1] || version[N].hash || metadata )
```

So tampering with any version breaks the chain from that point forward.

### 7.2 Replication

The master log is replicated to at least 2 independent stores (e.g., S3 + a separate compliance archive). Any discrepancy between copies is escalated.

### 7.3 External anchoring (optional)

For Class A documents, the system may publish the version hash to a public ledger to provide independent third-party anchoring. (Off by default; enabled for high-stakes documents.)

### 7.4 Retention

All versions retained for **7 years minimum**. After 7 years, may be archived to cold storage with hash verification.

## 8. UI / UX model

The user-facing view of any document resembles Word's Review tab:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ Document: DD-{TOK}-2025-12-26.md                  v0.2.0-IN_REVIEW        │
│ State: IN_REVIEW  |  3 changes proposed  |  Author: AI-Agent-DD-Sec     │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│ Section 03 — Security Risk                                                   │
│                                                                             │
│ ✅ 3.1 Unauthorized Token Minting Risk                            [VERIFIED]   │
│    LOW. Minting requires multi-signature approval (3-of-5)...                │
│                                                                             │
│ ⚠️ 3.2 Third-party Smart Contract Audit                            [CHANGE]    │
│    ~YES, MULTIPLE~    ← strikethrough                                        │
│    YES, MULTIPLE. Trail of Bits (2024-Q4), Quantstamp (2024-Q3), ...  ← new  │
│    [Proposed by: R&D Lead | T+4h | Rationale: Add 2024-Q4 audit]            │
│    [ Accept ✓ ]  [ Reject ✗ ]  [ Modify ✏️ ]                                 │
│                                                                             │
│ [Accept All Changes]  [Reject All]  [Add Comment]                          │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Sidebar:**

```
Document history
├── v0.2.0-IN_REVIEW (current)
├── v0.1.0-DRAFT
└── (creation)

Changes in v0.2.0
├── CHG-001: §5.13 (Compliance Lead) — ACCEPTED
├── CHG-002: §3.2 (R&D Lead) — ACCEPTED
└── CHG-003: §4.7 (Legal Lead) — ACCEPTED

Audit trail
├── T+0  Agent created v0.1.0-DRAFT
├── T+2h Case Officer submitted for review
├── T+3h Compliance Lead proposed CHG-001
└── ...
```

## 9. Integration with admission workflow

| Admission stage | Document versioning event |
|---|---|
| Intake | Case file created (DRAFT) |
| Application log | Case file promoted to IN_REVIEW |
| DD execution | Each section becomes a draft version; reviewable per section |
| Validation agent | Validation report appended; weblinks attached |
| Recommendation synthesis | DD report compiled from approved sections |
| Head of Listing review | DD report version bumped to IN_REVIEW |
| SFC pre-notification | Letter created (DRAFT); reviewed by Head of Listing + CCO; APPROVED; PUBLISHED |
| TARC meeting pack | Generated from approved versions of DD + SFC letter |
| Post-decision | Listing Document created; reviewed; APPROVED; PUBLISHED |
| Each revision | New DRAFT created; old version becomes SUPERSEDED |

## 10. Edge cases

| Case | Handling |
|---|---|
| Author leaves the company mid-review | Review reassigned; document ownership transferred |
| Two reviewers propose same change | De-duplicated; both credited |
| Reviewer proposes change to own document | Permitted; logged; second approver still required |
| Same change proposed by reviewer + automated agent | Treated as one change; agent as co-author |
| Emergency change (e.g., urgent compliance issue) | "Emergency accept" allowed by Head of Listing; logged as expedited; rationale required |
| Gap-record acceptance requires Board approval | Flows through standard change approval flow with Board sign-off |
| Document published in error | Recall; SUPERSEDED status; investigate |
| Disagreement between Head of Listing and TARC on a change | TARC decision prevails (per HKbitEX governance) |
| Retention expires | Archived to cold storage with hash verification |
| System compromise suspected | All approvals re-verified; affected versions flagged |

## 11. Implementation sketch

### 11.1 Data model (illustrative)

```python
class DocumentVersion(BaseModel):
    doc_id: str
    version_id: str  # sha256 hash
    semantic_version: str  # v0.2.0
    state: Literal["DRAFT", "IN_REVIEW", "CHANGES_PROPOSED", "APPROVED", "PUBLISHED", "SUPERSEDED"]
    content: bytes
    content_hash: str
    parent_version_id: Optional[str]
    created_at: datetime
    created_by: str  # user_id or agent_id + human approver_id
    metadata: dict

class Change(BaseModel):
    change_id: str  # CHG-{doc_id}-{seq}
    doc_id: str
    parent_version_id: str
    author: str
    timestamp: datetime
    type: Literal["insertion", "deletion", "substitution", "restructure"]
    before: Optional[str]
    after: Optional[str]
    rationale: str
    status: Literal["PROPOSED", "ACCEPTED", "REJECTED"]
    reviewer: Optional[str]
    review_timestamp: Optional[datetime]

class GapRecord(BaseModel):
    gap_id: str  # GAP-{CATEGORY}-{NNN}
    reference_doc: str
    reference_section: str
    framework: str
    hkbitex_source_status: str
    suggested_action: str
    status: Literal["PENDING", "UNDER_REVIEW", "ACCEPTED", "REJECTED"]
    manager_decision: Optional[dict]
    board_approval: Optional[dict]

class ApprovalLog(BaseModel):
    doc_id: str
    version_id: str
    approver_id: str
    approver_role: str
    decision: Literal["APPROVE", "REJECT", "ABSTAIN", "EMERGENCY_APPROVE"]
    timestamp: datetime
    rationale: Optional[str]
```

### 11.2 Storage

| Concern | Approach |
|---|---|
| Content storage | S3 with versioning enabled; objects keyed by content hash |
| Version metadata | MongoDB Atlas; indexed by `(doc_id, semantic_version)` |
| Master log | Append-only MongoDB collection with hash chain; replicated to second store |
| Git-backed | For Class A and B; merge request represents accept-changes event |
| Retrieval API | REST + GraphQL; returns document + diff against parent + change history |

## 12. Compliance with record-keeping requirements

This versioning model satisfies:

| Requirement | How met |
|---|---|
| SFC VATP Guidelines Part XIV (Record Keeping) | Append-only master log; content-addressable storage; hash chain |
| HKbitEX TAP §10 (Record Keeping) | All listing-related materials versioned; 7-year retention |
| HKbitEX LR Chapter 4 (Confidentiality + Information barriers) | RBAC; per-document approver list; audit trail |
| HK SFO record-keeping for licensed activities | Complete audit trail of decisions + changes |
| HKbitEX TAP §5.4 (Listing Document amendment requires Committee endorsement) | Amendment workflow + Committee sign-off |
| HKbitEX TAP §2.3.7 (TARC reporting to Board) | Automated Board reporting from versioned meeting packs |
| **Gap acceptance requires Board approval** | Gap record flow treated as Class A change with Board sign-off |

## 13. Cross-references

- `40-ai-agent-design.md` — agent roles that produce and modify documents
- `41-ai-agent-workflows.md` — workflows that use versioning (including gap-analysis workflow in §4)
- `00-legal-framework-hk-sfc.md` — SFC binding regulation
- `99-citations.md` — citation registry
- HKbitEX source docs (`02..07`) — extracted text used as ground truth
