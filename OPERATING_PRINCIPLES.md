# Operating Principles

These are the non-negotiable constraints under which every PlausiDen-namespace decision is made. They sit above doctrine — doctrine implements these; these define them.

## 1. The meta-layer must be net-negative work

If building and maintaining the doctrine infrastructure costs more hours than the hours it saves on object-level work, it's a vanity project. This is a falsifiable claim and must be measured.

**Measurement mechanism**: each doctrine repo maintains an `INCIDENT_LOG.md` — append-only entries: "this amendment was prompted by incident X on date Y, where it would have saved Z hours." If after 6 months the log is thin, the meta-layer is underperforming and **must contract, not expand**.

## 2. The doctrine stance is a project-shape commitment

Highly-doctrined projects are high-ceiling, high-floor, slow-velocity. They're excellent for long-lived, correctness-critical, multi-party systems (voting, security, sovereign infrastructure) — exactly the PlausiDen portfolio. They're terrible for experimental, fast-pivoting, single-user scripts.

**The implicit commitment**: PlausiDen is **not the place for experimental spikes**. Experimental work happens in unlabeled scratch repos and only graduates into the PlausiDen namespace when it's ready for doctrine.

## 3. Object-level work always wins

The object-level consumer projects (those building actual products) are the work. The PlausiDen meta-layer (Canon, Audits, Tests, Obs, Harvest, AVP-Doctrine, Meta) exists to make them better. **If at any point doctrine work competes with object-level work for attention, object-level work wins — always.**

## 4. Time-budget cap

**20% of engineering time per week** is the maximum allocation to meta-infrastructure. If this week's budget is spent, the remainder goes to object-level consumer projects. Tracked in [`DECISION_LOG.md`](DECISION_LOG.md) on a quarterly review.

## 5. One-consumer-in-production rule

No PlausiDen-namespace piece graduates from "experimental" to "normative" until at least one production consumer has adopted it. The expansive design lists in the doctrine repos are *destinations*, not v0.1 checklists.

## 6. Trigger-gated, not anticipated

Building ahead of a trigger declared in [`PRIORITY.md`](PRIORITY.md) is itself a doctrine violation against the meta-layer. The architecture exists; the implementation waits for evidence.

## 7. Determinism over plausibility for enforcement

Hard invariants (touch targets, contrast, raw-value detection, type-level contracts) are checked deterministically. AI may draft contracts, explain violations, propose fix codemods — but never autonomously mutates a baseline, waives a violation, or runs as a CI gate. This preserves PSA (no vendor hostage) and makes eventual swap to a sovereign engine a swap, not a rebuild.

## 8. Local-first defaults

Every PlausiDen tool works with zero external dependencies by default. Remote aggregation, cloud vendors, third-party services are opt-in and never mandatory. Aligns with the broader PlausiDen mission.

## 9. The complexity cliff is real

The full design surface across all doctrine repos plus Canon plus the object-level consumer projects is **18–24 months of focused work for one person if everything else stops**. Everything else is not stopping. **The architecture is correct. Building all of it serially is not survivable.** This document is the discipline against that.

## 10. Counter-mechanism: meta-layer additions require demonstrated object-level pain

Every doctrine tenet, every harvest candidate type, every AVP tier must cite the **concrete failure it prevents** — preferably an incident that already happened. Speculative generality is the failure mode that turns infrastructure into a beautiful cathedral that competes with object-level consumer work for attention. **Citation requirement is enforced by the doctrine-amendment process** (see [`GOVERNANCE.md`](GOVERNANCE.md)).
