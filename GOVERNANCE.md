# Governance

How doctrine itself changes. The piece without which four doctrine repos drift against each other or get weaponized by whoever shouts loudest.

## ADR-style doctrine amendments

Every doctrine change is a numbered amendment in the doctrine repo:

```
<doctrine-repo>/doctrine/amendments/0023-severity-decay-extended.md
```

Format (mirrors judicial decision structure — the reasoning is part of the artifact):

```markdown
# Amendment 0023 — Severity decay extended from 30 to 60 days

## Context
[The state of the world before this amendment.]

## Decision
[What changed.]

## Consequences
- Positive: [...]
- Negative: [...]
- Neutral: [...]

## Citations
- Incident YYYY-MM-DD-X (link)
- Prior amendment 0019 (precedent / superseded)

## Dissenting views
[If any. Even solo-maintained projects benefit from steel-manning the rejected position.]

## Decided by
- Maintainer: <name>
- Date: <ISO-8601>
- Public comment period: <start> → <end>
```

Prevents doctrine from becoming a pile of rules nobody remembers why.

## Cross-doctrine conflict resolution

Doctrines will conflict. Auditing says "rules must be explainable," Testing says "property tests dominate," Observability says "logs are typed events." When they collide:

- **AVP-Doctrine is supreme.** It grades the others; its tenets win in conflict.
- **Below AVP, the four are peer.** Cross-doctrine amendments live in `PlausiDen-AVP-Doctrine/doctrine/amendments/` because they affect AVP's grading.
- Cross-doctrine amendments name every doctrine they touch, with a paired entry in each affected repo's `doctrine/amendments/` linking back.

## Sunset clauses

Every doctrine tenet has a **review date**. Not a mandatory expiry — a forced reconsideration.

```toml
# in doctrine/principles.toml
[[principle]]
id          = "no-unwrap-in-lib"
review_date = "2027-04-24"
[...]
```

When the review date arrives, the maintainer must:
- Re-read the tenet.
- Cite either an incident or a near-miss it has prevented in the past 12 months.
- If neither exists, the tenet either gets demoted (severity ↓) or removed.

Prevents the "this rule has existed for three years and nobody knows if it still matters" rot.

## Public comment period for amendments

Even for solo-maintained projects: amendments sit in a PR for **N days before merge**. Default N = 7. Forces slow thinking. Doctrine that can be changed in an hour will be changed in an hour.

**Override**: emergency amendments (security-critical, blocking production) can short-circuit the comment period with an explicit `EMERGENCY:` prefix in the PR title and a 24-hour mandatory post-merge review window.

## Amendment justification standard

Every amendment must cite **at least one** of:

1. A concrete incident that already happened (preferred).
2. A documented near-miss (acceptable).
3. An external authority change (e.g., WCAG version bump, RFC update).
4. A demonstrated cross-repo inconsistency that the amendment resolves.

Speculative amendments ("this might be a problem someday") are rejected by default. The bar exists because the meta-layer must remain net-negative work (see [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) §1).

## Doctrine versioning

Doctrine versions independently from engine code. A consumer can be on engine v2.3.1 while tracking doctrine v1.7. The doctrine version is what `harvest.toml` declares alignment with. This lets the engine iterate for performance/bug fixes without forcing a doctrine review on every patch.

Doctrine version bumps follow SemVer-for-doctrine:
- **MAJOR**: Tenet removed, new tenet added, conflict-resolution precedence changed.
- **MINOR**: Anti-pattern added, vocabulary field added, taxonomy entry added.
- **PATCH**: Wording clarification, example added, typo fix.

## Maintainer ratification

One maintainer ratifies an amendment for it to merge. Solo-maintained means you're the maintainer; the public comment period substitutes for peer review. As more humans join the project, ratification expands to require N maintainers.

## Reverse-direction notification

When a doctrine repo ships a doctrine-affecting change, consumers are notified via `CHANGELOG.toml` published from the doctrine repo. Consumer CI pulls this during audits and warns when the consumer is on a stale doctrine version. See [`PlausiDen-Harvest`](https://github.com/thepictishbeast/PlausiDen-Harvest) doctrine tenet 8.
