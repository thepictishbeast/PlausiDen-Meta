# Security Baseline

PlausiDen consumers must satisfy the following security properties
**regardless of stack**. Each item is a *what*; the *how* is per-language /
per-platform.

## 1. Secret values are type-refused at the language level

Every language has a way to express "this value must not appear in logs,
debug output, or wire formats." Examples:

| Language | Mechanism |
|---|---|
| Rust | newtype wrapper with no `Display` / `Debug` / `Serialize` impl (e.g., `plausiden-obs::Secret<T>`) |
| TypeScript | branded type with no `toString` / `JSON.stringify` access; redactor at logger |
| Kotlin | inline value class with custom `toString` returning "[REDACTED]" |
| Python | `__repr__` / `__str__` overrides + `__slots__` to prevent reflection |
| Go | unexported field + `MarshalJSON` returning redacted value |
| Swift | property wrapper with no `CustomStringConvertible` |
| Java | dedicated `Secret<T>` class with overridden `toString()` |

The implementation varies; the requirement does not. A consumer where
`println!("{}", api_key)` compiles is not security-baseline-compliant.

## 2. Untrusted input passes through a declared validation boundary

Every value entering the trust boundary (HTTP body, file content, env var,
IPC message, CLI argument) is parsed by a typed validator before reaching
business logic. No `unwrap()` on user input. No `eval()`. No string
concatenation into shell commands or SQL queries.

Per-language tools: `serde` (Rust), `zod` / `valibot` (TS), `kotlinx.serialization` (Kotlin), `pydantic` / `attrs` (Python), `parser-combinators` family universally.

## 3. Cryptographic primitives come from audited libraries

No hand-rolled crypto. No "I'll just XOR this for now." The doctrine names
**candidate** libraries per language; consumers may pick equivalents with
documented justification.

| Language | Recommended (non-exhaustive) |
|---|---|
| Rust | `ring`, `rustls`, `dalek-cryptography`, `RustCrypto` |
| TypeScript / JS | Web Crypto API, `noble-cryptography`, `libsodium-wrappers` |
| Kotlin / JVM | `BouncyCastle`, `Tink` |
| Python | `cryptography`, `pynacl` |
| Go | stdlib `crypto/*`, `nacl` |
| Swift | `CryptoKit`, `swift-crypto` |
| C/C++ | `libsodium`, `mbedtls`, `BoringSSL` |

## 4. No hardcoded credentials

Secrets come from a declared capability source — env var (with documented
provenance), keystore, capability handle, broker API. Hardcoded credentials
in source, config, or test fixtures are a doctrine violation.

## 5. Memory safety

Memory-safe languages (Rust, Go, Swift, Kotlin, JVM family, Python, Ruby,
JS, OCaml, Haskell, etc.) get this for free. C / C++ / unsafe-Rust /
inline-asm consumers must justify why a safer language was not used and
must apply:

- Sanitizers (ASan / UBSan / TSan / MSan) in CI.
- Static analysis (clang-tidy, Coverity, PVS-Studio, or equivalent).
- Fuzzing on every parser / deserializer (libFuzzer / AFL++ / honggfuzz).
- Formal methods where critical (TrustInSoft, Frama-C, SPARK for Ada).

## 6. Constant-time operations on secret-comparing paths

Token comparison, MAC verification, password verification — never with
short-circuiting `==`. Per-language: `subtle::ConstantTimeEq` (Rust),
`crypto_verify_*` (libsodium), `MessageDigest.isEqual` (JVM, when used
correctly), platform-equivalent.

## 7. Dependency hygiene

- License-audited (no LGPL/GPL/AGPL pulled into MIT-distributed binaries
  unless explicitly justified and documented).
- Vulnerability-scanned in CI (`cargo audit`, `npm audit`, `pip-audit`,
  `gemnasium`, `osv-scanner`, equivalent).
- Reproducibly built where the ecosystem supports it (Nix, Bazel, deterministic Cargo, Reproducible Java).
- Dead deps removed; transitive surface kept minimal.

## 8. Supply-chain attestation at release

Every release artifact ships:

- SBOM (CycloneDX or SPDX format).
- Signed checksums (Sigstore / minisign / GPG).
- Provenance (SLSA level 2 or higher target).
- Reproducibility instructions where applicable.

## 9. TLS 1.3 only for outbound; no plaintext network protocols

If a consumer reaches the network, it does so over TLS 1.3 (or a deliberately-chosen newer post-quantum protocol). No plaintext HTTP, no plaintext SMTP, no plaintext FTP, no plaintext anything other than localhost loopback for development.

## 10. Authn before authz; deny by default

Every endpoint, IPC handler, or capability invocation authenticates the
caller before consulting authorization. Authorization defaults to **deny**;
the allow path is explicit, narrow, and audit-logged.

## 11. Audit trail for security-relevant operations

Every security-relevant operation (auth attempt, secret access, capability
delegation, privilege escalation, configuration change) emits a
tamper-evident audit event per the Observability Doctrine's audit-event
sink. Audit retention is policy-driven, not log-rotation-driven.

## 12. Input fuzzing on every parser

Every parser, deserializer, codec, or external-format ingester ships with a
fuzzing target that runs in CI for at least 1 minute per release. Crashes
are P0.

## 13. Threat model documented per system boundary

A `THREAT_MODEL.md` lives next to every system that crosses a trust
boundary. It enumerates: the boundary, the trusted assumptions, the
attacker capabilities considered, the in-scope mitigations, and the
explicitly-out-of-scope risks. STRIDE, LINDDUN, or equivalent framing.

## 14. Post-quantum migration path documented

Any cryptographic protocol specification ships a documented PQ migration
path. Implementation may follow later, but the *path* must be in writing
at design time. Reference: NIST PQC Round 4 finalists (Kyber, Dilithium,
SPHINCS+, Falcon).

## 15. No undocumented network destinations

Every outbound network destination is declared in a manifest. CI fails if
runtime-observed destinations diverge from the manifest.

## 16. Capability-based design where authorization is dynamic

Where role-based access control is insufficient (per-resource permissions,
delegation, time-bounded capability grants), use capability tokens
(macaroons, OAuth scopes done correctly, sigstore certificates with
constraints, biscuits) over expanding role hierarchies.

---

## Enforcement

The Auditing Doctrine ([`PlausiDen-Audits/DOCTRINE.md`](https://github.com/thepictishbeast/PlausiDen-Audits/blob/main/DOCTRINE.md))
specifies that rules in the security family are non-waivable by default.
Specific waivers require documented justification + ticket + expiry per the
waiver-ceremony tenet.

Conformance to this baseline is graded by the AVP-Doctrine
[`security`](https://github.com/thepictishbeast/PlausiDen-AVP-Doctrine)
tier (to be added when triggered per
[`PRIORITY.md`](PRIORITY.md)).
