# Decision Log

Append-only record of tier-list and meta-layer decisions. Date order, newest first.

---

## 2026-04-24 (latest) — Anti-duplication + CONTRIBUTOR_CHECKLIST + CHANGELOG.toml stubs

**Context**: Many independent Claude Code sessions, AI agents, and humans will contribute to PlausiDen repos without seeing each other's work. The maintainer flagged having many vastly different consumer projects all referencing these repos, so duplication / redundancy / over-specificity hurts all of them. Default failure mode without discipline = fragmentation.

**Decision**:
1. `OPERATING_PRINCIPLES.md` §13 — no duplication, no redundancy, no over-specificity. Six rules: search before adding, extend don't fork, generalize before committing, reference don't duplicate doctrine clauses, cross-repo via Meta, per-consumer specifics stay in consumer's repo.
2. `CONTRIBUTOR_CHECKLIST.md` — 7-step workflow every contributor follows. PR descriptions must report search results + independence-test result, or PRs get closed.
3. `CHANGELOG.toml` stubs in all 7 doctrine repos. Required by §11 staleness gate.
4. `CONTRIBUTING.md` added to repos that didn't have one (Meta, Obs, Harvest); each redirects to CONTRIBUTOR_CHECKLIST.md plus repo-specific "what lives here" guidance.

**Consequences**: Mechanical anti-fragmentation. Future contributors have a written discipline. Trade-off: more PR friction, accepted per OPERATING_PRINCIPLES §3.

---

## 2026-04-24 — Doctrine-is-living principle (§11) + EXCEPTIONS.md protocol (§12)

**Context**: The doctrine-as-published framing left two things implicit that consumers would miss: doctrines evolve so consumers must check regularly, and when a consumer can't comply the response must be a narrow formal exception not silent deviation.

**Decision**:
1. `OPERATING_PRINCIPLES.md` §11 — doctrines are living; consumers check on a declared cadence (monthly minimum, more during active work). `CHANGELOG.toml` is canonical update signal; CI staleness gates warn at 30d / fail at 90d.
2. `OPERATING_PRINCIPLES.md` §12 — exceptions must be specific, scope-limited, narrow, time-bounded.
3. `EXCEPTIONS.md` — full protocol + template + extension rules + cross-project escalation.

**Consequences**: Concrete cadence expectation; deviation is tracked + reviewed + expiring; every exception doubles as evidence for amendment.

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
