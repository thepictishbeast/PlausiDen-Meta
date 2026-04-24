# Quickstart for a New PlausiDen Consumer

> Paste the relevant section into your project's README or Claude Code system prompt.

## The six reference repos

| Repo | Verb | What it asks of you |
|---|---|---|
| **[`PlausiDen-Meta`](https://github.com/thepictishbeast/PlausiDen-Meta)** | Read | Ecosystem-wide principles, governance, priority triggers, doctrine. Start here. |
| **[`PlausiDen-Obs`](https://github.com/thepictishbeast/PlausiDen-Obs)** | Use | Structured logging, tracing, secret handling. Mandatory for Rust consumers. |
| **[`PlausiDen-Canon`](https://github.com/thepictishbeast/PlausiDen-Canon)** | Conform to | Tokens + (eventually) UI primitives + contracts. |
| **[`PlausiDen-Audits`](https://github.com/thepictishbeast/PlausiDen-Audits)** | Run | Static + runtime audit gates in CI with a baseline ledger. |
| **[`PlausiDen-Tests`](https://github.com/thepictishbeast/PlausiDen-Tests)** | Adopt the harnesses of | Contract-parameterized tests via the contract runner. |
| **[`PlausiDen-AVP-Doctrine`](https://github.com/thepictishbeast/PlausiDen-AVP-Doctrine)** | Be graded by | Validation tier targets declared in `integrations/avp.toml`. |
| **[`PlausiDen-Harvest`](https://github.com/thepictishbeast/PlausiDen-Harvest)** | Emit `harvest.toml` if applicable | Upstream-candidate broadcast convention. |

## Standard new-project preamble (paste this)

> This project is part of the PlausiDen ecosystem. See **PlausiDen-Meta** for ecosystem-wide principles, governance, and priority triggers. For its technical dependencies:
>
> - **PlausiDen-Obs** — use for structured logging, tracing, and secret handling (mandatory for Rust).
> - **PlausiDen-Canon** — conform to tokens and (eventually) UI primitives and contracts.
> - **PlausiDen-Audits** — run in CI with a baseline ledger.
> - **PlausiDen-Tests** — use harnesses for contract-parameterized testing.
> - **PlausiDen-AVP-Doctrine** — declare target tiers in `integrations/avp.toml` and pass them before release.
> - **PlausiDen-Harvest** — if this project produces reusable patterns, emit a `harvest.toml` per the protocol schema.
>
> For repos not yet built, follow their doctrine principles inline and migrate to formal dependency when the repo is ready. Current build status: [`PlausiDen-Meta/PRIORITY.md`](https://github.com/thepictishbeast/PlausiDen-Meta/blob/main/PRIORITY.md).

## Honest current-state caveat (2026-04-24)

Most engines are scaffolded, not production-ready. Per [`PRIORITY.md`](PRIORITY.md):

| Repo | Tier | What's actually shippable today |
|---|---|---|
| `PlausiDen-Obs` | **0** | `plausiden-obs` Rust crate scaffold; awaits first production consumer. |
| `PlausiDen-Canon` | 1 | Tokens + contracts + React adapter scaffold. Awaits first UI consumer. |
| `PlausiDen-Audits` | 1 | Catalog + doctrine; engine implementation deferred until trigger. |
| `PlausiDen-Tests` | 1 | Doctrine + harness scaffold; runner deferred until Canon ships contracts in production. |
| `PlausiDen-AVP-Doctrine` | shipped | Validation protocol + standing orders. Meta-doctrine grading upgrade is Tier 2. |
| `PlausiDen-Harvest` | 3 | Protocol + schema only; tooling deferred until 3+ active candidate-proposing consumers. |
| `PlausiDen-Meta` | meta | This repo. Operational. |

**Until each engine has real implementation, follow doctrine principles inline.** Don't reference vaporware as if it's production-ready.

## Minimum integration for a greenfield consumer

```sh
# 1. Add a doctrine-alignment file at repo root
cat > integrations/avp.toml <<'EOF'
[meta]
repo                = "<your-repo-name>"
doctrine_version    = "1.0"

[tiers]
tier_1_correctness  = "in_progress"

[graduation]
next_gate = "<what triggers your v1.0>"
EOF

# 2. Declare ecosystem participation via harvest convention (even if empty)
cat > harvest.toml <<'EOF'
[meta]
project              = "<your-repo-name>"
upstream_candidates  = false   # set true when you have something to propose
review_cadence       = "monthly"
EOF

# 3. Add the standardized README header (HTML comments at top of README.md)
# See REPO_LABEL_REGISTRY.md for the schema.

# 4. If Rust: add plausiden-obs to Cargo.toml as soon as it has real content.
# 5. Open a tracking issue in PlausiDen-Meta noting the new entrant.
```

## Minimum integration for retrofitting an existing project

The principle: **baseline first, don't block on legacy violations.**

```sh
# 1. Run the audit engine against your tree, capture all current violations
#    into a baseline file. CI fails only on NEW violations.
plausiden-audits run --baseline-init .audit-baseline.json

# 2. Existing violations decay per the severity-decay schedule in
#    PlausiDen-Audits/doctrine/principles.toml. Fix them at your pace.

# 3. New code lands clean: every PR that introduces a new violation fails CI.

# 4. Same pattern for Tests, Obs, Canon. Adopt incrementally; never flag-day.
```

## When NOT to enter the PlausiDen namespace

Per [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) §2: **PlausiDen is
not for experimental spikes.** If your project is:

- A throwaway prototype to test an idea
- A single-user script
- A fast-pivoting experiment with unclear shape

Keep it in an unlabeled scratch repo. Graduate to PlausiDen-namespace only
when it's stable enough to obey doctrine.

## Where to ask

1. Open an issue in [`PlausiDen-Meta`](https://github.com/thepictishbeast/PlausiDen-Meta) for ecosystem-wide questions.
2. Open an issue in the specific repo for repo-specific questions.
3. Doctrine amendment proposals: PR with the [`GOVERNANCE.md`](GOVERNANCE.md) ADR template.
