# Decision Log — BlackRoad OS, Inc.

Architectural and strategic decisions, recorded for institutional memory.

---

## 2026

### DEC-2026-003 — App Router for blackroad-os-web
**Date:** 2026-01-20 | **Status:** ✅ Accepted

**Decision:** Use Next.js App Router (`app/` directory) exclusively. No Pages Router.

**Rationale:**
- Server Components reduce bundle size for data-heavy agent dashboards
- Route groups `(app)` and `(auth)` cleanly separate authenticated and public flows
- Streaming support aligns with SSE agent event streams

**Consequences:** All pages in `app/(app)/` for authenticated, `app/(auth)/` for login/signup.

---

### DEC-2026-002 — Python dataclasses over Pydantic for SDK types
**Date:** 2026-01-15 | **Status:** ✅ Accepted

**Decision:** Use stdlib `dataclasses` in Python SDK, not Pydantic v2.

**Rationale:**
- Zero dependencies for SDK consumers
- Sufficient for our type needs (no complex validators)
- Pydantic available as optional extra for users who want it

**Consequences:** `sdk-python/blackroad/types.py` uses `@dataclass`. Validation is manual.

---

### DEC-2026-001 — SQLite over PostgreSQL for local CLI persistence
**Date:** 2026-01-10 | **Status:** ✅ Accepted

**Decision:** All `br` CLI tool scripts use SQLite databases in `~/.blackroad/*.db`.

**Rationale:**
- Zero-config, zero-server, zero-dependencies
- Sufficient for single-developer CLI tooling
- Self-initializing on first run

**Consequences:** All tools call `init_db()` which creates tables if not present.

---

## 2025

### DEC-2025-005 — Tokenless gateway as trust boundary
**Date:** 2025-06-01 | **Status:** ✅ Accepted | **See:** RFC-001

**Decision:** All AI provider credentials held only by gateway. No agent ever holds a key.

---

### DEC-2025-004 — PS-SHA∞ for memory integrity
**Date:** 2025-05-15 | **Status:** ✅ Accepted

**Decision:** Memory entries form a hash chain: `hash = SHA256(prev_hash:content:timestamp_ns)`.
First entry uses `"GENESIS"` as prev_hash.

**Rationale:** Tamper-evident memory is essential for auditable AI systems.

---

### DEC-2025-003 — Trinary logic (Łukasiewicz) for truth states
**Date:** 2025-04-20 | **Status:** ✅ Accepted

**Decision:** Truth states are 1 (True), 0 (Unknown), -1 (False). No boolean only.

**Rationale:** AI knowledge is inherently uncertain. Binary truth is insufficient for
reasoning about partially-verified claims.

---

### DEC-2025-002 — Cloudflare as primary edge platform
**Date:** 2025-03-10 | **Status:** ✅ Accepted

**Decision:** Deploy all public-facing services to Cloudflare Workers/Pages.
Account: `848cf0b18d51e0170e0d1537aec3505a`.

**Rationale:** Zero-cold-start edge compute. Global distribution. D1, KV, R2 integrated.

---

### DEC-2025-001 — Brand gradient golden ratio stops
**Date:** 2025-02-01 | **Status:** ✅ Accepted

**Decision:** Brand gradient uses golden ratio (φ = 1.618) for color stops:
`amber(0%) → hot-pink(38.2%) → violet(61.8%) → electric-blue(100%)`.

Primary brand color: `#FF1D6C` (hot-pink).
