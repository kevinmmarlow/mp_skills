---
name: angellist-architecture
description: Cross-repo architecture map for AngelList's core services - venture (Rails investments monolith), nova (TS/Next.js portal), ipseity (investor identity/KYC), treasury (banking/ledger) - covering API boundaries, REST/Prophet/NATS/Temporal mechanisms, callback patterns, client gems, and TypeSpec SDKs. ALWAYS use when working in any of these four repos, or when a task mentions cross-service calls, API boundaries, Prophet, NATS, Wedge Products API, TypeSpec SDKs, entity_api/treasury_api gems, Temporal workflows, or investor/fund/KYC/money-movement domains.
---

# AngelList Architecture

## Repos & Relationships

- **venture**: Ruby/Rails monolith. System of record: investments, fundraising, fund admin. Exposes REST (Wedge Products API), GraphQL, Prophet/NATS RPC.
- **nova**: TypeScript monorepo: Next.js frontend + GraphQL gateway (Pothos/Yoga). Primary user-facing portal.
- **ipseity**: Ruby/Rails. Investor identity, KYC/AML, accreditation, PII. "Where investor data lives."
- **treasury**: Ruby/Rails. Banking, ledger, money movement. "Where investor money lives."

## API Boundaries

| Caller → Callee | Mechanism | Detail |
|---|---|---|
| nova → venture | REST + Prophet/NATS | TypeSpec SDK → `/wedge_products/api/v1/*`; NATS for data-room/entity ops |
| nova → ipseity | Prophet/NATS + REST | `Prophet.client('ipseity')`; TypeSpec SDK for newer endpoints |
| nova → treasury | Prophet/NATS + REST | `Prophet.client('treasury')`; subscribes to `treasury` NATS stream for async events |
| venture → ipseity | REST | `entity_api` gem → Protobuf endpoints |
| venture → treasury | REST | `treasury_api` gem → Protobuf endpoints |
| venture ↔ nova | REST + Prophet/NATS | Venture calls Nova at `/internal/sync/*`; Nova calls Venture's Wedge API |
| ipseity → venture | REST callbacks | POSTs to `/ipseity/callbacks/*` on entity/accreditation changes |
| ipseity → treasury | Temporal | Triggers `UpdateCustomerAccountVerificationWorkflow` on verification changes |
| treasury → venture | REST callbacks | POSTs to `/treasury/callbacks/*` for ACH/wire/ledger events |
| treasury → ipseity | REST | `entity_api` gem for entity lookups |

## Key Patterns

- **Callbacks**: Ipseity/Treasury push state changes to Venture via callback endpoints (not polling).
- **Prophet/NATS**: Shared service mesh for real-time RPC. Preferred for new endpoints.
- **Ruby client gems**: `entity_api`, `treasury_api` (+ prophet variants) bundled in-repo, Git-sourced.
- **TypeSpec SDKs**: Nova generates typed TS clients (`@angellist/sdk-{venture,ipseity,treasury}`) from TypeSpec defs in `packages/sdk/`.
- **Temporal**: Cross-repo long-running workflows (verification, fund ops).

When touching an API boundary, note affected contracts so the consuming repo can be updated separately.
