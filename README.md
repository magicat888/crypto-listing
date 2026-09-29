# TAR-app — Token Admission & Review Automation (HKbitEX)

> **Public repository.** Framework, design docs, templates, and SFC citation registry for TAR-app — an automation system for HKbitEX's token admission, due-diligence, monitoring, and gap-analysis workflows.
>
> **Companion private repo.** HKbitEX-internal governance extracts and live reference material live in [`magicat888/crypto-listing-private`](https://github.com/magicat888/crypto-listing-private).

---

## What is TAR-app?

TAR-app is the automation layer for HKbitEX's Listing Committee (TARC) workflows, built on HK SFC VATP regulatory requirements. It produces, validates, and tracks all artefacts needed for token admission, ongoing monitoring, suspension, and delisting.

Three flagship features (currently design-only, pending approval):

| Feature | Detail |
|---|---|
| **Gap-analysis workflow** | Surfaces every use of a framework NOT in HKbitEX's approved policies; routes to manager review → accept (becomes policy) / reject (removed). ~80 gaps pre-loaded from policy docs. |
| **Staging version control** | Word-style "track changes + accept/reject" applied programmatically to every artefact, with named approvers and tamper-evident audit log. |
| **Validation agent with weblinks** | Every DD report claim gets a clickable weblink to a permanent versioned snapshot of the source — for easy human verification. |

## Architecture (high-level)

- **Agent framework:** [eve.dev](https://eve.dev) (durable AI agents)
- **LLM backend:** MiniMax
- **Frontend:** Next.js + Tailwind + shadcn/ui on Vercel
- **Data:** MongoDB Atlas + Pinecone (vectors) + S3 (versioned snapshots)
- **Auth:** Corporate SSO (single-tenant HKbitEX)

See `TAR-app/docs/40-ai-agent-design.md` for full architecture.

## Document categories

| Category | Description | In this repo? | Sibling repo? |
|---|---|---|---|
| **i. SFC + Laws** | Public regulatory text | ✅ Yes | n/a |
| **ii. HKbitEX Policies** | HKbitEX-internal governance (LR v3.1, TAP v1.0, monthly report, LC minutes, Review of Information Sources) | ❌ No | ✅ Private |
| **iii. Working Templates** | DD checklist, monthly report, smart-contract DD, listing document | ✅ Yes (with HKbitEX-strict gap addenda) | n/a |
| **system** | TAR-app architecture + workflows + versioning | ✅ Yes | n/a |

## Reading order

1. `TAR-app/docs/00-legal-framework-hk-sfc.md` — what SFC requires
2. `TAR-app/docs/40-ai-agent-design.md` — TAR-app architecture
3. `TAR-app/docs/41-ai-agent-workflows.md` — workflows (gap-analysis in §4; validation agent in §6)
4. `TAR-app/docs/42-ai-agent-document-versioning.md` — staging + accept/reject flow
5. `TAR-app/docs/30-…` through `33-…` — working templates

## Status

- ✅ HKbitEX-strict policy extracts (private repo only)
- ✅ SFC public framework (this repo)
- ✅ Templates + system design (this repo)
- ⏳ Implementation: **pending approval of the system design**
- ⏳ Gap-analysis workflow: **design only** (will be built if approved)

## License

TBD by HKbitEX.

## Related

- Private repo with HKbitEX-internal extracts: [`magicat888/crypto-listing-private`](https://github.com/magicat888/crypto-listing-private)
- HKbitEX (SFC VATP — deemed-to-be-licensed for Type 1 + Type 7): operating platform
