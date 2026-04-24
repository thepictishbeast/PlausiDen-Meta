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

## 11. Doctrines are living; consumers check on a declared cadence

No PlausiDen doctrine is frozen. They evolve via the amendment process (see [`GOVERNANCE.md`](GOVERNANCE.md)) as incidents, evidence, and harvest candidates accumulate. Consumers **must** track doctrine updates and cannot assume yesterday's doctrine still applies.

Mechanism:

- **Every doctrine repo publishes `CHANGELOG.toml` at its root.** Machine-readable; keyed by doctrine version; lists each amendment's id, summary, and affected tenets.
- **Every consumer's CI pulls the `CHANGELOG.toml` on every audit run** and warns when the consumer's declared `doctrine_version` (in `integrations/avp.toml`) is behind the published version.
- **Staleness escalation**: warn at 30 days behind; error at 90 days behind. Consumers that can't upgrade immediately must file an exception per [`EXCEPTIONS.md`](EXCEPTIONS.md).
- **Cadence expectation**: doctrine amendments land on no fixed schedule (amendment is gated on evidence, not calendar). But consumers should **manually check doctrine repos at least monthly** even when CI is green, since amendment proposals sit in public-comment PRs for at least 7 days before merging and consumer feedback matters most before the amendment lands, not after.

Corollary: **if you haven't checked a doctrine repo in 30 days, assume something has changed.** `git fetch && cat CHANGELOG.toml` is a 5-second operation and should happen whenever a consumer maintainer resumes work on a doctrine-adopting project.

## 13. No duplication. No redundancy. No over-specificity in contributions.

Many independent Claude Code sessions, AI agents, and human contributors
will work on PlausiDen repos over time without seeing each other's work.
The default failure mode is **fragmentation**: two sessions independently
write the same rule with slightly different wording, the same harness
under two names, the same vocabulary field with two spellings.

The discipline against fragmentation:

1. **Search before adding.** Every contribution starts with a search of the relevant repo's existing artifacts. Adding a rule? Grep `audits/` and `templates/` first. Adding a harness? Search `templates/` first. Adding a vocabulary field? Read `doctrine/vocabulary.toml` end-to-end first.

2. **Extend, don't fork.** If an existing artifact is 80% right, extend it (add a clause, add an allowlist entry, expand a taxonomy). Do not introduce a parallel artifact unless the existing one is structurally incompatible.

3. **Generalize before committing.** Any pattern observed in one consumer that gets contributed up MUST first pass the independence test in [`SCOPE.md`](SCOPE.md): "if a stranger cloned this repo tomorrow and knew nothing about my project, would this make sense as a standalone artifact?" If no, generalize until yes, or scope it back into your own repo.

4. **Reference, don't duplicate, doctrine clauses.** When writing a new rule that enforces an existing doctrine clause, reference the clause by id rather than restating it. Doctrine has one canonical source of each tenet; rules are the multiple enforcers of those tenets.

5. **Cross-repo concerns route through PlausiDen-Meta.** When a contribution touches multiple doctrine repos (e.g., a rule that depends on a new vocabulary field in Obs and a new contract in Canon), file an issue in PlausiDen-Meta first to coordinate the cross-repo amendment before committing in any single repo.

6. **Per-consumer specifics stay in the consumer's repo.** If your contribution only makes sense in the context of a specific consumer's project, the contribution belongs in that consumer's repo, not in any PlausiDen-namespace repo.

Enforcement: see [`CONTRIBUTOR_CHECKLIST.md`](CONTRIBUTOR_CHECKLIST.md) for the
workflow every contributor (human or AI agent) follows before opening a PR.

---

## 12. Exceptions must be specific, scope-limited, and narrow

When a consumer genuinely cannot meet a doctrine requirement, the answer is an **exception**, not a silent deviation. Exceptions are tracked artifacts with strict form.

**Required** for every exception:

1. **Named tenet.** The exact principle or rule being excepted, by id. Not "the security doctrine" — "`plausiden-obs` doctrine tenet 4 (secret-type-refused), specifically the `Serialize` derivation path."
2. **Scope.** File-path glob or module id. Not "our codebase" — "`src/legacy/fixture_gen.rs` only, lines 47–92."
3. **Expiry.** Absolute date (ISO-8601). Not "TBD" — "2026-07-31." No open-ended exceptions.
4. **Justification.** Why the doctrine can't be satisfied as-is in this scope. Technical, not preferential.
5. **Plan to retire.** What condition or work will remove the need for the exception.
6. **Linked ticket.** Issue tracker URL. No orphan exceptions.
7. **Approver.** Maintainer signature. For cross-project exceptions (doctrine-breaking at the ecosystem level), approval routes through the PlausiDen-Meta maintainer.

**Explicitly forbidden**:

- Blanket exceptions ("we don't use this tenet").
- Open-ended exceptions ("until we figure something out").
- Exceptions broader than a single file/module/endpoint.
- Exceptions to non-waivable tenets (see the tenet's `waivable` field).
- Exceptions that amount to disagreement with a doctrine — that's a doctrine amendment proposal, not an exception.

See [`EXCEPTIONS.md`](EXCEPTIONS.md) for the full template and the review protocol. **If the exception doesn't fit on one page with all fields filled, it's too broad.**
