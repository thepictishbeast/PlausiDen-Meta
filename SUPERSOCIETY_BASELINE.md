# Supersociety Baseline

PlausiDen's stance on sovereignty, longevity, and architectural ethics. These
are *opinionated demands*; together with the
[`SECURITY_BASELINE`](SECURITY_BASELINE.md) they distinguish PlausiDen from
bland generic standards bodies. Stack-neutral. Required of every consumer
regardless of language, platform, or framework.

## 1. Local-first

Every PlausiDen consumer must function with **zero external dependencies by
default**. Network access, cloud services, and third-party APIs are opt-in,
declared, and documented. The reference deployment is a single binary on a
single machine with no inbound or outbound traffic to a service the operator
does not control.

Cloud is a *feature*, not a *requirement*. A consumer that fails when
disconnected from a SaaS is not supersociety-compliant.

## 2. Sovereignty-respecting

The operator owns their data, their keys, their identity, and their compute.
PlausiDen artifacts:

- Never phone home (no telemetry by default; opt-in if at all).
- Never lock the operator into a vendor (data exportable in open formats; APIs documented; alternative clients welcomed).
- Never assume a cloud provider, identity provider, or payment processor.
- Never embed credentials, license-checks, or kill-switches.

If the maintainer of PlausiDen disappeared tomorrow, every consumer would
continue to function indefinitely.

## 3. Metadata-minimizing

Systems must declare *what metadata they emit* and *justify every item*.
Default to the minimum metadata that makes the system work; expansion
requires rationale.

Concretely:

- HTTP requests don't carry `User-Agent` strings that fingerprint the
  consumer beyond what's strictly necessary.
- Logs don't include IP addresses, timestamps, or principal IDs unless the
  Observability Doctrine vocabulary requires them.
- Crash reports don't include stack traces from the user's home directory
  paths or environment variables.
- Search queries are not silently aggregated.

## 4. Plausible deniability where applicable

For consumers handling coercion-relevant operations (voting, whistleblower
intake, journalist sources, dissident communication, financial privacy),
the system is architected such that **observable behavior does not uniquely
identify real behavior**. Noise floors, synthetic-data ratios, and
distinguishability bounds (target: AUC ≤ 0.55 for any classifier
distinguishing real from cover behavior) are part of the design.

This is not a feature flag — it is a structural property. Systems that need
plausible deniability must demonstrate it; systems that don't may skip this
section but must declare why.

## 5. Capability-based authorization

Where authorization is dynamic, use capabilities (macaroons, biscuits,
narrowly-scoped tokens with constraints) over expanding role hierarchies.
Capabilities are easier to reason about, easier to revoke, easier to
delegate without privilege escalation, easier to audit.

Role-based access control is acceptable for static, low-cardinality
authorization. The moment a per-resource per-action grant appears, switch
to capabilities.

## 6. Post-quantum readiness

Any cryptographic protocol specified by a PlausiDen consumer ships with a
documented PQ migration path. Implementation may follow when consensus
solidifies (Kyber, Dilithium, SPHINCS+, Falcon as of NIST PQC Round 4
finalists). The *path* must be in writing at design time.

## 7. Formal-methods-friendly

Critical components have a documented path toward formal verification, even
if not yet executed. The doctrine names candidate tools per language:

- Rust: Kani, Prusti, Creusot, Miri (model-checking aspect)
- C: Frama-C, TrustInSoft Analyzer, CBMC
- Functional / verified: Lean 4, Coq, F\*, Idris 2, Agda
- Concurrent / distributed: TLA+, P, Alloy
- Ada: SPARK

Selecting a tool from this list is acceptable; selecting an equivalent with
documented justification is also acceptable. Picking nothing is acceptable
*for non-critical components*. For critical components, the path must
exist.

## 8. Open and documented data formats

All persistent data formats are open and have a public specification.
Wire protocols are documented to the point a third-party implementation
could interoperate without reverse-engineering.

Proprietary or undocumented formats inside a system boundary are
acceptable; they leak across a system boundary only with explicit
documentation.

## 9. Plural-client support

APIs must support multiple client implementations. A system only usable
from one client library is a violation. The reference client is one of
many; alternative clients are first-class.

## 10. Open formats: timestamps, identifiers, monetary values

| Domain | Required form |
|---|---|
| Timestamps | RFC 3339 UTC (e.g., `2026-04-24T12:34:56Z`) |
| Identifiers | ULID (preferred), UUIDv7 (acceptable), other time-sortable formats with documented sort order |
| Money | Minor units (integer) + ISO 4217 currency code; no float |
| Text | UTF-8 |
| Newlines | LF (`\n`) |
| Byte order marks | Forbidden in any text file |
| Coordinates | WGS84 decimal degrees; lat-then-long order |

## 11. Inclusion baselines

These overlap with security-baseline accessibility but deserve their own
listing under the supersociety frame:

- WCAG 2.2 AA minimum for any user-facing surface.
- Keyboard-only operation as a required modality.
- Screen-reader compatibility verified (axe-core, equivalent).
- Motion-reduction support (`prefers-reduced-motion` honored).
- Color-contrast minimum 4.5:1 body / 3:1 large; never relies on color
  alone to convey information.
- i18n-capable from day one (string externalization, plural rules,
  RTL-safe primitives), even if only one locale ships initially.
- Reading-level conscious — error messages and onboarding text written for
  the median reader, not for the technically literate.

## 12. Process longevity

PlausiDen consumers are expected to be maintained for years, not quarters.
Architectural decisions favor:

- **Boring tech over novel tech** where boring tech meets the requirements.
- **Stable APIs over rapid evolution.** Breaking changes are doctrine
  events, not patch-version churn.
- **Reproducible builds** so a consumer can be rebuilt 5 years from now
  from source.
- **Complete documentation** as a first-class deliverable, not an
  afterthought.
- **Bus-factor mitigation** — at least one other person can maintain the
  system after the original author leaves.

## 13. Failure-mode honesty

Documentation, error messages, and status pages tell the user what's
broken, why, and what to do. No "an error occurred." No silent retries
that mask real failures. No dashboards that show "all systems operational"
when the system is degraded.

## 14. Anti-fingerprinting where applicable

Browser extensions, desktop apps, mobile apps, and web services minimize
fingerprint surface (per the metadata-minimization tenet) and document
their fingerprint budget. Where the consumer's purpose is plausible
deniability or anonymity, anti-fingerprinting is mandatory and verified.

## 15. Replaceable maintainership

The PlausiDen ecosystem itself adheres to this: any PlausiDen repo could
have its maintainer replaced and continue to function. Doctrine, governance,
and the priority gate ([`PRIORITY.md`](PRIORITY.md)) are designed so that
new contributors can ramp without depending on the originator's tacit
knowledge.

---

## What this baseline is *not*

- Not a complete security checklist (see [`SECURITY_BASELINE`](SECURITY_BASELINE.md) for that).
- Not vendor lock-out — third-party services may be used; they may not be required.
- Not a refusal of cloud computing — cloud is a deployment option among many.
- Not a refusal of proprietary tooling — but the consumer must remain functional without it.

## Enforcement

Conformance to this baseline is graded by AVP-Doctrine when the
corresponding tier ships (per [`PRIORITY.md`](PRIORITY.md) Tier 2 / 3
schedule). Until then, this document is a normative guide that maintainers
self-attest to in `integrations/avp.toml`.
