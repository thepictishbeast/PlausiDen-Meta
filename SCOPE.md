# Scope & Charter

PlausiDen is a generic FOSS ecosystem for building correctness-critical
software. Every PlausiDen-namespace repo (PlausiDen-Canon, -Audits, -Tests,
-Obs, -Harvest, -AVP-Doctrine, -Meta, plus this repo's siblings as they
emerge) exists to serve **any project** in **any language** on **any
platform** that wants doctrine-governed, sovereignty-respecting,
adversarially-validated infrastructure.

It is currently used by a small set of consumer projects (the maintainer's
own work). It is **not** designed for those consumers specifically. If a
PlausiDen artifact only makes sense in the context of a particular
consumer, it has been contaminated and must be generalized or relocated to
the consumer's own repo.

## Independence axes

PlausiDen is independent across:

1. **Consumers.** No PlausiDen artifact references a specific downstream project by name in its normative content. Examples may use placeholders; rationales must generalize beyond any single consumer.
2. **Languages.** Specifications are language-neutral. Reference implementations exist in pragmatic languages (Rust for engine-level components, TypeScript/Python where ecosystem dictates), but the specification is the canonical artifact and any language-conformant implementation is a legitimate alternative.
3. **Platforms.** Doctrines work on Linux, macOS, BSD, Windows, embedded, and microkernel targets. Where a platform-specific affordance is invoked, it is named as such and a fallback is documented.
4. **Frameworks.** Canon adapters target React, Compose, iced, egui, SwiftUI, ratatui, and any other UI substrate that arrives. Audits' detector classes are defined declaratively; per-language detectors are pluggable. Tests' contract runner dispatches to whichever per-language test framework the consumer chose.
5. **Deployment models.** Local-first by default. Cloud is opt-in; self-hosted is the reference; no PlausiDen artifact requires a third-party SaaS to function.

## Independence test

For every file in every PlausiDen repo:

> If a stranger cloned this repo tomorrow and knew nothing about the maintainer or the maintainer's specific projects — would this file make complete sense as a standalone artifact?

If yes: correctly scoped.
If no: contaminated; either generalize or relocate.

This is the same test the AVP `generality` tier enforces. See
[`PlausiDen-AVP-Doctrine/tiers/generality.md`](https://github.com/thepictishbeast/PlausiDen-AVP-Doctrine/blob/main/tiers/generality.md).

## What PlausiDen is opinionated about

Stack-neutral does not mean stance-neutral. PlausiDen requires specific
architectural properties of every consumer regardless of stack:

- **Security baseline** (see [`SECURITY_BASELINE.md`](SECURITY_BASELINE.md)) — secrets type-refused, untrusted input bounded by a validation layer, audited crypto only, supply-chain attestation at release.
- **Supersociety baseline** (see [`SUPERSOCIETY_BASELINE.md`](SUPERSOCIETY_BASELINE.md)) — local-first, sovereignty-respecting, post-quantum-ready, metadata-minimizing, capability-based authorization.
- **Accessibility baseline** — WCAG 2.2 AA minimum for any user-facing surface, keyboard-only operation, screen-reader compatibility, i18n-capable from day one.
- **Interoperability baseline** — open data formats, documented APIs, plural-client support, RFC 3339 UTC timestamps, ULIDs (or equivalent time-sortable IDs), UTF-8 + LF.

Specifications encode the *what*; per-stack adapters encode the *how*.

## What PlausiDen is not

- Not a personal toolkit. The maintainer's notebook lives elsewhere.
- Not a SaaS, paid product, or commercial offering. MIT-licensed; alternative
  implementations are welcomed, not forks-to-be-suppressed.
- Not a vendor-specific framework. Anthropic, OpenAI, Google, AWS, Azure,
  Cloudflare, GitHub, GitLab — none are required by any PlausiDen
  artifact's normative path. Where a vendor is named, it is one of several
  documented alternatives, never the only option.
- Not a single-language ecosystem. Rust is the pragmatic first-language
  choice for engine-level reference implementations; alternative-language
  implementations of the same specification are first-class peers.
- Not a Linux-only ecosystem. Reference implementations target POSIX where
  practical and document non-POSIX requirements.

## Governance independence

PlausiDen's governance ([`GOVERNANCE.md`](GOVERNANCE.md)) is documented
publicly. External contributors are first-class. As more contributors join,
maintainership expands per the doctrine; until then, the maintainer
substitutes for peer review via the public-comment-period mechanism.

## Reference implementation language choices

For pragmatic reasons (performance, single-binary distribution, memory
safety, ecosystem strength), the engine-level reference implementations
target Rust:

- `plausiden-obs` (Rust crate) — observability substrate
- `token-forge` (Rust binary) — token generator
- `contract-runner` (Rust binary) — test dispatcher
- `harvest-tool` (Rust binary) — harvest crawler
- `audit-tool` (mixed: Python + shell, per the Audits repo) — schema validators

These are reference implementations, not specifications. Alternative
implementations in Go, Zig, OCaml, Haskell, etc. are legitimate
contributions and would coexist with the Rust reference.

## Practical test before any commit

Apply the independence test (above) to every file you add or modify. If a
file fails the test, fix it before committing or move it out of the
PlausiDen namespace.

## Currently-known consumers

Listed in [`REPO_LABEL_REGISTRY.md`](REPO_LABEL_REGISTRY.md) → "Known
consumers" section. The list is informational; PlausiDen's design is
unaffected by which consumers happen to exist at any given time.
