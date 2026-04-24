# PlausiDen Ecosystem — Priority & Trigger Doc

*Last updated: 2026-04-24. Living document — revise when triggers fire or conditions change.*

---

## Operating principles (non-negotiable)

1. **Meta-infrastructure is net-negative until proven otherwise.** Every built piece must cite the object-level pain it eliminated within 90 days of shipping. Unjustified pieces get shelved.
2. **Meta-layer time budget: 20% of engineering time, weekly.** If this week's meta budget is spent, remaining time goes to LFI, Sacred.Vote, or Protection Suite. Full stop.
3. **One consumer in production before generalization.** No PlausiDen-namespace piece graduates from experimental until at least one real project depends on it in anger.
4. **Triggers fire; you don't anticipate them.** Building ahead of a trigger is a doctrine violation against this document itself.
5. **PlausiDen is not for experimental spikes.** Spikes happen in unlabeled scratch repos. Only doctrine-ready work enters the PlausiDen namespace.

---

## Tier 0 — Build this month

### `plausiden-obs` crate (Observability substrate, minimal)

- **What**: Thin Rust crate wrapping `tracing` + `tracing-subscriber`. Ships: JSON formatter with opinionated field taxonomy, `Secret<T>` wrapper type (no Display/Debug/Serialize), audit-log sink separate from debug-log sink, test utilities for asserting log structure.
- **Trigger (already fired)**: ~77k lines of Rust across LFI and Protection Suite with ad-hoc logging. Debugging cost compounds weekly.
- **Scope cap**: One weekend. If it's taking longer, it's over-scoped.
- **Success criterion**: LFI and at least one Protection Suite crate adopt it within 30 days and report reduced debug friction.
- **Doctrine at this stage**: Inline comments + a `DOCTRINE.md` stub. No separate doctrine repo yet.
- **Non-goals**: Metrics, traces across services, OTel export, remote aggregation. All deferred.
- **Status (2026-04-24)**: Scaffold lives at [`PlausiDen-Obs`](https://github.com/thepictishbeast/PlausiDen-Obs) → `crates/plausiden-obs`. Awaiting consumer adoption to validate.

---

## Tier 1 — Build when specific triggers fire

### PlausiDen-Audits engine (static rules only)
- **Trigger**: Any one of — (a) LFI exceeds 100k lines of Rust, (b) writing the same pre-commit check in three repos, (c) Sacred.Vote approaches its first audited release and needs attestation artifacts.
- **Initial scope**: Tree-sitter query engine, baseline ledger, three rules end-to-end (raw-string detection, `println!`/`console.log` detection, `unwrap()` in public paths), SARIF output, one CI adapter (GitHub Actions).
- **Excludes**: Runtime rules, codemods, plugin system, registry, LSP. All deferred.

### PlausiDen-Canon (UI tokens + React primitives)
- **Trigger**: Sacred.Vote UI work resumes in earnest OR a second consumer needs shared UI primitives.
- **Initial scope**: `tokens.toml` + `token-forge` CLI + React target with `Box`, `Stack`, `Text`, `Button`, `TextField`. ESLint custom rule banning raw `<button>`/`<input>`.
- **Excludes**: Kotlin/iced/egui targets, full contracts YAML, theming overlays, multi-domain expansion. All deferred.
- **Status (2026-04-24)**: **Built ahead of trigger.** Full scaffold exists — tokens, contracts for 7 components, React target with 4 primitives + Button. Pending Sacred.Vote adoption to validate or trim.

### PlausiDen-Tests harness (contract-parameterized runner)
- **Trigger**: Canon ships ≥3 components with behavior contracts AND at least one non-Canon consumer wants to reuse the same contract tests.
- **Initial scope**: TOML contract schema, test-case generator for one target (React via Playwright), content-fuzz generator, viewport-matrix helper.
- **Excludes**: Multi-language runners, visual regression, mutation-testing integration. All deferred.
- **Status (2026-04-24)**: Doctrine-only scaffold exists. Contract-runner crate is a CLI shape with stubs.

---

## Tier 2 — Build when ecosystem matures

| Item | Trigger |
|---|---|
| Observability Doctrine repo (promoted from `plausiden-obs` inline docs) | `plausiden-obs` has 3+ consumers AND ≥1 external user |
| Auditing Doctrine repo (promoted) | Audits has 3+ consumers OR rule-pack conflicts emerge |
| Testing Doctrine repo (promoted) | Same threshold as Auditing Doctrine |
| Canon: additional targets (Kotlin, iced, egui) | A specific consumer commits AND has budgeted time |
| Canon: contracts layer (full enforcement) | Canon has 5+ components AND ≥2 targets need parity enforcement |
| AVP-Doctrine upgrade to meta-doctrine | Three doctrine repos exist with amendments |
| Audits: runtime rules + autofix codemods | Static rules in production across 3+ consumers AND a runtime-only violation has caused incidents |
| Security Doctrine | Sacred.Vote first external security audit OR LFI confidentiality kernel v1.0 |
| Release Doctrine | About to cut v1.0 of any PlausiDen-namespace artifact AND have hit a versioning mishap |

---

## Tier 3 — Build only on concrete demand

All have design sketches in the blueprint conversations. None get built preemptively.

- **PlausiDen-Harvest tooling** — Trigger: 3+ consumers actively proposing harvest candidates. **Note (2026-04-24)**: scaffold exists; promote to Tier 0/1 only when trigger fires.
- **Plugin system + sandbox** — Trigger: 5+ rule packs exist AND a third party submits one
- **PlausiDen-Registry** — Trigger: 10+ plugins
- **PlausiDen-Forge meta-CLI** — Trigger: consumer feedback explicitly cites "too many CLIs"
- **Editor LSP integrations** — Trigger: Audits has 15+ rules AND personal CI-violation friction is real
- **Playground / web UI** — Trigger: external-adopter acquisition becomes a goal
- **Incidents repo** — Trigger: 5+ doctrine amendments have been filed
- **Examples repo** — Trigger: external adoption goal OR repeated doctrine questions wishing for a reference
- **Docs site (`plausiden.dev`)** — Trigger: external adoption goal AND 3+ doctrine repos with published amendments
- **Adoption dashboard** — Trigger: 5+ consumer repos exist AND drift is visible without tooling
- **Release Artifact / Dependency / Documentation / API Design / Data doctrines** — Trigger per-doctrine: a concrete cross-repo incident the doctrine would have prevented
- **Accessibility / i18n doctrines** — Trigger: first consumer with concrete a11y/i18n requirement (Sacred.Vote likely)
- **Plausible Deniability / PSA / Anti-Fingerprinting doctrines** — Trigger: Protection Suite v1.0 release AND cross-component consistency needed
- **Corpus Hygiene Doctrine** — Trigger: LFI ingests production corpus data (likely Tier 2 within 6 months)

---

## Tier 4 — External-consumption product mode only

Trigger for this entire tier: **explicit decision to market PlausiDen tooling outside your own projects.**

Multi-tenant doctrine inheritance, supply-chain attestation (SLSA, SBOM, signed releases), OCSF alignment, OpenTelemetry remote export, chat integrations, ticket integrations, vulnerability scanner integrations beyond Sentinel, formal verification adapters (Kani, Prusti, TLA+, Lean, Coq, individually gated), mutation testing adapters, ephemeral environments (Nix flakes), reproducible builds, plugin WASM, economic analysis, violation clustering, predictive drift detection (HDC-based), temporal reasoning, counterfactual analysis, doctrine coverage metrics, social-layer doctrines, license compliance, export-control stubs, legal-surface doctrine, project i18n.

---

## Shelved indefinitely

- Dedicated `PlausiDen-Doctrine-Core` shared-types crate (use duplicated types until pain emerges)
- Pre-commit hook packaging (a 10-line shell script suffices)
- Shell completions (free from clap if/when forge CLI exists)
- Matrix-aware CI execution (solve when you have the problem)
- Distributed audit execution (solve when audits take >60s on largest repo)
- Rule compilation caching (solve when cold-start exceeds 10s)
- Benchmark-as-CI-gate for the audit engine itself

---

## Decision log

| Date | Decision | Rationale |
|---|---|---|
| 2026-04-24 | Tier 0 = `plausiden-obs` only | Highest leverage per LFI line count; smallest scope; pays back from line one |
| 2026-04-24 | Canon React target deferred to Tier 1 | No active UI consumer this week; Sacred.Vote UI work not current focus |
| 2026-04-24 | Harvest tooling deferred to Tier 3 | Insufficient consumer count; manual PRs sufficient |
| 2026-04-24 | All non-core doctrines shelved to Tier 2–4 | No concrete incident-driven demand yet |
| 2026-04-24 | Built-ahead-of-trigger: Canon/Tests/Observability/Harvest scaffolds shipped before this priority doc was committed | Acknowledged in respective READMEs as "early scaffolds awaiting trigger." Not a precedent — future builds wait for triggers. |

---

**Next concrete action**: spin up `plausiden-obs` as a Cargo crate this weekend, wire it into one LFI crate and one Protection Suite crate, measure debug-friction reduction over 30 days. That's the Tier 0 commitment; everything else waits.
