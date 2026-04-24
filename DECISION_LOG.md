# Decision Log

Append-only record of tier-list and meta-layer decisions. Date order, newest first.

---

## 2026-04-24 — PlausiDen-Meta repo created; priority doc adopted as gate

**Context**: A long design conversation produced an extensive blueprint covering Canon, Audits, Tests, Observability, Harvest, and a dozen-plus additional doctrines. Risk identified: the meta-layer becoming the work, competing with object-level consumer work for attention.

**Decision**: Create this repo with `PRIORITY.md` as the trigger-gated gate for any future "should I build X" decision. Adopt the 5 operating principles + 20% meta-layer time budget cap.

**Consequences**: Future meta-layer work must cite a PRIORITY.md tier OR fire a tier-promotion trigger. Speculative meta work is rejected by default.

---

## 2026-04-24 — Built-ahead-of-trigger: Canon, Tests, Observability, Harvest scaffolds

**Context**: During the same session that produced the priority doc, scaffolds for all four were built at full skeleton level (tokens, contracts, doctrine documents, crate skeletons, primitive React components, harvest tool stub). This happened *before* the priority doc was committed.

**Decision**: Keep the scaffolds in their respective repos with explicit "built ahead of trigger" headers in each README pointing back to this decision and to PRIORITY.md. Do not extend any of them until their respective triggers fire.

**Consequences**:
- Canon, Tests, Observability, Harvest each ship as "early scaffolds awaiting trigger."
- The Tier 0 commitment (`plausiden-obs`) is *de facto* satisfied by `PlausiDen-Obs/crates/plausiden-obs` — but this counts only when a real consumer crate adopts it. Until then, scaffold status remains "experimental."
- This is a **one-time exception**, not a precedent. Future builds wait for triggers per the priority doc.

---

## 2026-04-24 — Axiom floor declared

**Context**: Recursive self-application risks (Audits audits Audits; AVP grades AVP) need a declared bottom or every discussion descends into meta-meta territory.

**Decision**: AVP-Doctrine Tier 0 ("exists and is internally consistent") is the axiom floor. Below it, claims are asserted by fiat. See [`AXIOM_FLOOR.md`](AXIOM_FLOOR.md).

**Consequences**: Doctrine debates can be cut off at the floor. Floor changes require constitutional-level friction (30-day comment vs. 7-day standard).

---

## 2026-04-24 — Governance model adopted

**Context**: Doctrine without governance drifts or gets weaponized.

**Decision**: ADR-style amendments with `Context / Decision / Consequences / Citations / Dissenting views`. AVP-Doctrine supreme; other doctrines peer. Sunset review dates on every tenet. 7-day public comment period default; 24-hour `EMERGENCY:` override with mandatory post-merge review. Citation requirement (incident, near-miss, external authority change, or cross-repo inconsistency) for every amendment. See [`GOVERNANCE.md`](GOVERNANCE.md).

**Consequences**: Speculative amendments rejected by default. Doctrine PRs slow down — this is the point.

---

## How to append

Every entry uses the format:

```
## YYYY-MM-DD — One-line decision title

**Context**: [State of the world before.]

**Decision**: [What changed.]

**Consequences**: [Bullet list of impacts.]
```

Newest first. Never edit prior entries — append a new one if a decision is reversed.
