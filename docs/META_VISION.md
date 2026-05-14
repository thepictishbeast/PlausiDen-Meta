# PlausiDen-Meta — vision document

> If PlausiDen-Meta did everything we wanted, what would this say?

**[shipped]** / **[queued]** / **[concept]**.

---

## 1. What PlausiDen-Meta IS

**The single written prior commitment that defeats in-the-moment
pull toward building the next shiny meta-piece instead of
object-level consumer projects.** Ecosystem priority + governance
gate. Holds the trigger-gated tier list, the operating
principles, the security baseline, the supersociety baseline,
the governance protocol, the axiom floor, and the per-repo verbs
guide.

Operationally a Markdown + TOML repo:

| File | Purpose |
|---|---|
| `SCOPE.md`                  | What PlausiDen is and isn't (independence test) |
| `PRIORITY.md`               | Trigger-gated tier list — single source of truth |
| `OPERATING_PRINCIPLES.md`   | Time budget / one-consumer-in-prod / doctrines-are-living / exceptions-must-be-narrow |
| `SECURITY_BASELINE.md`      | 16 stack-neutral security demands of every consumer |
| `SUPERSOCIETY_BASELINE.md`  | 15 sovereignty / longevity / inclusion demands |
| `GOVERNANCE.md`             | How doctrine itself changes (ADR amendments / cross-doctrine precedence / sunset / comment periods) |
| `AXIOM_FLOOR.md`            | Where recursive self-application stops (asserted-by-fiat bottom) |
| `ECOSYSTEM_GUIDE.md`        | Per-repo verbs for new consumers (use / conform to / run / adopt / be graded by) |
| `CHANGELOG.toml`            | Machine-readable changelog of doctrine amendments |

PlausiDen-Meta is **not** code (no `src/`, no `Cargo.toml`).
Not a tooling repo. Not a CI runner. It's the constitution.

## Dependencies

**Direct:** none. Meta is one of the roots of the ecosystem DAG
(consumed-by every PlausiDen-* repo; consumes nothing).

**Transitive:** none.

**Consumed by:** every PlausiDen-* repo (advisory; gate for
"should I build X" decisions). Forge / Oxidizer / Audits all
read PRIORITY.md to enforce the tier-promotion rules.

## The meta-mission: AI-built UI reliability

Meta is **the priority gate that prevents agents from drifting
into shiny-object work.** An agent contemplating "should I build
PlausiDen-Y?" reads PRIORITY.md first. If Y isn't tier-promoted,
agent declines. Without Meta, every agent re-litigates priority
every session; with Meta, the priority is one document, signed,
versioned.

## 2. Capability map

| Capability | Status |
|---|---|
| `SCOPE.md` (independence test) | shipped |
| `PRIORITY.md` (trigger-gated tier list) | shipped |
| `OPERATING_PRINCIPLES.md` | shipped |
| `SECURITY_BASELINE.md` (16 demands) | shipped |
| `SUPERSOCIETY_BASELINE.md` (15 demands) | shipped |
| `GOVERNANCE.md` (amendment protocol) | shipped |
| `AXIOM_FLOOR.md` | shipped |
| `ECOSYSTEM_GUIDE.md` (per-repo verbs) | shipped |
| `CHANGELOG.toml` (machine-readable amendments) | shipped |
| Per-repo PRIORITY-conformance check (Forge `phase_priority_check`) | concept |
| Per-amendment RFC process with public comment periods | shipped |
| Trigger-promotion automation (when promoting condition met, auto-update tier) | concept |
| Cross-doctrine precedence resolver (when two doctrines conflict) | shipped (in GOVERNANCE.md) |
| Sunset clause enforcement (when does a doctrine retire) | shipped (in GOVERNANCE.md) |
| MCP server for agent queries (`get_tier`, `get_baseline`, `propose_amendment`) | concept |
| Federated meta (cross-signing of doctrine amendments) | concept |
| Quarterly governance ceremony with public log | concept |
| Per-amendment historical-context reference | concept |

## 3. Architecture

```
┌──────────── PlausiDen-Meta ────────────┐
│  Constitution (Markdown + TOML):        │
│  - SCOPE.md                             │
│  - PRIORITY.md                          │
│  - OPERATING_PRINCIPLES.md              │
│  - SECURITY_BASELINE.md                 │
│  - SUPERSOCIETY_BASELINE.md             │
│  - GOVERNANCE.md                        │
│  - AXIOM_FLOOR.md                       │
│  - ECOSYSTEM_GUIDE.md                   │
│  - CHANGELOG.toml                       │
└─────────────────────────────────────────┘
            │
            ▼ advisory (every repo reads)
   Every PlausiDen-* repo
   - reads tier from PRIORITY.md
   - conforms to baselines
   - cites the Meta commit it conforms to
```

## 4. Roadmap

- **Sprint 1:** Forge `phase_priority_check`; trigger-promotion
  automation; per-repo CHANGELOG sync from Meta amendments.
- **Sprint 2:** MCP server for agent queries; per-amendment
  historical-context reference.
- **Sprint 3:** Federated meta (peer cross-signing of doctrine
  amendments); quarterly governance ceremony with public
  transparency log.

## 5. Acceptance criteria

1. Every PlausiDen-* repo cites the Meta commit it conforms to.
2. Every Meta amendment goes through the GOVERNANCE.md
   RFC + comment-period process.
3. Trigger-promotion is mechanically verifiable (Forge phase
   confirms the trigger condition is met before allowing tier
   bump).
4. The 16 SECURITY_BASELINE + 15 SUPERSOCIETY_BASELINE demands
   are mechanically checked per consumer (Audits / Forge
   integration).
5. The axiom floor is asserted-by-fiat and not subject to
   recursive self-questioning — base case prevents infinite
   regress.
6. Federated Meta instances cross-sign for decentralized
   governance.
