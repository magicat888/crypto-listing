# TAR-app — Token Admission & Review Automation (HKbitEX)

> **Public repository.** Framework, design docs, templates, and SFC citation registry for TAR-app.
>
> **Companion private repo.** HKbitEX-internal governance extracts: [`magicat888/crypto-listing-private`](https://github.com/magicat888/crypto-listing-private).

---

## Three flagship features (design-only, pending approval)

| Feature | Detail |
|---|---|
| **Gap-analysis workflow** | Surfaces every use of a framework NOT in HKbitEX's approved policies; routes to manager review → accept (becomes policy) / reject (removed). 85 gaps pre-loaded. Six triggers. |
| **Staging version control** | Word-style "track changes + accept/reject" applied programmatically; tamper-evident audit log. |
| **Validation agent with weblinks** | Every DD claim gets a clickable weblink to a permanent versioned snapshot of the source. |

## Architecture

- **Agent framework:** [eve.dev](https://eve.dev)
- **LLM backend:** MiniMax
- **Frontend:** Next.js + Tailwind + shadcn/ui on Vercel
- **Data:** MongoDB Atlas + Pinecone + S3
- **Auth:** Corporate SSO

See `TAR-app/docs/40-ai-agent-design.md`.

## Document categories

| Category | In this repo? | Sibling? |
|---|---|---|
| **i. SFC + Laws** | ✅ | n/a |
| **ii. HKbitEX Policies** | ❌ | ✅ Private |
| **iii. Working Templates** | ✅ | n/a |
| **system** (TAR-app architecture) | ✅ | n/a |

## Reading order

1. `TAR-app/docs/00-legal-framework-hk-sfc.md` — SFC requirements
2. `TAR-app/docs/40-ai-agent-design.md` — architecture + 6-trigger gap model
3. `TAR-app/docs/41-ai-agent-workflows.md` — workflows (gap-analysis §4; COI gate §2.2; validation §6)
4. `TAR-app/docs/42-ai-agent-document-versioning.md` — staging + accept/reject
5. `TAR-app/docs/30-…` through `33-…` — templates (incl. Path A/B audit selection in `32-…`)

## Status

- ✅ HKbitEX-strict policy extracts (private repo only)
- ✅ SFC public framework (this repo)
- ✅ Templates + system design (this repo)
- ⏳ Implementation: **pending approval of the system design**

## Related

- Private repo: [`magicat888/crypto-listing-private`](https://github.com/magicat888/crypto-listing-private)
