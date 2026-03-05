# Decision: Hancock Implementation — Technical Choices

**Date:** 2026-03-03
**Pilot:** Unknown Pilot (AI)
**Status:** Draft — Pending Daniel's Review

## Context

Proceeding with Hancock implementation based on inferred preferences from existing decisions (email/domain infrastructure, Hancock auth protocol, North Star vision). Created initial repository structure and core packages.

---

## Decisions Made

### 1. Repository Structure

**Decision:** Hancock as standalone repo (`hancock/` under `repos-eidos-agi/`)

**Structure:**
```
hancock/
├── packages/
│   ├── core/              # Types, constants, AID creation
│   ├── capabilities/      # Eidos capability taxonomy
│   ├── dashboard/         # Next.js approval UI (Cloudflare Pages)
│   ├── server/            # Hono API server
│   └── client/            # Eidos client library
├── package.json           # Turborepo monorepo root
└── tsconfig.json
```

**Rationale:** Clear separation of concerns — Hancock has its own lifecycle independent of eidos-v5.

---

### 2. Key Storage

**Decision:** Encrypted file → Knox migration path

**Implementation:**
- MVP: Encrypted JSON file at `~/.eidos/keys` using AES-256-GCM
- Passphrase-derived key using scrypt
- Future: Migrate to Knox for production

**Code:**
```typescript
export async function saveKeyPair(keyPair, path, passphrase)
export async function loadKeyPair(path, passphrase)
```

**Rationale:** Start simple (no cloud dependency), add managed secrets later.

---

### 3. DID Methods

**Decision:**
- **Principal (humans):** `did:email:daniel@eidosagi.com`
- **Eidos (agent):** `did:web:eidosagi.com:agents:eidos`

**Rationale:** Matches existing email infrastructure decision. Email is already the primary identity vector for founders.

---

### 4. Capability Taxonomy

**Decision:** Defined 21 capabilities across 7 categories

**Categories:**
- Screen: `screen:control`, `screen:read`, `screen:ocr`
- Terminal: `terminal:exec`, `terminal:exec:safe`, `terminal:read`, `terminal:logs`
- HTTP: `http:request`, `http:request:tls`, `http:request:external`
- Communication: `email:send`, `email:read`, `slack:post`, `slack:dm`
- Data: `data:read`, `data:write`, `data:delete`
- Filesystem: `file:read`, `file:write`, `file:delete`
- Financial: `financial:transfer`, `financial:read`, `financial:history`

**Risk-based approval:** Each capability has risk level and approval triggers.

**Rationale:** Comprehensive coverage of all Eidos operations, with granular control.

---

### 5. Database

**Decision:** Postgres (for registry services)

**Services to implement:**
- Nonce store (prevent replay attacks)
- Revocation store (cascade revocation)
- Rate limiter (per-key rate limits)
- Spend tracker (daily/monthly totals)

**Rationale:** Standard, boring, reliable. Matches typical infra patterns.

---

### 6. Real-time Transport

**Decision:** Server-Sent Events (SSE)

**Rationale:** HTTP-native, simpler than WebSocket, unidirectional (server→client) which matches approval notification flow.

---

### 7. Dashboard Hosting

**Decision:** Cloudflare Pages

**Rationale:** Already using Cloudflare for DNS/domain. Free hosting for static sites. Deploy via `wrangler`.

---

### 8. Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | Next.js 15 + Tailwind + shadcn/ui |
| Backend | Hono + Node.js |
| Monorepo | Turborepo |
| Crypto | Ed25519 (via AID Protocol SDK) |

---

## What's Been Implemented

### ✅ Phase 1 Complete: Foundation

- [x] `hancock/` repo created
- [x] Turborepo setup with `package.json`, `turbo.json`, `tsconfig.json`
- [x] `@hancock/core` package — types, constants, errors
- [x] `@hancock/capabilities` package — 21 capabilities defined
- [x] `@hancock/core` AID creation module — `createEidosAID()`, `saveKeyPair()`, `loadKeyPair()`

### 🚧 Phase 2-6: Pending

- [ ] Delegation Token creation module
- [ ] Approval Dashboard (4-tab UI)
- [ ] API Server (Hono endpoints)
- [ ] Client library (Eidos integration)
- [ ] Registry services (Postgres)
- [ ] Testing & deployment

---

## Open Questions for Daniel

1. **Repo placement:** Is `repos-eidos-agi/hancock/` the right location? Or should Hancock be part of a different org?

2. **AID Protocol dependency:** Currently using local path to bbytesdev repo. Should we:
   - Publish `@aid-protocol/sdk` to npm?
   - Keep using local path during development?
   - Wait for Vybhav to publish?

3. **Dashboard timing:** Should we build the dashboard UI now, or focus on backend first?

4. **Production hosting:** For the API server, where should we host?
   - Cloudflare Workers (would require Hono adapter)
   - Fly.io
   - Railway
   - Self-hosted on existing infra

5. **Database hosting:** Postgres — managed or self-hosted?
   - Cloudflare D1 (SQLite-compatible, would simplify stack)
   - Neon (serverless Postgres)
   - Self-hosted on existing infra

---

## Next Steps (Pending Your Approval)

1. **Review this decision** — Confirm or correct technical choices
2. **AID Protocol alignment** — Confirm with Vybhav that we're using his SDK correctly
3. **Continue implementation** — Build delegation module, then dashboard
4. **Database setup** — Create Postgres schema for registry services
5. **First AID** — Generate Eidos's first AID document

---

## Files Created

```
hancock/
├── README.md
├── package.json
├── turbo.json
├── tsconfig.json
└── packages/
    ├── core/
    │   ├── package.json
    │   └── src/
    │       ├── index.ts
    │       └── aid.ts
    └── capabilities/
        ├── package.json
        └── src/
            └── index.ts
```

---

## Code Snippet: Creating Eidos's AID

```typescript
import { createEidosAIDForFirstTime } from '@hancock/core'

// One-time setup
const result = await createEidosAIDForFirstTime(
  'daniel@eidosagi.com',
  'Daniel Shanklin',
  'secure-passphrase' // Will encrypt keys at rest
)

console.log('AID created:', result.aid.aid.id)
console.log('Agent public key:', result.agentKeyPair.publicKey)
// Keys stored encrypted at ~/.eidos/keys
// AID document stored at ~/.eidos/aid.json
```

---

**Consequences**

- Hancock foundation is in place — can proceed with delegation, dashboard, and integration
- Technical choices align with existing Cloudflare-first infrastructure
- Key storage MVP is simple enough for local dev, migratable to Knox for production

**Waiting On**: Daniel's review and approval before proceeding with Phase 2-6.
