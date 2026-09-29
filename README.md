# TAR-app — Token Admission & Review Automation (HKbitEX)

> **Public repository.** Framework, design docs, templates, and SFC citation registry for TAR-app — automation for HKbitEX's token admission, due-diligence, monitoring, and gap-analysis workflows.
>
> **Companion private repo.** HKbitEX-internal governance extracts and live reference material live in [`magicat888/crypto-listing-private`](https://github.com/magicat888/crypto-listing-private).

---

## Three flagship features (currently design-only, pending approval)

| Feature | Detail |
|---|---|
| **Gap-analysis workflow** | Surfaces every use of a framework NOT in HKbitEX's approved policies; routes to manager review → accept (becomes policy) / reject (removed). 85 gaps pre-loaded. |
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

| Category | In this repo? | Sibling repo? |
|---|---|---|
| **i. SFC + Laws** (public regulatory text) | ✅ | n/a |
| **ii. HKbitEX Policies** (internal governance) | ❌ | ✅ Private |
| **iii. Working Templates** (DD + reporting) | ✅ | n/a |
| **system** (TAR-app architecture) | ✅ | n/a |

## Reading order

1. `TAR-app/docs/00-legal-framework-hk-sfc.md` — what SFC requires
2. `TAR-app/docs/40-ai-agent-design.md` — TAR-app architecture
3. `TAR-app/docs/41-ai-agent-workflows.md` — workflows (gap-analysis in §4; COI gate in §2.2; validation agent in §6)
4. `TAR-app/docs/42-ai-agent-document-versioning.md` — staging + accept/reject flow
5. `TAR-app/docs/30-…` through `33-…` — working templates

## Status

- ✅ HKbitEX-strict policy extracts (private repo only)
- ✅ SFC public framework (this repo)
- ✅ Templates + system design + COI gate workflow (this repo)
- ⏳ Implementation: **pending approval of the system design**

## License

TBD by HKbitEX.

## Related

- Private repo with HKbitEX-internal extracts: [`magicat888/crypto-listing-private`](https://github.com/magicat888/crypto-listing-private)
