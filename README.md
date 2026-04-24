<!-- repo-label: meta-doctrine -->
<!-- repo-class: ecosystem-priority-and-governance-gate -->
<!-- repo-consumes: nothing -->
<!-- repo-consumed-by: every PlausiDen-* repo (advisory; gate for "should I build X" decisions) -->

# PlausiDen-Meta

> The single written prior commitment that defeats in-the-moment pull toward
> building the next shiny meta-piece instead of object-level consumer projects
> PlausiDen ecosystem.
>
> Sister repos: every PlausiDen-* repo references this one for its tier status.

## What this repo holds

| File | Purpose |
|---|---|
| [`PRIORITY.md`](PRIORITY.md) | The trigger-gated tier list. What gets built when, why, and what stays shelved. Single source of truth. |
| [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) | Non-negotiable operating principles (meta-layer time budget, one-consumer-in-production, etc.). |
| [`GOVERNANCE.md`](GOVERNANCE.md) | How doctrine itself changes (ADR amendments, cross-doctrine precedence, sunset clauses, comment periods). |
| [`AXIOM_FLOOR.md`](AXIOM_FLOOR.md) | Where the recursive self-application stops. The asserted-by-fiat bottom. |
| [`DECISION_LOG.md`](DECISION_LOG.md) | Append-only record of tier-list decisions with date + rationale. |

## What this repo is not

- Not a roadmap. Roadmaps imply sequencing; the priority list is trigger-gated.
- Not a wishlist. Shelved items are not "someday" — they're "only if a specific condition changes."
- Not permanent. Tier boundaries move as reality develops. Append to the decision log when they do.
- Not a substitute for the design conversations behind it. Those live in the doctrine repos and in session memory.

## Review cadence

- **Monthly**: Scan tier 0/1 for trigger fires. Update [`PRIORITY.md`](PRIORITY.md).
- **Quarterly**: Review meta-layer time budget actuals vs. the 20% target. Demote or pull forward as needed.
- **Semi-annually**: Review whether shelved items should be deleted (genuinely bad idea retroactively) or remain shelved (still valid, still no trigger).
- **On every incident**: Check whether the incident would have been prevented by a shelved item. If yes, that's a trigger — promote it.

## License

[MIT](LICENSE).
