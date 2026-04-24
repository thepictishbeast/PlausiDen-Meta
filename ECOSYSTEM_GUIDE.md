# PlausiDen Ecosystem Guide

> The instruction set for any new project entering the PlausiDen namespace.
> Use this as the basis for project READMEs, CONTRIBUTING files, or
> Claude Code system prompts in PlausiDen-namespace repos.

## Five reference repos, one per role

For a new project, the default repos to reference are:

1. **[`PlausiDen-Obs`](https://github.com/thepictishbeast/PlausiDen-Obs)** — Observability substrate (logs, traces, secrets, audit-event sink). **Use it now**: it's Tier 0 in [`PRIORITY.md`](PRIORITY.md), the one piece available from line one of any new Rust project.
2. **[`PlausiDen-Canon`](https://github.com/thepictishbeast/PlausiDen-Canon)** — Design system and standards (tokens, primitives, components, contracts). **Conform to it**: not optional. New UI code uses Canon primitives; new code in any domain uses Canon tokens/contracts where applicable.
3. **[`PlausiDen-Audits`](https://github.com/thepictishbeast/PlausiDen-Audits)** — Enforcement engine and generic rule packs. **Run it, don't fight it**: install as a dev dependency, run in CI with a baseline ledger, fix new violations, decay existing ones per schedule.
4. **[`PlausiDen-Tests`](https://github.com/thepictishbeast/PlausiDen-Tests)** — Test harnesses and contract-parameterized suites. **Use its harnesses**: contract tests are parameterized from Canon contracts via PlausiDen-Tests. Don't reinvent test infrastructure.
5. **[`PlausiDen-AVP-Doctrine`](https://github.com/thepictishbeast/PlausiDen-AVP-Doctrine)** — Validation protocol the project is graded against before releases. **Grade against it before release**: the project declares target tiers in `integrations/avp.toml`; it doesn't cut v1.0 without passing them.

## Honest current-state caveat

Per [`PRIORITY.md`](PRIORITY.md), most of the engines are not built out yet:

- `PlausiDen-Obs` — **Tier 0** (early scaffold; awaiting first production consumer adoption to validate)
- `PlausiDen-Canon` — **Tier 1** (early scaffold; awaiting Sacred.Vote UI work as concrete adoption trigger)
- `PlausiDen-Audits` — **Tier 1** (existing repo with audit catalog; engine + rule packs build when LFI exceeds 100k lines OR third-repo duplicate-rule pain emerges)
- `PlausiDen-Tests` — **Tier 1** (early scaffold; harness builds when Canon ships ≥3 components AND a non-Canon consumer wants reuse)
- `PlausiDen-AVP-Doctrine` — **shipped** (existing). Doctrine-grading upgrade is **Tier 2** (when three doctrine repos have amendments)

**Until each repo has real implementation, the instruction below tells contributors to follow doctrine principles inline and migrate to formal dependency when the repo is ready.** This avoids generating confident-sounding references to vaporware.

---

## Standard project preamble (copy into new-project READMEs / Claude Code instructions)

> This project is part of the PlausiDen ecosystem. It must:
>
> - Emit structured logs and handle secrets via **`PlausiDen-Obs`** (mandatory for Rust; inline-compatible patterns for other languages until bindings exist).
> - Conform to **`PlausiDen-Canon`** for all design tokens, UI primitives, and cross-cutting standards where applicable.
> - Run **`PlausiDen-Audits`** in CI with a baseline ledger; new violations block merge.
> - Use **`PlausiDen-Tests`** harnesses for contract-parameterized testing.
> - Pass **`PlausiDen-AVP-Doctrine`** tier targets declared in `integrations/avp.toml` before any release.
>
> For repos not yet built, follow their doctrine principles inline and migrate to formal dependency when the repo is ready. Current build status of each: [`PlausiDen-Meta/PRIORITY.md`](https://github.com/thepictishbeast/PlausiDen-Meta/blob/main/PRIORITY.md).

---

## Per-repo instruction style

Each reference repo has a different verb. Match it:

| Repo | Verb | What it means |
|---|---|---|
| **Obs** | **Use** | Instantiate it. Imports, calls, the substrate is loaded into your service. |
| **Canon** | **Conform to** | A standard. You don't run Canon; you obey it. New code uses Canon constructs by default. |
| **Audits** | **Run** | A tool you invoke. CI step, pre-commit hook. Output is a verdict. |
| **Tests** | **Adopt the harnesses of** | A library of test scaffolding. You use its generators and runners; you don't write your own. |
| **AVP-Doctrine** | **Be graded by** | An external validation framework. Declare your tier targets, prove you meet them, link the proof in your release notes. |

## Language coverage

For non-Rust projects (rare — TypeScript in Sacred.Vote migration, Kotlin eventually), `Obs` drops off until it has bindings for that language. The doctrine still applies (structured logging, Secret-equivalent type, audit sink) — the engine adapts.

For language-specific guidance, see the corresponding `templates/<language>/` directory in each repo.

## When NOT to enter the PlausiDen namespace

Per [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) §2, **PlausiDen is not for experimental spikes.** If your project is:

- A throwaway prototype to test an idea
- A single-user script
- A fast-pivoting experiment with unclear shape

Keep it in an unlabeled scratch repo. Graduate to PlausiDen-namespace only when it's stable enough to obey doctrine.

## When entering the namespace

Once a project decides to enter:

1. Add the standard preamble above to the README.
2. Create `integrations/avp.toml` declaring tier targets (start with `tier_1_correctness` only).
3. Create `harvest.toml` at repo root (even if empty — declares the project participates in the harvest convention).
4. If Rust, add `plausiden-obs` to `Cargo.toml` as soon as it has real content.
5. Open a tracking issue in `PlausiDen-Meta` noting the new entrant — used for ecosystem-wide visibility and harvest discovery.
