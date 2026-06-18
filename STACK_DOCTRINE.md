<!-- repo-doc: stack-doctrine -->
<!-- doc-class: ecosystem-wide best-language-per-domain reference + language-selection doctrine -->
<!-- doc-doctrine-version: 1.0.0 -->
<!-- doc-status: living -->
<!-- doc-target-stack-scope: any -->
<!-- doc-companions: SECURITY_BASELINE.md (what guarantees), SUPERSOCIETY_BASELINE.md (sovereignty demands), this (which tool) -->

# 🧭 The Sovereign Polyglot Stack

![doctrine](https://img.shields.io/badge/doctrine-living-2ea44f) ![scope](https://img.shields.io/badge/scope-ecosystem--wide-blue) ![stack](https://img.shields.io/badge/stack--scope-any-informational) ![cadence](https://img.shields.io/badge/review-quarterly-orange) ![license](https://img.shields.io/badge/license-MIT-black)

**A best-language-per-domain reference. Snapshot — not prophecy.**

> **Version:** 2026.06-r3 · **Maintainer:** William Armstrong / PlausiDen · **Review cadence:** quarterly.
> Language dominance churns on a ~10–20yr cycle; treat every "best" below as a *current estimate*, not a permanent fact.

> 📜 **Doctrine status.** This is the **third leg of the PlausiDen consumer floor**, alongside
> [`SECURITY_BASELINE.md`](SECURITY_BASELINE.md) (the 16 *what-guarantees* demands) and
> [`SUPERSOCIETY_BASELINE.md`](SUPERSOCIETY_BASELINE.md) (the 15 *sovereignty/longevity* demands).
> Those two say **what** every consumer must guarantee; this one says **which tool** to reach for per domain.
> It is **advisory, not a gate** — but a repo choosing against it should be able to say why (price the capture risk; see [Risk / steel-man](#-risk--steel-man)).

---

## 🚦 How to use this

**If you are an AI agent (or human) starting or extending a PlausiDen repo:**

1. Find your domain in the [layer index](#-layer-index) → jump to the layer.
2. Take the **Primary** unless a documented constraint forces the Fallback. Note the **governance flag** (🟢/🟡/🔴) — a 🔴 choice is a *forced* dependency to keep thin and swappable, not a default.
3. Cross-check the [PlausiDen ecosystem mapping](#-plausiden-ecosystem-mapping) for a repo that already instantiates that choice — copy its pattern instead of re-deriving.
4. If you deviate from a Primary, record *why* (the capture-cost calculation) in your repo's decision log.

The durable unit is the **interface** (WASM, protobuf, SQL, wire protocols), not the language. Choose languages so interfaces stay stable while implementations swap underneath.

---

## 🗂️ Layer index

| # | Layer | Spine choice |
|---|---|---|
| [0](#layer-0--silicon--hardware) | Silicon & Hardware | SystemVerilog 🔴 / Chisel 🟢 (open-silicon) |
| [1](#layer-1--firmware-bare-metal-embedded) | Firmware / Bare-metal / Embedded | **Rust** 🟢 · Ada/SPARK 🟢 (safety-critical) |
| [2](#layer-2--os--kernel) | OS / Kernel | C + **Rust** 🟢 · seL4 (formal) |
| [3](#layer-3--systems--performance-critical) | Systems & Performance | **Rust** 🟢 |
| [4](#layer-4--backend-servers-concurrency) | Backend / Servers / Concurrency | **Rust** (Axum) 🟢 · **BEAM** 🟢 |
| [5](#layer-5--data--databases) | Data & Databases | **SQL** 🟢 · Rust storage engines |
| [6](#layer-6--data-engineering-ml-scientific) | Data Eng / ML / Scientific | **Python** 🟢 (train) · **Rust** 🟢 (infer) |
| [7](#layer-7--mobile-gatekept--mitigate) | Mobile (gatekept) | **Rust + UniFFI** 🟢 core; thin Kotlin/Swift shells |
| [8](#layer-8--desktop) | Desktop | **Tauri** 🟢 |
| [9](#layer-9--web-frontend-captive-platform-the-browser) | Web Frontend | **TypeScript** 🟢 → **Rust→WASM** 🟢 |
| [10](#layer-10--cli-scripting-shell) | CLI / Scripting / Shell | **Rust** 🟢 · Python 🟢 · Bash 🟢 |
| [11](#layer-11--infra-as-code--config) | Infra-as-Code & Config | **Nix** 🟢 · **OpenTofu** 🟢 |
| [11b](#layer-11b--config-as-policy-format-neutral-security-doctrine) | Config-as-Policy | format-neutral policy doctrine |
| [12](#layer-12--blockchain--zk) | Blockchain & ZK | **Rust** 🟢 (zkVM) · Noir/Cairo 🟢 |
| [13](#layer-13--formal-methods--correctness) | Formal Methods | **Lean 4** 🟢 · **TLA+** 🟢 |
| [14](#layer-14--security--re--offensive) | Security / RE / Offensive | **Rust** 🟢 / Python 🟢 |
| [15](#layer-15--game-dev-if-ever-relevant) | Game Dev | Godot 🟢 / Bevy 🟢 |

---

## 🎯 Selection axes

1. **Fitness** — is it actually the best tool for the domain, ignoring everything else?
2. **Governance / capture risk** — who controls it? Single vendor (high risk) → foundation (low risk) → captive platform (forced, mitigate). This is a first-class axis here, not a footnote.
3. **Maturity** — production-ready, or watchlist?

### Legend

| Flag | Meaning |
|:---:|---|
| 🟢 | FOSS + foundation/community-governed |
| 🟡 | FOSS but single-vendor-steered |
| 🔴 | captive/closed or vendor-gatekept platform |
| ⏳ | pre-1.0 / not load-bearing yet |

The through-line: **logic lives in Rust; captive platform languages are thin, swappable UI shells (UniFFI). Interfaces (WASM, protobuf, SQL, wire protocols) are the durable unit — not languages.**

A corollary for doctrine/Canon: **security and correctness doctrine attaches to the policy *class* (process sandbox, network ingress, secret handling), never to the file format that happens to express it.** A rule tied to "Caddyfile" dies the moment you swap Caddy; a rule tied to "the ingress class" survives the swap. Config formats are not languages and don't get language doctrine — they get policy doctrine, and the policy is portable across formats.

---

## Layer 0 — Silicon & Hardware

| Domain | Primary | Fallback / Legacy | Watch | Notes |
|---|---|---|---|---|
| RTL / chip design | SystemVerilog 🔴 (IEEE std) | VHDL 🔴 | **Chisel** 🟢 (Scala-based, runs RISC-V/SiFive), Amaranth 🟢 (Python), Veryl 🟢 ⏳ | FOSS toolchain exists: Yosys + Verilator + nextpnr. The open-silicon path. |
| FPGA | SystemVerilog / VHDL | — | Chisel, SpinalHDL 🟢 | Same toolchain story. |
| HW verification | SystemVerilog + UVM 🔴 | — | SymbiYosys 🟢 (formal) | Formal verification of RTL is FOSS-viable now. |
| GPU kernels | CUDA C++ 🔴 (NVIDIA-captive) | OpenCL | **wgpu/WGSL** 🟢 (Rust), ROCm/HIP 🟡, Mojo 🔴 | CUDA is the deepest moat in computing. wgpu is the sovereign escape, slower today. |

## Layer 1 — Firmware, Bare-Metal, Embedded

| Domain | Primary | Fallback / Legacy | Watch | Notes |
|---|---|---|---|---|
| MCU / bare-metal | **Rust** 🟢 (embassy async) | C 🟢 (widest vendor support) | Zig 🟢 ⏳ | Rust is the future; C still wins raw breadth of vendor toolchains. |
| RTOS | C 🟢 (Zephyr, FreeRTOS) | — | Rust-on-Zephyr | — |
| Safety-critical (avionics/auto/med) | **Ada/SPARK** 🟢 (provable, DO-178C) | MISRA C | **Rust via Ferrocene** 🟢 (now ISO 26262 / IEC 61508 qualified) | Qualified Rust toolchain is the genuinely new thing. SPARK if you need machine-checked proofs. |
| DSP | C/C++ + intrinsics | — | Rust | — |

## Layer 2 — OS / Kernel

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Kernel | C 🟢 (Linux) + **Rust** 🟢 (mainline) | — | Redox OS 🟢 (Rust microkernel) | Rust-for-Linux is merged; the safe-systems debate is over. |
| Microkernel / formal | C + **Isabelle/HOL** proofs (seL4) | — | Rust + capability IPC | The proof corpus is the asset. Don't rewrite seL4; build on its guarantees. (PlausiDenOS target.) |

## Layer 3 — Systems & Performance-Critical

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Systems / perf services | **Rust** 🟢 | C++ 🟢 (where ecosystem forces it: games, HPC, legacy) | Zig ⏳ | Default. |
| Crypto / security tooling | **Rust** 🟢 | C (libsodium, audited) | F*/hax 🟢 (Rust→F* extraction) | "Proven" > "tested" for an adversarial product. |

## Layer 4 — Backend, Servers, Concurrency

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Concurrent / fault-tolerant / realtime | **Elixir/Erlang (BEAM)** 🟢 | — | **Gleam** 🟢 (typed BEAM, 1.0) | Nothing else has the supervision/distribution model. Gleam = OTP + static types; the typed-BEAM future. |
| General API / web backend | **Rust** (Axum) 🟢 | Elixir/Phoenix, Gleam, Go 🟡 | — | CPU/latency-bound → Rust; connection-bound → BEAM. |
| Ops/infra services | **Go** 🟡 | Rust | — | The k8s/cloud-native ecosystem is Go; fight it only with reason. |
| Wire protocols | protobuf/gRPC, Cap'n Proto 🟢 | — | — | The durable interface layer. Language-agnostic by design. |

## Layer 5 — Data & Databases

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Querying data | **SQL** 🟢 | — | — | The single most future-proof language in existence. 50yr, outlived every paradigm. Bet hard on it. |
| Stored procedures | PL/pgSQL 🟢 (Postgres) | — | — | Your CRM/Salesman backend. |
| Building a storage engine | **Rust** 🟢 | C++ 🟢 (Postgres/SQLite legacy) | — | New DBs are overwhelmingly Rust. |
| Embedded DB | SQLite (C) 🟢 | — | — | Most-deployed DB on Earth. |
| Advanced query / reasoning | Datalog 🟢 | — | — | For graph/recursive queries; relevant to neurosymbolic work. |

## Layer 6 — Data Engineering, ML, Scientific

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| ML/AI research & training | **Python** 🟢 (PyTorch/JAX) | — | — | Lingua franca. Unavoidable. Don't fight it; wrap it. |
| Production ML inference | **Rust** 🟢 (candle, burn) | Python | Mojo 🔴 (disqualified) | Fits LFI's Rust HDC core directly. |
| Scientific / numerical | **Julia** 🟢 | Python | — | Modern FOSS challenger; fast, MIT. |
| Dense numerics / HPC | **Fortran** 🟢 | C++ | Julia | Not a joke — still optimal for dense linear algebra/HPC. The incumbent that refuses to die because it's correct. |
| Dataframes / analytics | SQL + **Polars** 🟢 (Rust) | pandas | — | — |
| Mojo status | — | — | — | 🔴 1.0 Beta (May 2026) but **compiler is closed** (Modular Community License), single-vendor. Technically exciting, fails the FOSS/PSA filter. Revisit only if the compiler is OSI-licensed + foundation-governed. |

## Layer 7 — Mobile (gatekept — mitigate)

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Shared logic core | **Rust + UniFFI** 🟢 | — | — | **The sovereign move.** Real logic in Rust, exposed to both platforms. Native languages become thin shells. |
| Android UI | Kotlin 🟡 (JetBrains/Google) | Java | — | Apache-licensed but Google-steered platform. |
| iOS UI | Swift 🔴 (Apple-gatekept) | Obj-C | — | Swift-the-language is open; the platform/tooling is captive. Highest capture risk in the stack. |
| Cross-platform (one codebase) | **Flutter/Dart** 🟡 or KMP 🟡 | React Native/TS | Dioxus 🟢 (Rust) | Flutter is BSD-FOSS but Google-governed — license isn't the risk, governance capture is. |

## Layer 8 — Desktop

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Cross-platform (sovereign) | **Tauri** 🟢 (Rust core + web UI) | Qt 🟢/🟡 (C++) | Dioxus 🟢, Slint 🟢 | Tiny binaries, Rust core. Avoid Electron (bloat). |
| Linux native | C/C++ (GTK/Qt) 🟢 | Rust (gtk-rs) | Slint | — |
| Windows native | C# / .NET 🟡 (MIT, MS-steered) | C++ Win32 | — | .NET is FOSS now; still MS-directed. |
| macOS native | Swift/SwiftUI 🔴 | — | — | Captive. |

## Layer 9 — Web Frontend (captive platform: the browser)

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Production web UI | **TypeScript** 🟢 + Svelte/Solid | React | — | The browser is a captured runtime; TS is the floor of sanity. Treat as legacy-you-can't-escape-yet. |
| Sovereign / WASM future | **Rust→WASM** 🟢 (Leptos, Dioxus) | Gleam→JS | WASM Component Model 🟢 | Migration target off React. Push logic into WASM behind a stable component boundary. (Sacred.Vote's TS/React stays until this matures.) |

## Layer 10 — CLI, Scripting, Shell

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| CLI tools | **Rust** (clap) 🟢 | **Go** (cobra) 🟡 | — | Genuine tie. Most cloud CLIs are Go; Rust wins on perf/correctness. |
| Glue / automation | **Python** 🟢 | — | — | Ecosystem reach. Unavoidable. |
| Shell scripting | Bash 🟢 (portability floor) | — | **Nushell** 🟢 (Rust, structured data), fish | Nushell is the modern structured-data shell. |

## Layer 11 — Infra-as-Code & Config

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Reproducible systems | **Nix / NixOS** 🟢 | — | — | The supersociety answer for reproducible infra. But Nix is a **real lazy-functional language with real bug surface** — see doctrine below. Treat it as code, not config. |
| Cloud provisioning | **OpenTofu** 🟢 (HCL) | — | — | **Use OpenTofu, not Terraform.** HashiCorp's BSL relicense is the exact vendor-capture event your filter exists to prevent; OpenTofu is the Linux Foundation FOSS fork. Case study in why governance is a selection axis. |
| Typed config | **Nickel** 🟢 (Rust/Nix ecosystem) | CUE 🟢, Dhall 🟢 | — | Escape YAML. |

### Nix language doctrine (it's a language, give it one)

Lazy, dynamically typed, impure-at-the-edges. The footguns are not where intuition points:

- **Headline risk — the store is world-readable.** Any secret string-interpolated into a derivation, `builtins.readFile`'d into config, or otherwise touched during evaluation lands in `/nix/store` readable by *every local user*. This is the AVP-2-relevant Nix failure, worse than shell injection. **Mandate out-of-store secrets (sops-nix or agenix); forbid secret material in derivation inputs entirely.** Assume-breach + blast-radius both bite here.
- **`with` is the real scoping footgun, not `let … in`.** `let … in` is lexically scoped and well-behaved. `with pkgs;` introduces names that don't shadow lexical bindings predictably — silently masking or failing to mask, producing wrong-but-evaluating configs. Constrain or ban `with` at module scope.
- **`rec` self-reference** → accidental infinite recursion. Prefer explicit `let` bindings.
- **Type-tighten `mkOption`.** Unconstrained option types (`types.attrs`, raw `types.unspecified`) defeat the module system's only real safety check. Use precise submodule types.
- **IFD (import-from-derivation) discipline** — IFD serializes evaluation and hides build steps inside eval; ban it in CI-critical paths.
- **`pkgs.runCommand` / `writeShellScript`** are shell-injection surface — quote and validate inputs as you would any `sh -c`.

## Layer 11b — Config-as-Policy (format-neutral security doctrine)

These are **not languages** and don't get language doctrine. They encode security policy, and the doctrine attaches to the **policy class** — so it survives swapping the tool that expresses it. Two classes matter most:

**Process-sandbox class** — instantiated by systemd units (`[Service]` directives), seccomp-BPF, AppArmor/SELinux profiles, K8s SecurityContext, OCI runtime configs. Portable hardening baseline, regardless of format:
- W^X: `MemoryDenyWriteExecute=yes` (no writable+executable pages)
- Syscall allow-listing: `SystemCallFilter=` (default-deny, allow the minimum)
- `NoNewPrivileges=yes`, `ProtectSystem=strict`, `ProtectHome=yes`
- Capability bounding: drop all, add back the minimum (`CapabilityBoundingSet=`)
- Namespace isolation: `PrivateTmp`, `PrivateDevices`, `RestrictNamespaces`
- These map 1:1 onto seccomp/AppArmor/SecurityContext when you migrate off systemd — write the doctrine against the *capabilities*, not the directive names.

**Network-ingress class** — instantiated by Caddyfile, nginx.conf, Envoy/Traefik config, nftables rules, K8s NetworkPolicy. Portable baseline:
- TLS floor (1.3-only where feasible), HSTS, OCSP stapling
- Security headers: CSP, `X-Content-Type-Options`, `Referrer-Policy`, frame-ancestors
- Default-deny ingress; explicit allow per route/port
- Rate-limiting + connection caps at the edge
- These survive a Caddy→nginx→Envoy swap unchanged because they're stated as ingress guarantees, not Caddy syntax.

Canon enforcement: lint the *policy*, not the file. A check that asserts "every long-running service has W^X + syscall filter + no-new-privs" applies whether the unit is systemd today or a K8s SecurityContext tomorrow. This is exactly the stack-neutral enforcement substrate the [`PlausiDen-Canon`](https://github.com/thepictishbeast/PlausiDen-Canon) repo is supposed to be, and it dovetails with [`SECURITY_BASELINE.md`](SECURITY_BASELINE.md).

## Layer 12 — Blockchain & ZK

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Chain logic / zkVM | **Rust** 🟢 (SP1, RISC Zero, Substrate, Solana) | — | — | Already your stack. Correct. |
| EVM contracts | Solidity 🟢 | Vyper 🟢 | — | Unavoidable for Ethereum. |
| ZK circuits | **Noir** 🟢, **Cairo** 🟢 (Starknet/STARK) | Circom, Halo2 (Rust) | — | Cairo aligns with your STARK-only/SP1 posture. |

## Layer 13 — Formal Methods & Correctness

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Theorem proving / verified programs | **Lean 4** 🟢 | Rocq (Coq) 🟢, Agda 🟢, Idris 2 🟢 | — | Lean is now a real programming language, not just a prover. |
| Verified systems code | **Verus / Creusot / Aeneas** 🟢 (Rust) | SPARK/Ada 🟢 | — | Prove your actual production code, not a model of it. |
| Protocol / consensus design | **TLA+** 🟢 | Quint 🟢 (modern TLA+), Alloy | — | **Spec the dual-chain consensus and Sacred.Vote tally protocol in TLA+/Quint before writing Rust.** Catches the bugs tests never will. |

## Layer 14 — Security / RE / Offensive

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Tooling | **Rust** 🟢 / C | — | — | — |
| Exploit PoC / scripting | Python 🟢 | — | — | — |
| Reverse engineering | Assembly (x86/ARM) + Ghidra 🟢 (Java platform) | — | — | — |

## Layer 15 — Game Dev (if ever relevant)

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| FOSS engine | **Godot/GDScript** 🟢 or **Bevy/Rust** 🟢 | — | — | Avoid Unity (proprietary, licensing-volatile) and Unreal/C++ unless AAA. |

---

## 🗺️ PlausiDen ecosystem mapping

Where the doctrine is *already instantiated*. Use these as worked examples — copy the pattern, don't re-derive it. (Build status per [`PRIORITY.md`](PRIORITY.md); many are scaffolds, not production.)

| Layer | Choice | Instantiated by |
|---|---|---|
| 1 — Embedded | Rust + Zig hardware layer | [`PlausiDen-USB`](https://github.com/thepictishbeast/PlausiDen-USB) (RP2040→iCE40 FPGA), [`PlausiDen-Shard`](https://github.com/thepictishbeast/PlausiDen-Shard) (`no_std`) |
| 2 — Microkernel | seL4 + capability isolation | [`PlausiDenOS`](https://github.com/thepictishbeast/PlausiDenOS), [`PlausiDen-OS-for-Mobile`](https://github.com/thepictishbeast/PlausiDen-OS-for-Mobile) |
| 3 — Systems / crypto | **Rust** | the spine of nearly every repo — [`PlausiDen-Engine`](https://github.com/thepictishbeast/PlausiDen-Engine), [`PlausiDen-Shard`](https://github.com/thepictishbeast/PlausiDen-Shard), [`PlausiDen-Auth`](https://github.com/thepictishbeast/PlausiDen-Auth), [`PlausiDen-Vault`](https://github.com/thepictishbeast/PlausiDen-Vault) |
| 3 — Perf + eBPF/DPI | Rust + Zig | [`PlausiDen-Firewall`](https://github.com/thepictishbeast/PlausiDen-Firewall), [`PlausiDen-PDFS`](https://github.com/thepictishbeast/PlausiDen-PDFS) (Zig kernel + Rust FUSE) |
| 4 — API backend | **Rust + Axum** | [`plausiden.com`](https://github.com/thepictishbeast/plausiden.com) (Axum+Maud), [`sacredvote-axum-poc`](https://github.com/thepictishbeast/sacredvote-axum-poc), [`Tempered-Studio`](https://github.com/thepictishbeast/Tempered-Studio) (`rpro-serve`) |
| 4 — Concurrent / control plane | **Elixir (BEAM)** + Rust | [`PlausiDen-Shield`](https://github.com/thepictishbeast/PlausiDen-Shield) (Rust + Elixir control plane) |
| 5 — Data | **SQL** + PL/pgSQL | [`PlausiDen-CRM`](https://github.com/thepictishbeast/PlausiDen-CRM), [`PlausiDen-Salesman`](https://github.com/thepictishbeast/PlausiDen-Salesman) (Postgres LISTEN/NOTIFY) |
| 6 — ML train / infer | **Python** train → **Rust** HDC/VSA infer | [`PlausiDen-AI`](https://github.com/thepictishbeast/PlausiDen-AI), [`PlausiDen-LFI`](https://github.com/thepictishbeast/PlausiDen-LFI), [`Neurosymbolic-Toolkit`](https://github.com/thepictishbeast/Neurosymbolic-Toolkit), [`PlausiDen-GraphNet`](https://github.com/thepictishbeast/PlausiDen-GraphNet) |
| 7 — Mobile | Rust core + thin Kotlin shell | [`PlausiDen-Android`](https://github.com/thepictishbeast/PlausiDen-Android) (Rust + Kotlin) |
| 8 — Desktop | **Tauri** (Rust core + web UI) | [`PlausiDen-Desktop`](https://github.com/thepictishbeast/PlausiDen-Desktop), [`PlausiDen-Atrium`](https://github.com/thepictishbeast/PlausiDen-Atrium), [`SacredVote-Desktop`](https://github.com/thepictishbeast/SacredVote-Desktop) |
| 9 — Web (WASM future) | **Rust→WASM (Leptos)** | [`Linux-File-Manager`](https://github.com/thepictishbeast/Linux-File-Manager-and-UI-for-Remote-Access-) (Leptos WASM + Axum), [`PlausiDen-Browser-Ext`](https://github.com/thepictishbeast/PlausiDen-Browser-Ext) (engine→WASM) |
| 10 — CLI | **Rust (clap)** | [`claude-tools`](https://github.com/thepictishbeast/claude-tools), [`claude-secrets`](https://github.com/thepictishbeast/claude-secrets), most repos' admin CLIs |
| 11 — IaC / config | **Nix flakes + OpenTofu + sops/age** | [`PlausiDen-DevOps`](https://github.com/thepictishbeast/PlausiDen-DevOps) |
| 11b — Config-as-policy | systemd hardening + Caddy ingress | [`PlausiDen-Vaultwarden`](https://github.com/thepictishbeast/PlausiDen-Vaultwarden), mail-stack overlays |
| 12 — ZK / chain | **Rust zkVM (SP1, STARK) + Belenios** | [`Post-Quantum-Belenios`](https://github.com/thepictishbeast/Post-Quantum-Belenios), [`sacredvote-pq-bench`](https://github.com/thepictishbeast/sacredvote-pq-bench), [`Sacred.Vote`](https://github.com/thepictishbeast/Sacred.Vote) |
| 13 — Formal methods | TLA+/Quint spec before Rust | target for [`Sacred.Vote`](https://github.com/thepictishbeast/Sacred.Vote) consensus/tally (per Layer 13) |
| 14 — Security / RE | **Rust** tooling + Python PoC | [`PlausiDen-Sentinel`](https://github.com/thepictishbeast/PlausiDen-Sentinel), [`Vulnerability-Scanner`](https://github.com/thepictishbeast/Vulnerability-Scanner) |

> The closest sibling to this doc is [`PlausiDen-AI/docs/SUPERSOCIETY_STACK.md`](https://github.com/thepictishbeast/PlausiDen-AI/blob/main/docs/SUPERSOCIETY_STACK.md) — a *product-specific* migration/architecture plan for the AI app. **This doc is the ecosystem-wide reference; that one is one repo's instantiation.** They cross-reference each other.

---

## 🪨 The irreducible core

If you stripped this to the minimum set that covers ~90% of all development, sovereignty-weighted:

- **Rust** — systems, embedded, crypto, backend, CLI, ML-inference, ZK, WASM, desktop core. The spine.
- **Python** — ML/AI and glue. Wrapped, not loved. Unavoidable.
- **SQL** — all data. The most durable language alive.
- **Elixir / Gleam (BEAM)** — concurrency & fault tolerance.
- **TypeScript** — browser floor, until WASM displaces it.
- **C** — firmware/kernel incumbent.
- **Lean 4 + TLA+** — correctness frontier.
- **Nix** — reproducible infra.
- **Kotlin + Swift** — mobile UI shells only, forced by gatekeepers, kept thin over a Rust core.

That's the honest floor: ~9 languages, three of them (TS, Swift, Kotlin) forced on you by captured platforms rather than chosen.

## 🛡️ Risk / steel-man

The strongest objection: a sovereignty-weighted stack systematically *under-weights ecosystem gravity*, and ecosystem is itself a material force. Choosing OpenTofu over Terraform, wgpu over CUDA, or Tauri over Electron is correct on governance grounds and pays a real, ongoing tax in tooling maturity, talent availability, and integration friction. Solo, that tax is affordable and the sovereignty is worth more. The moment there's a team or a delivery deadline that the FOSS option can't hit, the stack has to be re-derived against the new material conditions — the captured tool sometimes wins because the cost of avoiding it exceeds the cost of depending on it. The discipline isn't "always pick FOSS"; it's "price the capture risk honestly and pay it deliberately, not by default." Re-run that calculation each time you update this doc.

---

## 🔗 Relationship to PlausiDen doctrine

- [`SECURITY_BASELINE.md`](SECURITY_BASELINE.md) — the *what-guarantees* leg (16 stack-neutral demands). Layer 11b feeds it directly.
- [`SUPERSOCIETY_BASELINE.md`](SUPERSOCIETY_BASELINE.md) — the *sovereignty/longevity* leg (15 demands). This doc's governance axis is its language-level expression.
- [`PlausiDen-AVP-Doctrine`](https://github.com/thepictishbeast/PlausiDen-AVP-Doctrine) `gates/{rust,python,typescript,frontend}.md` — *how* to write each language well once chosen. This doc is *which* to choose; those gates are *how* to wield it.
- [`PRIORITY.md`](PRIORITY.md) — whether a given consumer should be built at all.

## 🗓️ Changelog & cadence

- **2026.06-r3** — Adopted into PlausiDen-Meta as ecosystem doctrine. Added: layer index + nav, how-to-use, the PlausiDen ecosystem mapping, doctrine cross-references (baselines, AVP gates), badges, and a cross-link to `PlausiDen-AI/docs/SUPERSOCIETY_STACK.md`. Layer content unchanged from r2.
- **2026.05-r2** — Expanded Nix from a one-liner to a real language-doctrine entry; added Layer 11b (Config-as-Policy); added the "doctrine attaches to the policy class, not the file format" principle.

**Cadence:** reviewed quarterly (next: 2026-09). Amendments tracked in [`CHANGELOG.toml`](CHANGELOG.toml); decisions in [`DECISION_LOG.md`](DECISION_LOG.md). Language dominance churns on a decade cycle — this is a snapshot, re-derive it as material conditions change.
