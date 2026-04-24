# Contributing to PlausiDen-Meta

Before opening a PR against any PlausiDen-namespace repo (this one or any
sibling), run through [`CONTRIBUTOR_CHECKLIST.md`](CONTRIBUTOR_CHECKLIST.md).

The checklist is non-negotiable for **additive** contributions (new
principles, new docs, new entries in the registry, new doctrine clauses).
Trivial fixes (typos, broken links, formatting) skip to the PR.

## What lives in this repo

- Ecosystem-wide governance — [`GOVERNANCE.md`](GOVERNANCE.md).
- Operating principles — [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md).
- The priority gate — [`PRIORITY.md`](PRIORITY.md).
- The independence test + scope — [`SCOPE.md`](SCOPE.md).
- Stack-neutral baselines — [`SECURITY_BASELINE.md`](SECURITY_BASELINE.md), [`SUPERSOCIETY_BASELINE.md`](SUPERSOCIETY_BASELINE.md).
- The repo registry + label schema — [`REPO_LABEL_REGISTRY.md`](REPO_LABEL_REGISTRY.md).
- Decision history — [`DECISION_LOG.md`](DECISION_LOG.md).

## What does NOT live here

- Audit rules (those go in [`PlausiDen-Audits`](https://github.com/thepictishbeast/PlausiDen-Audits))
- Test harnesses (→ [`PlausiDen-Tests`](https://github.com/thepictishbeast/PlausiDen-Tests))
- Observability vocabulary (→ [`PlausiDen-Obs`](https://github.com/thepictishbeast/PlausiDen-Obs))
- Design tokens / contracts (→ [`PlausiDen-Canon`](https://github.com/thepictishbeast/PlausiDen-Canon))
- Validation tier definitions (→ [`PlausiDen-AVP-Doctrine`](https://github.com/thepictishbeast/PlausiDen-AVP-Doctrine))
- Harvest protocol details (→ [`PlausiDen-Harvest`](https://github.com/thepictishbeast/PlausiDen-Harvest))

If your contribution is in any of those domains, file it against the
relevant repo, not this one. PlausiDen-Meta only holds artifacts that
coordinate the others.

## Doctrine amendments

To change `OPERATING_PRINCIPLES.md`, `GOVERNANCE.md`, `SCOPE.md`,
baselines, or any normative content:

1. Open a PR with the ADR template from [`GOVERNANCE.md`](GOVERNANCE.md).
2. Bump the doctrine version in the affected file's header.
3. Add an entry to [`CHANGELOG.toml`](CHANGELOG.toml) describing the change.
4. Add an entry to [`DECISION_LOG.md`](DECISION_LOG.md) with rationale.
5. PR sits for the public-comment period (default 7 days).
6. One maintainer ratifies. AVP doctrine-conformance + generality tiers must pass.

## Co-author trailer

Every commit includes a `Co-Authored-By:` trailer. Agent contributions
explicitly identify the model and version.
