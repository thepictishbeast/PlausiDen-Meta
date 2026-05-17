> # ⚠️ DO NOT USE — UNVERIFIED — UNSAFE ⚠️
>
> This software is **unverified and unsafe for any production use**.
> It is published publicly only for transparency, third-party audit,
> and reproducibility. Treat every commit as guilty until proven
> innocent.
>
> By using this code you accept:
> - **No warranty** of any kind, express or implied.
> - **No fitness** for any particular purpose.
> - **No guarantee** of correctness, safety, or freedom from defects.
> - **Zero liability** on the maintainer for any damages — data loss,
>   security compromise, financial loss, or any consequential damages.
>
> The code is under active engineering development per the
> [Adversarial Validation Protocol v2](https://github.com/thepictishbeast/PlausiDen-AVP-Doctrine/blob/main/AVP2_PROTOCOL.md).
> Every commit's default verdict is **STILL BROKEN**. AVP-2 requires
> a minimum of 36 verification passes before a `SHIP-DECISION:`
> annotation may be considered. **No commit in this repository has
> reached `SHIP-DECISION:` status.**

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
| [`SCOPE.md`](SCOPE.md) | What PlausiDen is and isn't. Generic FOSS ecosystem charter. The independence test. |
| [`PRIORITY.md`](PRIORITY.md) | The trigger-gated tier list. What gets built when, why, and what stays shelved. Single source of truth. |
| [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) | Non-negotiable operating principles (time budget, one-consumer-in-production, doctrines-are-living, exceptions-must-be-narrow, etc.). |
| [`SECURITY_BASELINE.md`](SECURITY_BASELINE.md) | 16 stack-neutral security demands of every consumer. |
| [`SUPERSOCIETY_BASELINE.md`](SUPERSOCIETY_BASELINE.md) | 15 sovereignty / longevity / inclusion demands of every consumer. |
| [`GOVERNANCE.md`](GOVERNANCE.md) | How doctrine itself changes (ADR amendments, cross-doctrine precedence, sunset clauses, comment periods). |
| [`AXIOM_FLOOR.md`](AXIOM_FLOOR.md) | Where the recursive self-application stops. The asserted-by-fiat bottom. |
| [`ECOSYSTEM_GUIDE.md`](ECOSYSTEM_GUIDE.md) | Per-repo verbs for new consumers (use / conform to / run / adopt / be graded by). |
| [`QUICKSTART.md`](QUICKSTART.md) | 1-page paste-into-new-project preamble + minimum-integration scripts + monthly-update sweep. |
| [`EXCEPTIONS.md`](EXCEPTIONS.md) | Protocol + template for when a consumer cannot meet a doctrine requirement. Specific, scope-limited, narrow, time-bounded. |
| [`REPO_LABEL_REGISTRY.md`](REPO_LABEL_REGISTRY.md) | Standardized header schema + every PlausiDen-* repo's URL + local path + label + class + crate-location table + known-consumers index. |
| [`DECISION_LOG.md`](DECISION_LOG.md) | Append-only record of tier-list and doctrine-positioning decisions. |

## These docs are living

Every file in this repo is subject to doctrine amendments per [`GOVERNANCE.md`](GOVERNANCE.md). **Consumers must check in regularly** — at minimum monthly, more often during active work on a doctrine-adopting project. Staleness is tracked via `CHANGELOG.toml` at each doctrine repo's root; CI warns at 30 days behind and fails at 90 days behind. See [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) §11 + [`EXCEPTIONS.md`](EXCEPTIONS.md).

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
