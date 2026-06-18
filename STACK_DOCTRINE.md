<!-- repo-doc: stack-doctrine -->
<!-- doc-class: ecosystem-wide best-language-per-domain reference + language-selection doctrine -->
<!-- doc-doctrine-version: 1.0.0 -->
<!-- doc-status: living -->
<!-- doc-target-stack-scope: any -->
<!-- doc-companions: SECURITY_BASELINE.md (what guarantees), SUPERSOCIETY_BASELINE.md (sovereignty demands), this (which tool) -->

# 🧭 The Sovereign Polyglot Stack

![doctrine](https://img.shields.io/badge/doctrine-living-2ea44f) ![scope](https://img.shields.io/badge/scope-ecosystem--wide-blue) ![stack](https://img.shields.io/badge/stack--scope-any-informational) ![cadence](https://img.shields.io/badge/review-quarterly-orange) ![license](https://img.shields.io/badge/license-MIT-black)

**A best-language-per-domain reference. Snapshot — not prophecy.**

> **Version:** 2026.06-r4 · **Maintainer:** William Armstrong / PlausiDen · **Review cadence:** quarterly.
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

## 🧮 How to choose — the decision heuristic

*TL;DR — the ordered algorithm: fitness first, then governance flag, then maturity, then interop cost, then named tie-breaks — and record the reason.*

> **Price the capture risk honestly and pay it deliberately, not by default. A choice with no recorded reason is a default in disguise.**

The [Selection axes](#-selection-axes) say *what to weigh*; [How to use this](#-how-to-use-this) says *how to navigate the doc*. This is the third thing: the ordered algorithm an agent runs to pick a language for a domain not already pinned by a Primary. Run the steps in order; do not skip ahead. The output is a choice plus a recorded reason.

1. **Fitness first — pick the best tool for the domain as if governance didn't exist.** Match the problem's dominant constraint to the language built for it: memory-safe systems/perf → Rust; supervision/distribution/soft-realtime → BEAM; dense linear algebra → Fortran/Julia; querying relational data → SQL; machine-checked proofs → Lean 4 / SPARK; ML research/training → Python. If exactly one language is the obvious fit, you have a candidate — go to step 2. If two are close, hold both and let the tie-breaks in step 5 decide.
2. **Read the governance flag on the candidate.** 🟢 (FOSS + foundation/community-governed) → proceed, no penalty. 🟡 (FOSS but single-vendor-steered) → proceed, but note the steering vendor in your decision log; it is a watch item, not a block. 🔴 (captive/closed or vendor-gatekept) → **do not adopt as architecture.** A 🔴 that wins on fitness is a *forced dependency to keep thin and swappable, never a default* (the through-line: logic in Rust, captive platforms are thin UniFFI shells). If the 🔴 is avoidable, go to step 4 and prefer the escape. If it is genuinely unavoidable (no viable alternative exists — see the hinge rule below), you take it, wrap it behind a stable interface, and minimize the surface. Either way it does not become load-bearing logic.
3. **Check maturity.** If the candidate is ⏳ (pre-1.0 / not load-bearing), it cannot carry production weight yet — relegate it to the Watch column and select the matured Fallback for anything shipping. ⏳ is a bet to track, not a foundation to build on.
4. **Price the interop cost.** This operationalizes the through-line — *the interface is the durable unit, not the language* — as a tax line, not as a fourth canonical axis (the canonical axes remain the three above). A language that speaks the durable interfaces of [the interop spine](#-the-interop-spine) cheaply (UniFFI across the FFI boundary, WASM at the component boundary, protobuf/Cap'n Proto on the wire, SQL at the data boundary) costs little to adopt and little to *leave*. A language that forces a bespoke, non-portable boundary raises both adoption and exit cost — weight that against it. Cheap-to-leave beats locally-optimal-but-trapped, because this is a snapshot, not prophecy: today's best is next decade's legacy, and you will swap it.
5. **Break ties with the doc's named rules** (in priority order):
   - **Rust vs BEAM** → *CPU/latency-bound → Rust; connection-bound (massive concurrency, supervision, soft-realtime) → BEAM.* (Layer 4.)
   - **Rust vs Go** → Rust wins on perf/correctness; *concede to Go only where the ecosystem is overwhelmingly Go (k8s, cloud-native, ops/infra) — fight that gravity only with a documented reason.* (Layers 4, 10.) Go is 🟡; note the steering.
   - **A 🔴 candidate that won step 1 on fitness** → *take it, keep it thin and swappable* — never let it hold logic; expose it behind a durable interface so the implementation can be replaced without touching callers.
   - **Two 🟢 candidates still tied** → prefer the one already instantiated in the [PlausiDen ecosystem mapping](#-plausiden-ecosystem-mapping) (copy a worked pattern over re-deriving), then the one with the lower interop cost from step 4.
6. **Record the reason.** If you took anything other than the layer's Primary — or took a 🔴/🟡 at all — write the capture-cost calculation in your repo's decision log, exactly as [How to use this](#-how-to-use-this) and the [Risk / steel-man](#-risk--steel-man) require. *Price the capture risk honestly and pay it deliberately, not by default.* A choice with no recorded reason is a default in disguise.

> **The hinge rule.** *Governance disqualifies a tool only when a viable alternative exists. When the captured tool is genuinely unavoidable, you take it and keep it thin.* This is the single principle that separates a hard disqualification (Mojo, Electron, Unity, Terraform — a real escape exists, so the capture cost isn't worth paying) from a forced-mitigate dependency (CUDA, iOS/Swift — no equal escape today, so you adopt and minimize). Same governance axis; opposite verdict, decided entirely by whether the escape exists. See [Anti-patterns & disqualified](#-anti-patterns--disqualified).

---

## 🗂️ Layer index

| # | Layer | Spine choice |
|---|---|---|
| [0](#-layer-0--silicon--hardware) | Silicon & Hardware | SystemVerilog 🔴 / Chisel 🟢 (open-silicon) |
| [1](#-layer-1--firmware-bare-metal-embedded) | Firmware / Bare-metal / Embedded | **Rust** 🟢 · Ada/SPARK 🟢 (safety-critical) |
| [2](#-layer-2--os--kernel) | OS / Kernel | C + **Rust** 🟢 · seL4 (formal) |
| [3](#-layer-3--systems--performance-critical) | Systems & Performance | **Rust** 🟢 |
| [4](#-layer-4--backend-servers-concurrency) | Backend / Servers / Concurrency | **Rust** (Axum) 🟢 · **BEAM** 🟢 |
| [5](#-layer-5--data--databases) | Data & Databases | **SQL** 🟢 · Rust storage engines |
| [6](#-layer-6--data-engineering-ml-scientific) | Data Eng / ML / Scientific | **Python** 🟢 (train) · **Rust** 🟢 (infer) |
| [7](#-layer-7--mobile-gatekept--mitigate) | Mobile (gatekept) | **Rust + UniFFI** 🟢 core; thin Kotlin/Swift shells |
| [8](#-layer-8--desktop) | Desktop | **Tauri** 🟢 |
| [9](#-layer-9--web-frontend-captive-platform-the-browser) | Web Frontend | **TypeScript** 🟢 → **Rust→WASM** 🟢 |
| [10](#-layer-10--cli-scripting-shell) | CLI / Scripting / Shell | **Rust** 🟢 · Python 🟢 · Bash 🟢 |
| [11](#-layer-11--infra-as-code--config) | Infra-as-Code & Config | **Nix** 🟢 · **OpenTofu** 🟢 |
| [11b](#-layer-11b--config-as-policy-format-neutral-security-doctrine) | Config-as-Policy | format-neutral policy doctrine |
| [12](#-layer-12--blockchain--zk) | Blockchain & ZK | **Rust** 🟢 (zkVM) · Noir/Cairo 🟢 |
| [13](#-layer-13--formal-methods--correctness) | Formal Methods | **Lean 4** 🟢 · **TLA+** 🟢 |
| [14](#-layer-14--security--re--offensive) | Security / RE / Offensive | **Rust** 🟢 / Python 🟢 |
| [15](#-layer-15--game-dev-if-ever-relevant) | Game Dev | Godot 🟢 / Bevy 🟢 |
| [16](#-layer-16--cross-cutting-infrastructure) | Cross-cutting infrastructure | format-over-engine: OTel, OpenAPI, CBOR, Valkey/OpenSearch forks |

**Cross-cutting reference sections:** [How to choose](#-how-to-choose--the-decision-heuristic) · [The interop spine](#-the-interop-spine) · [Anti-patterns & disqualified](#-anti-patterns--disqualified) · [Governance scorecard](#-governance-scorecard)

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

## 📊 Governance scorecard

A rough read of where the doctrine's picks sit on the capture axis (approximate — count the Primaries, not the watch items):

- **🟢 the strong majority of Primaries** — Rust everywhere it appears (systems, embedded, crypto, backend, CLI, ML-infer, ZK, WASM, desktop/mobile cores), SQL, BEAM/Elixir/Gleam, Nix, OpenTofu, Lean 4, TLA+, Tauri, Godot/Bevy, and the Layer 16 format/interface Primaries (JSON, protobuf, CBOR, OpenTelemetry, Prometheus, Valkey, OpenSearch, Tantivy, OpenAPI/AsyncAPI/JSON Schema). The doctrine is overwhelmingly foundation/community-governed by design.
- **🟡 single-vendor-steered, used deliberately and kept thin** — Go (Google-steered; ops/infra only), Kotlin (Google-steered platform), Flutter/Dart & KMP, C#/.NET (MS-steered), Meilisearch, Grafana, RabbitMQ (now Broadcom-stewarded), Mimir.
- **🔴 capture-risk hot spots — the forced or rejected** — **GPU/CUDA** (NVIDIA-captive, the deepest moat; forced-mitigate behind wgpu), **iOS/Swift platform** (Apple-gatekept; the highest capture risk in the stack; forced-mitigate behind UniFFI), **macOS native SwiftUI** (captive), **RTL/SystemVerilog & UVM** (commercial-EDA-captive; mitigated by the open-silicon path). Rejected outright with a viable escape: **Mojo, Electron, Unity, Terraform** (see [Anti-patterns & disqualified](#-anti-patterns--disqualified)).

The hot spots cluster predictably at the **edges that touch hardware and gatekept platforms** — silicon, GPUs, mobile/desktop OS frontends. The interior (logic, data, correctness, infra) is almost entirely 🟢. That shape *is* the doctrine: keep the captured surfaces thin and at the edge; keep the spine sovereign.

---

## 🔩 Layer 0 — Silicon & Hardware

*TL;DR — SystemVerilog is the forced industry interface; the open-silicon FOSS chain buys auditability and license-freedom, not leading-edge parity; CUDA is the deepest moat — escape it slowly via wgpu.*

| Domain | Primary | Fallback / Legacy | Watch | Notes |
|---|---|---|---|---|
| RTL / chip design | SystemVerilog 🔴 (IEEE std) | VHDL 🔴 | **Chisel** 🟢 (Scala-based, runs RISC-V/SiFive), Amaranth 🟢 (Python), Veryl 🟢 ⏳ | FOSS toolchain exists: Yosys + Verilator + nextpnr. The open-silicon path. |
| FPGA | SystemVerilog / VHDL | — | Chisel, SpinalHDL 🟢 | Same toolchain story. |
| HW verification | SystemVerilog + UVM 🔴 | — | SymbiYosys 🟢 (formal) | Formal verification of RTL is FOSS-viable now. |
| GPU kernels | CUDA C++ 🔴 (NVIDIA-captive) | OpenCL | **wgpu/WGSL** 🟢 (Rust), ROCm/HIP 🟡, Mojo 🔴 | CUDA is the deepest moat in computing. wgpu is the sovereign escape, slower today. |

> **The durable unit is the emitted (System)Verilog netlist, not the source HDL.** Adopt governed HDLs incrementally behind that boundary.

<details><summary><strong>Primary vs Fallback, the open-silicon path, and the CUDA moat — rationale</strong></summary>

**Primary vs Fallback — when to reach for which.** SystemVerilog stays Primary for RTL because the entire commercial tooling chain (simulation, synthesis to silicon, sign-off, IP libraries) assumes it; if you are taping out on a real foundry process or buying vendor IP, you are on SystemVerilog whether you like it or not. Reach for VHDL only on legacy/defence/EU-aerospace bases that already mandate it — it is not a "better" choice, it is an inherited one. Reach for the 🟢 *watch* HDLs (Chisel, Amaranth, SpinalHDL, Veryl) when the target is an FPGA or an open-PDK ASIC flow and you control the whole chain end-to-end — they generate Verilog as their interface, so the **durable unit is the emitted (System)Verilog netlist, not the source HDL**, and you can adopt them incrementally behind that boundary.

**The open-silicon path, concretely.** The FOSS chain is real but uneven by target. Yosys (synthesis) + Verilator (cycle-accurate sim) + nextpnr (place-and-route) + SymbiYosys (formal property checking) covers FPGAs first-class for the bitstream-reverse-engineered families: Lattice iCE40 (Project IceStorm) and ECP5 (Project Trellis), plus Gowin. Xilinx/AMD and Intel/Altera high-end parts still need vendor tools for the final bitstream — the open flow thins out exactly where the parts get big. For *ASIC*, the open path is OpenROAD/OpenLane against open PDKs (SkyWater 130nm, IHP 130nm); these are genuinely fabbable on shuttle runs but are mature-node, low-density processes — fine for sovereign trust-anchor silicon, not for competing with a leading-edge SoC. **Maturity caveat: the open-silicon path buys you auditability and freedom from per-seat EDA licensing, not parity with a commercial node.** Treat it as the trust-rooted-small-die option, not a drop-in for the whole layer.

**Migration path off SystemVerilog.** Pick a 🟢 HDL that emits Verilog (Chisel/SpinalHDL → FIRRTL/Verilog; Amaranth → Verilog), keep SystemVerilog/UVM for verification of the *generated* netlist, and verify equivalence at the Verilog boundary. This lets the design source move to a governed language while the sign-off flow stays on the industry-standard interface — the same "interface is the durable unit" discipline as the rest of the stack.

**GPU / CUDA capture — the deepest moat in the stack.** CUDA-the-language is the smallest part of the lock-in. The moat is the *ecosystem above it*: cuDNN, cuBLAS, CUTLASS, the Tensor-Core programming model, NCCL collectives, NVLink fabric, and the fact that every ML framework's fast path is tuned for it. Escaping "CUDA C++" the language is easy; escaping that stack is the hard part. Honest ranking of the escapes: **ROCm/HIP** 🟡 is the closest like-for-like — `hipify` mechanically ports CUDA source and AMD targets near-parity on supported datacentre GPUs, but it is single-vendor-steered and historically narrow on consumer-hardware and toolchain stability. **SYCL/oneAPI** is the open-standard, vendor-portable bet but carries its own runtime weight. **wgpu/WGSL** 🟢 is the sovereign escape the doctrine endorses: portable across Vulkan/Metal/D3D/GL, foundation-adjacent (Rust/web lineage), and it fits the Rust spine — but it is a graphics-and-general-compute API, not a tensor-core ML runtime, so it is **slower today** and lacks the cuDNN-class kernel library. **Mojo 🔴 stays a 🔴 watch only** — exciting GPU-portability story, but closed compiler (see Layer 6). The doctrinal stance holds: take the CUDA dependency where the perf genuinely forces it, keep it thin and quarantined behind a kernel-abstraction boundary, and treat wgpu as the migration target you grow into as it closes the gap — price the capture, don't marry it.

</details>

---

## 🔧 Layer 1 — Firmware, Bare-Metal, Embedded

*TL;DR — Rust is the default for new firmware; drop to C only where vendor toolchains force it; SPARK for machine-checked proofs, Ferrocene Rust for a certifiable general-purpose toolchain.*

| Domain | Primary | Fallback / Legacy | Watch | Notes |
|---|---|---|---|---|
| MCU / bare-metal | **Rust** 🟢 (embassy async) | C 🟢 (widest vendor support) | Zig 🟢 ⏳ | Rust is the future; C still wins raw breadth of vendor toolchains. |
| RTOS | C 🟢 (Zephyr, FreeRTOS) | — | Rust-on-Zephyr | — |
| Safety-critical (avionics/auto/med) | **Ada/SPARK** 🟢 (provable, DO-178C) | MISRA C | **Rust via Ferrocene** 🟢 (ISO 26262 / IEC 61508 qualified) | Qualified Rust toolchain is the genuinely new thing. SPARK if you need machine-checked proofs. |
| DSP | C/C++ + intrinsics | — | Rust | — |

> **Need a mathematical proof of a property → SPARK; need a certifiable general-purpose systems language → Ferrocene Rust; have neither requirement → plain Rust 🟢.**

<details><summary><strong>When to drop to C, the RTOS governance note, and the safety-critical decision rule — rationale</strong></summary>

**Primary vs Fallback — when to drop to C.** Rust (with `embassy` for async-on-bare-metal, RTIC for hard-realtime interrupt scheduling, `probe-rs` for flashing/debug) is Primary and should be the default for new firmware: memory safety on a target with no MMU and no allocator is worth the most exactly where a bug is unrecoverable. Reach for C as Fallback when the part's vendor HAL/SDK only ships C, when the silicon vendor's certified toolchain is C-only, or when the chip is so obscure that no Rust target/PAC exists — **C still wins raw breadth of vendor toolchains**, and fighting that on an exotic MCU burns the budget the sovereignty was supposed to save. The realistic migration pattern is incremental: keep the vendor C HAL at the bottom, wrap it, and write new application logic in Rust above a thin `extern "C"` boundary — the same thin-shell discipline used for captive platforms, applied to captive silicon SDKs. Zig stays a 🟢 ⏳ watch: excellent C interop and cross-compilation, genuinely useful as a C *replacement/build tool*, but pre-1.0 and not load-bearing for safety work yet.

**RTOS governance note.** The C Primary here is governance-clean, not a forced 🔴: Zephyr is Linux-Foundation-governed (Apache-2.0) and FreeRTOS is MIT (AWS-stewarded). "Rust-on-Zephyr" is the watch item — Zephyr has nascent Rust application support, but the kernel stays C; treat Rust there as application-layer-over-C-RTOS, not a kernel rewrite.

**Safety-critical — Ferrocene is the genuinely new thing.** SPARK (Ada subset) stays Primary when you need **machine-checked proofs** — absence of runtime errors, and contract/flow proofs the toolchain verifies, not just tests — which is the strongest guarantee available and why it earns Primary over MISRA C. Reach for **Rust via Ferrocene** when you want Rust's ergonomics and ecosystem *plus* a qualified toolchain: Ferrocene (Ferrous Systems) is a qualified downstream of upstream `rustc` whose qualification covers **ISO 26262 (automotive) and IEC 61508 (industrial)**, with the toolchain sources public/openly available — a real answer to "you can't use Rust in certified contexts." Honest maturity caveats: (1) qualification attaches to the *toolchain*, not to your code — you still owe the assurance case, requirements traceability, and verification evidence the standard demands; (2) coverage of the *medical* (IEC 62304) and *avionics* (DO-178C) regimes is expanding but is more recent / partial than the automotive+industrial core — confirm the current qualification scope for your exact standard and assurance level before committing, rather than assuming blanket coverage. The decision rule: **need a mathematical proof of a property → SPARK; need a certifiable general-purpose systems language → Ferrocene Rust; have neither requirement → plain Rust 🟢.** MISRA C remains the Fallback only where an existing certified codebase or a customer mandate forces it.

</details>

---

## 🧱 Layer 2 — OS / Kernel

*TL;DR — Rust-for-Linux is merged; write new leaf drivers in Rust, leave core subsystems C. Don't rewrite seL4 — inherit its proof corpus and build a Rust capability userland above it.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Kernel | C 🟢 (Linux) + **Rust** 🟢 (mainline) | — | Redox OS 🟢 (Rust microkernel) | Rust-for-Linux is merged; the safe-systems debate is over. |
| Microkernel / formal | C + **Isabelle/HOL** proofs (seL4) | — | Rust + capability IPC | The proof corpus is the asset. Don't rewrite seL4; build on its guarantees. (PlausiDenOS target.) |

> **The proof corpus — not the C — is what you are buying. Do not rewrite seL4; inherit the proof and expose capabilities to a Rust userland.**

<details><summary><strong>Rust-for-Linux status and the seL4 proof-corpus argument — rationale</strong></summary>

**Rust-for-Linux — status, honestly.** Rust support is *merged into mainline* and the in-principle debate is settled: the question is no longer "will the kernel allow Rust" but "how far do the abstractions reach." Reality check on maturity: the Rust *infrastructure* and an expanding set of subsystem bindings are in-tree, and real drivers are landing (network PHY, a null-block driver, Android Binder, and GPU-driver work including Apple AGX and the NVIDIA "Nova" effort), but **the bulk of the kernel is and will remain C**, and many subsystem maintainers have not yet exposed Rust abstractions. So the Primary is deliberately written as **C + Rust**, not "Rust": you write *new leaf drivers/modules* in Rust against whatever safe abstractions exist, and you do not attempt to displace core C subsystems. The migration path is additive and slow by design — new code in Rust at the edges, C everywhere the abstractions don't yet reach. Redox OS stays a 🟢 watch (clean-room Rust microkernel, genuinely interesting, **pre-1.0 and not load-bearing**) — useful as proof-of-concept and idea source, not a deployment target.

**Microkernel / formal — the proof corpus is the asset, not the code.** seL4's value is its **machine-checked functional-correctness proof in Isabelle/HOL**, binding the C implementation to an abstract specification, with further proofs of integrity/confidentiality and a binary-level correctness result down to the compiled image. That proof corpus — not the small C codebase — is what you are actually buying, and it is the single most valuable asset in this layer. The doctrine is therefore emphatic: **do not rewrite seL4.** A "Rust microkernel" would throw away the proofs to gain memory-safety guarantees that the proofs already subsume (and exceed). Reach for the "Rust + capability IPC" watch item *above* seL4, not instead of it: keep verified C at the trusted core and write the capability-mediated userland services in Rust, so the formal guarantee anchors the bottom and the Rust spine carries everything above the IPC boundary. seL4 is Foundation-governed (seL4 Foundation), so the Primary here is governance-clean despite being C. **This is the PlausiDenOS target** — build *on* the guarantees, inherit the proof, expose capabilities to a Rust userland.

</details>

---

## ⚙️ Layer 3 — Systems & Performance-Critical

*TL;DR — Rust is the unqualified default; C++ only where the ecosystem genuinely forces it; "proven" beats "tested" for the crypto core.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Systems / perf services | **Rust** 🟢 | C++ 🟢 (where ecosystem forces it: games, HPC, legacy) | Zig ⏳ | Default. |
| Crypto / security tooling | **Rust** 🟢 | C (libsodium, audited) | F*/hax 🟢 (Rust→F* extraction) | "Proven" > "tested" for an adversarial product. |

> **For an adversarial product the asymmetry is decisive: an attacker needs one bug, a test finds the bugs it thought to look for, a proof closes the whole class.**

<details><summary><strong>When C++ wins, and the proven-vs-tested crypto frontier — rationale</strong></summary>

**Primary vs Fallback — when C++ wins.** Rust is the unqualified **default** for systems and performance-critical services: it gives you C++-class performance with memory and data-race safety, a coherent build/dependency story (Cargo), and it is the spine the rest of this doctrine assumes. Reach for **C++** only **where the ecosystem genuinely forces it** — not on taste. The honest forcing functions: a mature engine or middleware whose API is C++ (games, see Layer 15); the HPC/scientific numeric libraries and vendor compilers that target C++ first; and large legacy codebases where a rewrite is not justifiable. In those cases the C++ is a constraint to be wrapped and contained, not a defeat — expose a clean ABI/FFI boundary and keep new logic in Rust above it. Zig stays a ⏳ watch: promising as a small, simple systems language and an excellent cross-compiler/C-build tool, but **pre-1.0 and not load-bearing** for production systems work yet. (The ecosystem mapping already uses Zig in exactly this bounded way — Zig kernels behind Rust FUSE/eBPF, not as the spine.)

**Crypto / security tooling — "proven" beats "tested" for an adversarial product.** Rust is Primary; the Fallback is **C only where the implementation is already audited** (libsodium being the canonical case — decades of scrutiny is itself a form of assurance you don't get from new code). The watch item is the real frontier and the reason this row exists: **F\*/hax** lets you write a verifiable subset of Rust and *extract* it to F\* (with further backends toward proof/protocol checkers), so a primitive can be **proven** — functional correctness, secret-independence/constant-time, absence of specific bug classes — rather than merely tested, then compiled back to real Rust that ships (the libcrux line of work is the worked example). For an adversarial product the asymmetry is decisive: an attacker needs one bug, a test suite finds the bugs it thought to look for, and a proof closes the whole class. **Maturity caveat:** the verifiable Rust subset is a *subset* — you constrain how you write the hot, sensitive code, and the verification toolchain is specialist and lower-throughput than ordinary Rust development. So the realistic posture is layered, matching the doctrine elsewhere: plain Rust 🟢 for the bulk of security tooling; reach for F\*/hax on the small, high-value, irreducible crypto core where a proven-vs-tested gap actually changes the threat model; fall back to audited C only when a vetted incumbent like libsodium already exists and re-implementing it would *add* risk rather than remove it.

</details>

---

## 🛰️ Layer 4 — Backend, Servers, Concurrency

*TL;DR — CPU/latency-bound → Rust/Axum; connection-bound → BEAM. Go only for the cloud-native control plane you must plug into. Wire protocols are the durable unit.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Concurrent / fault-tolerant / realtime | **Elixir/Erlang (BEAM)** 🟢 | — | **Gleam** 🟢 (typed BEAM, 1.0) | Nothing else has the supervision/distribution model. Gleam = OTP + static types; the typed-BEAM future. |
| General API / web backend | **Rust** (Axum) 🟢 | Elixir/Phoenix, Gleam, Go 🟡 | — | CPU/latency-bound → Rust; connection-bound → BEAM. The rule has teeth below. |
| Ops/infra services | **Go** 🟡 | Rust | — | The k8s/cloud-native ecosystem is Go; fight it only with reason. When Go is right — and its cost — below. |
| Wire protocols | protobuf/gRPC, Cap'n Proto 🟢 | — | — | The durable interface layer. Language-agnostic by design. |

> **Go for the cloud-native control plane you must plug into; Rust for everything that is your product.** Keep Go services thin and at the edges, the same way captive-platform UI shells are kept thin over a Rust core.

<details><summary><strong>The decision rule with teeth, when Go is the right call, and why wire protocols stay durable — rationale</strong></summary>

### The decision rule, with teeth

The one-liner — *CPU/latency-bound → Rust; connection-bound → BEAM* — decides most backends. Sharpened:

- **CPU/latency-bound** = the cost is in the *work per request*: parsing, crypto, serialization, compression, query planning, anything where p99 is dominated by computation and you want no GC pause and no runtime tax. → **Rust/Axum.** A predictable p99 with no stop-the-world is the whole point; you pay in compile times and borrow-checker friction, not at runtime.
- **Connection-bound** = the cost is in *holding many concurrent things alive*: large fan-outs of open sockets, long-lived sessions, presence, pub/sub fan-out, soft-realtime push, work that must *degrade gracefully* rather than fail hard. → **BEAM.** Per-process heaps, preemptive scheduling, and "let it crash" supervision are not replicable in Rust without rebuilding an actor runtime you'd then have to operate. The BEAM *is* that runtime, battle-tested across decades of telecom uptime.
- **The crossover is where people get it wrong, both directions:**
  - **Don't run heavy CPU work on the BEAM.** Number-crunching, large-payload crypto, or tight numeric loops in pure Elixir starve the schedulers and blow soft-realtime guarantees. The escape — NIFs — is a footgun: a NIF that runs too long or panics takes the *whole node* down, defeating the fault-isolation you chose BEAM for. Use **dirty schedulers** for long NIFs, and write the NIF in **Rust via `rustler`** (panic-safe boundary) — which is the same logic-in-Rust move, just hosted under OTP. This is the [`PlausiDen-Shield`](https://github.com/thepictishbeast/PlausiDen-Shield) shape: Rust does the work, Elixir owns the control plane.
  - **Don't reach for Rust when the problem is orchestration, not throughput.** A massive-fan-out presence/messaging service with modest per-message work is BEAM's home turf; rebuilding its supervision and distribution in async Rust is high-effort and you'll under-build the failure model.
- **When neither dominates**, default to **Rust/Axum** (the Primary) for one codebase, one toolchain, and reuse of the Rust core that already exists across the ecosystem. Reach for **Phoenix/Gleam** only when the connection/supervision axis is the actual product.

### When **Go** is the right call — and what it costs

Go is the Fallback, flagged 🟡 (Apache-licensed, **Google-steered** — governance, not license, is the risk). Take it deliberately, in narrow cases:

- **Ecosystem gravity is the load-bearing reason.** The k8s/cloud-native control-plane world — operators, controllers, admission webhooks, CRD tooling, the entire CNCF substrate — is written in Go, and its client libraries, codegen, and conventions assume Go. Writing a Kubernetes operator in Rust means re-deriving generated clients and fighting the grain for sovereignty points that don't pay off here. **Match the ecosystem you must integrate into.**
- **Secondary reasons:** trivial single-binary cross-compilation, a huge pool of contributors, and fast *enough* performance with a forgiving learning curve — good when the team, not the machine, is the constraint.

**The cost, stated honestly (so it's a deliberate trade, not a default):**

- **GC tail latency.** Go's collector is low-pause but not no-pause; for hard p99/soft-realtime SLAs it loses to Rust. If latency *is* the spec, this disqualifies it.
- **A weaker type system.** No sum types / exhaustive matching, `nil`-everywhere, `interface{}`/`any` escape hatches, generics that arrived late and stay shallow. Whole classes of bugs Rust makes unrepresentable are merely discouraged in Go.
- **Verbose, lossy error handling.** `if err != nil` everywhere, and errors easily swallowed or wrapped without context — the opposite of Rust's `Result`/`?` discipline.
- **Governance.** 🟡 means a single steward sets the language's direction. Acceptable for a thin, swappable ops service; price it before letting Go creep into product logic.

Rule of thumb: **Go for the cloud-native control plane you must plug into; Rust for everything that is your product.** Keep Go services thin and at the edges, the same way captive-platform UI shells are kept thin over a Rust core.

**Wire protocols stay the durable unit.** Whatever the language split — Rust here, BEAM there, a Go operator at the edge — the contract between them is **protobuf/gRPC or Cap'n Proto**, language-agnostic by design. Pin the schema; let implementations swap underneath it. This is the through-line: *interfaces are the durable unit, not languages.*

</details>

---

## 🗄️ Layer 5 — Data & Databases

*TL;DR — SQL is the safest long-horizon bet in the entire stack: declarative, ISO-standardized, no capture vector. Build storage engines in Rust; query them in portable SQL.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Querying data | **SQL** 🟢 | — | — | The single most future-proof language in existence. ~50yr, outlived every paradigm. Bet hard on it — argument below. |
| Stored procedures | PL/pgSQL 🟢 (Postgres) | — | — | Your CRM/Salesman backend. |
| Building a storage engine | **Rust** 🟢 | C++ 🟢 (Postgres/SQLite legacy) | — | New DBs are overwhelmingly Rust. |
| Embedded DB | SQLite (C) 🟢 | — | — | Most-deployed DB on Earth. |
| Advanced query / reasoning | Datalog 🟢 | — | — | For graph/recursive queries; relevant to neurosymbolic work. |

> **Keep logic in portable SQL; quarantine vendor extensions.** The same `SELECT` survives a complete rewrite of the storage engine, the planner, and the hardware beneath it.

<details><summary><strong>Why SQL is the safest long-horizon bet, and why engines are Rust — rationale</strong></summary>

### Why SQL is the safest long-horizon bet in the stack

SQL is the closest thing computing has to a permanent interface, and the reasons are structural, not nostalgic:

- **It's declarative — you state *what*, never *how*.** The query says which rows you want; the engine's optimizer decides access paths, join order, and execution. That separation is exactly the *interface-is-the-durable-unit* doctrine expressed at the data layer: the same `SELECT` survives a complete rewrite of the storage engine, the planner, and the hardware beneath it. Decades of optimizer and storage advances landed *underneath* unchanged queries.
- **The standard outlives every host.** SQL is ISO-standardized and multiply-implemented; no single vendor owns it. Queries port (with friction, never a rewrite) across Postgres, SQLite, and successors not yet written. Contrast every "kill SQL" wave — hierarchical, network, object databases, the NoSQL era, the "NewSQL" rebrand: each receded, and the survivors **added a SQL layer back on top.** SQL absorbed its challengers rather than being replaced by them.
- **It is the rare language with no governance-capture vector.** No foundation to defund, no relicense to fear (contrast the OpenTofu/Terraform event in Layer 11), no runtime to be captured. That is why it earns an unqualified 🟢 and the strongest "bet hard on it" in the doc.

Practical doctrine: **keep logic in portable SQL; quarantine vendor extensions.** `PL/pgSQL` is the Primary for stored procedures (the CRM/Salesman backend) and earns its place — but PL/pgSQL, Postgres-specific operators, and proprietary dialects are a capture surface. Push them behind a boundary, the same way captive platforms are kept thin, so the durable ANSI core stays portable.

**Storage engines are Rust; queries are SQL — the layers are independent.** When you build the engine rather than query one, the language is **Rust** (memory safety where corruption is unforgiving, fearless concurrency for the lock/MVCC machinery); the new-DB cohort is overwhelmingly Rust for exactly these reasons. **C++** remains the Fallback only where the ecosystem forces it (the Postgres/SQLite legacy corpus). **SQLite (C)** stays the embedded Primary on reach and reliability — the most-deployed database on Earth, with one of the most thorough test suites in existence; don't rewrite it, build on it. For graph/recursive and rule-based reasoning beyond relational's comfort zone, **Datalog** is the specialist tool — directly relevant to the neurosymbolic work.

</details>

---

## 🧠 Layer 6 — Data Engineering, ML, Scientific

*TL;DR — train in Python, serialize across a neutral artifact (safetensors/ONNX), infer in Rust. Mojo is disqualified on a closed compiler, not on merit.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| ML/AI research & training | **Python** 🟢 (PyTorch/JAX) | — | — | Lingua franca. Unavoidable. Don't fight it; wrap it. |
| Production ML inference | **Rust** 🟢 (candle, burn) | Python | Mojo 🔴 (disqualified) | Fits LFI's Rust HDC core directly. |
| Scientific / numerical | **Julia** 🟢 | Python | — | Modern FOSS challenger; fast, MIT. |
| Dense numerics / HPC | **Fortran** 🟢 | C++ | Julia | Not a joke — still optimal for dense linear algebra/HPC. The incumbent that refuses to die because it's correct. |
| Dataframes / analytics | SQL + **Polars** 🟢 (Rust) | pandas | — | — |
| Mojo status | — | — | — | 🔴 **compiler is closed** (Modular Community License), single-vendor. Technically exciting, fails the FOSS/PSA filter. Revisit only if the compiler is OSI-licensed + foundation-governed. |

> **The model file is the interface** — the durable unit between the Python lab and the Rust product, exactly as protobuf is between services and SQL is over storage.

<details><summary><strong>The train-Python/infer-Rust pattern, and why Mojo is disqualified — rationale</strong></summary>

### The Python-wrap pattern, made concrete: train in Python, infer in Rust

This is the operational meaning of *"don't fight it; wrap it"* — and the seam is deliberate, not incidental:

- **Train in Python.** Research, experimentation, and training stay in **PyTorch/JAX**. The ecosystem (datasets, optimizers, autograd, the entire paper-to-code pipeline) is unavoidable and not worth fighting. Python is the *lab*, not the *product*.
- **Serialize across a language-neutral artifact.** Export trained weights to a portable, framework-agnostic format — **safetensors** (preferred: no arbitrary-code-execution risk on load, unlike pickle) or ONNX where graph portability matters. **The model file is the interface** — the durable unit between the Python lab and the Rust product, exactly as protobuf is between services and SQL is over storage.
- **Infer in Rust.** Production inference loads that artifact and runs in **Rust** (`candle`, `burn`). Single static binary, no Python interpreter in the deployment, no GIL, no dependency-hell virtualenv on the serving path, predictable latency, and a far smaller attack surface — which is why this is a *sovereignty* move, not merely a performance one. It drops straight onto the existing Rust core (the LFI HDC/VSA inference is already Rust): the [`PlausiDen-AI`](https://github.com/thepictishbeast/PlausiDen-AI)/[`PlausiDen-LFI`](https://github.com/thepictishbeast/PlausiDen-LFI) shape.
- **The boundary keeps Python out of the trust path.** Python is wrapped, contained, and never ships in the sovereign inference deployment. **Python is the Fallback for inference only** when an op has no Rust kernel yet — accepted as a stopgap, not a destination, and tracked as debt to be moved across the boundary.

For dense linear algebra / HPC the incumbent **Fortran** is still genuinely optimal (correct, not a joke), C++ the Fallback, Julia the watch; for general scientific/numerical, **Julia** is the FOSS challenger (fast, MIT). Dataframes/analytics: **SQL + Polars** (Rust, same-binary, same sovereignty story), pandas as Fallback.

### Mojo: disqualified, and why the bar is exactly this

Mojo is at 🔴 and **disqualified** — not on technical merit (it is genuinely exciting) but on governance, which is a first-class selection axis here:

- **The compiler is closed**, shipped under the **Modular Community License** — not an OSI-approved license — and **single-vendor-controlled**. That is the precise capture vector the FOSS/PSA (Polyglot Sovereign Architecture) filter exists to reject. A closed, single-vendor compiler in the inference path is an unbounded future-capture risk no benchmark buys back.
- This is consistent with the rest of the doctrine: CUDA is tolerated as the deepest moat in computing only because there is no escape *yet* (wgpu is the sovereign path); Mojo offers no comparable necessity — **Rust inference already covers the domain.** There is no forcing function, so the closed compiler is disqualifying rather than merely flagged.
- **Revisit condition, unchanged and exact:** the compiler becomes **OSI-licensed and foundation-governed.** Until then it stays on the watch list and out of the stack — technically exciting, governance-failing.

</details>

---

## 📱 Layer 7 — Mobile (gatekept — mitigate)

*TL;DR — the most-captured layer. Write the product once in a Rust + UniFFI core; Kotlin/Swift are thin view shells that never hold a business rule. iOS 🔴 is gated; Android 🟡 is only steered.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Shared logic core | **Rust + UniFFI** 🟢 | — | — | **The sovereign move.** Real logic in Rust, exposed to both platforms. Native languages become thin shells. |
| Android UI | Kotlin 🟡 (JetBrains/Google) | Java | — | Apache-licensed but Google-steered platform. |
| iOS UI | Swift 🔴 (Apple-gatekept) | Obj-C | — | Swift-the-language is open; the platform/tooling is captive. Highest capture risk in the stack. |
| Cross-platform (one codebase) | **Flutter/Dart** 🟡 or KMP 🟡 | React Native/TS | Dioxus 🟢 (Rust) | Flutter is BSD-FOSS but Google-governed — license isn't the risk, governance capture is. |

> **Logic in Rust, captive languages as thin swappable shells, the FFI interface as the durable unit.** The captive language never holds a business rule.

<details><summary><strong>The UniFFI thin-shell pattern, and why Swift 🔴 ≠ Kotlin 🟡 — rationale</strong></summary>

**The UniFFI thin-shell pattern — why this is the central sovereign move.** Mobile is the most-captured layer in the stack: two duopoly vendors gatekeep the toolchains, the stores, and the signing keys. The doctrine's answer is to refuse to *write the product twice* in two captive languages. Instead, the entire product — domain logic, crypto, state machines, protocol handling, persistence — lives in one Rust crate (`no_std`-friendly where possible, per [`PlausiDen-Shard`](https://github.com/thepictishbeast/PlausiDen-Shard)). [UniFFI](https://mozilla.github.io/uniffi-rs/) (Mozilla; battle-tested in Firefox/application-services) generates idiomatic foreign bindings — Kotlin for Android, Swift for iOS — from a single interface definition (proc-macro attributes, or a UDL file). The platform code that remains is *only* what the platform genuinely owns: view tree, lifecycle, permissions prompts, push registration, hardware access. The captive language never holds a business rule.

This collapses the capture surface from "the whole app" to "the view layer," and it directly satisfies the through-line: **logic in Rust, captive languages as thin swappable shells, the FFI interface as the durable unit.** Concretely it buys:
- **One source of truth, two stores.** Logic bugs are fixed once; the Kotlin/Swift shells stay too thin to diverge. Bus-factor (SUPERSOCIETY §12) drops because there is one codebase to learn, in the spine language, not two platform dialects.
- **Reuse beyond mobile.** The same crate backs the Tauri desktop app (Layer 8) and compiles to WASM for the browser (Layer 9). UniFFI is one of several consumers of the core; the core is written once.
- **Testability off-device.** The logic is exercised in Rust's test harness on CI without an emulator, simulator, or Apple/Google build machine in the loop.
- **A migration hedge.** If Swift or Kotlin is ever swapped (a new UI framework, a KMP shell, a Dioxus-native target), only the regenerated binding layer changes — the interface contract holds. This is the same "interfaces are the durable unit" bet as protobuf and WASM, applied to the FFI boundary.

Cost paid deliberately: UniFFI's type system is a lowest-common-denominator across languages (own your types at the boundary; avoid leaking complex Rust generics, lifetimes, or async directly across the seam — model async as explicit callbacks/futures the binding understands). Object-heavy chatter across the FFI has marshalling cost, so the boundary is designed coarse (few, meaningful calls) rather than fine (per-field getters). These are interface-design disciplines, not defects — they are the price of the swap-ability.

**The precise capture risk — Swift vs Kotlin are not equal 🔴/🟡.**
- **iOS / Swift is 🔴 because the *platform* is captive, not the language.** Swift-the-language is open-source (Apache-2, Swift.org, an active evolution process). What is gatekept is everything around it that you cannot route around: building for device requires Xcode on Apple hardware; shipping requires a paid developer account, Apple's signing/notarization, and App Store review; the only first-party UI toolkit (SwiftUI) and the system WebView (WKWebView) are Apple-controlled and track OS releases on Apple's calendar. There is no sanctioned escape — no alternative store, no sideloading parity, no third-party toolchain that ships to the platform without Apple in the path. This is the **highest capture risk in the entire stack**: the gatekeeper controls distribution itself, not merely the tool. The mitigation is structural — keep the Swift shell as thin as UniFFI allows so that the captive surface is minimized and the product's value sits in code Apple does not govern.
- **Android / Kotlin is 🟡 because it is steered, not gated.** Kotlin is Apache-2 and JetBrains-developed; Android elevated it to first-class and Google now steers the platform direction (Jetpack, Compose, the AGP/Gradle toolchain, Play policies). The crucial difference from iOS: Android permits sideloading, alternative stores (F-Droid), and self-signed builds — distribution is *not* monopolized. That is why the doctrine's required mobile distribution path (AppImage/.deb/.rpm + APK, auto-updating, FOSS-first via GitHub) is *possible* on Android and *not* on iOS. Capture here is governance-direction risk (the platform can move under you), not distribution-gate risk. Keep the shell thin for the same reason, but the existential leverage Google holds is weaker than Apple's.

**On cross-platform-one-codebase.** Flutter/Dart and KMP stay 🟡 (Google-governed; the risk is governance capture, not the BSD/Apache license). React Native/TS is the Fallback. The Watch entry is **Dioxus 🟢 (Rust)** — the sovereign endgame where even the view layer is Rust over a Rust core, collapsing the shell to near-zero and removing the captive language from the product entirely; not yet load-bearing for production mobile, but it is the trajectory the UniFFI pattern is already pointed at. PlausiDen's instantiation today is [`PlausiDen-Android`](https://github.com/thepictishbeast/PlausiDen-Android) (Rust core + thin Kotlin shell); the iOS shell, when built, follows the identical pattern over the same crate.

</details>

---

## 🖥️ Layer 8 — Desktop

*TL;DR — Tauri (Rust core + OS WebView) is the sovereign default: order-of-magnitude smaller than Electron and reuses the mobile/web Rust core. Avoid Electron's bundled-Chromium bloat.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Cross-platform (sovereign) | **Tauri** 🟢 (Rust core + web UI) | Qt 🟢/🟡 (C++) | Dioxus 🟢, Slint 🟢 | Tiny binaries, Rust core. Avoid Electron (bloat). |
| Linux native | C/C++ (GTK/Qt) 🟢 | Rust (gtk-rs) | Slint | — |
| Windows native | C# / .NET 🟡 (MIT, MS-steered) | C++ Win32 | — | .NET is FOSS now; still MS-directed. |
| macOS native | Swift/SwiftUI 🔴 | — | — | Captive. |

> **One core, three frontends.** The desktop "shell" is as thin as the mobile one; the durable unit is the Tauri command boundary, not the UI language.

<details><summary><strong>Tauri vs Electron — the governance and binary-size case</strong></summary>

**Tauri vs Electron — the governance and binary-size case.** The two differ at the architectural root, and that root is exactly the governance axis. **Electron bundles its own runtime** — a full Chromium plus a Node.js — into every app. That is why an Electron "hello world" is on the order of tens of megabytes before you write a feature, why memory footprint is heavy, and why every app re-ships and must independently patch a browser engine. The backend logic runs in Node/JavaScript. **Tauri ships no browser.** It renders the web UI in the *operating system's existing WebView* (WebView2/Edge on Windows, WKWebView on macOS, WebKitGTK on Linux) and puts the application's real logic in a **Rust core**. The result is order-of-magnitude smaller binaries and a far smaller resident footprint, with the backend in the spine language rather than in JavaScript.

The size delta is the headline, but the *governance* delta is the doctrine's reason:
- **Engine control.** Electron couples you to a Google-developed engine you re-ship and re-patch yourself. Tauri delegates the engine to the OS, which is itself a captive dependency — but a shared, OS-maintained one rather than one each app freezes a copy of. The honest caveat the doctrine keeps: the system WebView is *itself* a captured runtime (WKWebView is Apple-controlled, on Apple's release cadence; WebView2 is Microsoft's). Tauri does not escape the browser monopoly — it stops *embedding and forking* it, and shrinks the captive surface to a shared system component.
- **Spine alignment.** Tauri's Rust core means the desktop app reuses the *same* Rust logic crate as the UniFFI mobile core (Layer 7) and the WASM web build (Layer 9). One core, three frontends — the desktop "shell" is as thin as the mobile one, and the durable unit is again the interface (the Tauri command boundary), not the UI language.
- **Foundation governance** is why Tauri is 🟢 and the doctrine's Primary while Electron is avoided: Tauri is community/foundation-governed and FOSS, fitting the FOSS+foundation filter that Electron's vendor-runtime coupling and bloat fail. The Fallback is Qt 🟢/🟡 (C++; the 🟡 is the commercial-licensing/governance ambiguity of the Qt Company stewardship). Watch: **Dioxus 🟢** and **Slint 🟢** — both Rust-native UI paths that drop the WebView entirely, the same "even the view layer goes Rust" trajectory as the mobile Watch entry.

Native-per-OS rows are unchanged in stance: Linux native C/C++ (GTK/Qt) 🟢 with Rust gtk-rs Fallback and Slint Watch; Windows C#/.NET 🟡 (MIT but MS-directed); **macOS native Swift/SwiftUI stays 🔴 — captive**, the same Apple gate as Layer 7 iOS. Tauri is the cross-platform sovereign default precisely so that the one codebase serves all three OSes without writing three captive native frontends. Instantiated by [`PlausiDen-Desktop`](https://github.com/thepictishbeast/PlausiDen-Desktop), [`PlausiDen-Atrium`](https://github.com/thepictishbeast/PlausiDen-Atrium), and [`SacredVote-Desktop`](https://github.com/thepictishbeast/SacredVote-Desktop).

</details>

---

## 🌐 Layer 9 — Web Frontend (captive platform: the browser)

*TL;DR — the browser is a captured runtime; TypeScript is the floor of sanity, not the goal. Push logic into a Rust→WASM core behind the WASM Component Model; shrink TS/React to a view shell.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Production web UI | **TypeScript** 🟢 + Svelte/Solid | React | — | The browser is a captured runtime; TS is the floor of sanity. Treat as legacy-you-can't-escape-yet. |
| Sovereign / WASM future | **Rust→WASM** 🟢 (Leptos, Dioxus) | Gleam→JS | WASM Component Model 🟢 | Migration target off React. Push logic into WASM behind a stable component boundary. (Sacred.Vote's TS/React stays until this matures.) |

> **JS owns DOM and events; WASM owns truth.** This is the same thin-shell pattern as mobile, with JS as the captive language.

<details><summary><strong>The browser as captive runtime, and the React→Rust/WASM migration path — rationale</strong></summary>

**The browser is the captive runtime, and TypeScript is the floor — not the goal.** The browser is a runtime you do not control and cannot replace; its only universally-available native execution target is JavaScript. TypeScript is the *floor of sanity* over that — types catch the class of bugs untyped JS ships to production — and the doctrine's stance is explicit: **treat it as legacy-you-can't-escape-yet, not as a chosen home.** Primary is TypeScript 🟢 with Svelte/Solid (compile-to-minimal-runtime frameworks that emit less framework overhead than React's virtual-DOM model); React is the Fallback — present because the ecosystem gravity is real (the [Risk/steel-man](#-risk--steel-man) tax: talent, libraries, integration), not because it is preferred.

**The React→Rust/WASM migration path.** The endgame is to stop writing the product's *logic* in a captive-runtime language at all. The move is incremental and interface-mediated, not a rewrite:
1. **Extract logic to a Rust core, compile it to WebAssembly.** The same crate that backs the UniFFI mobile shell (Layer 7) and the Tauri desktop core (Layer 8) compiles to a WASM module. Validation, crypto, tally logic, state — the things that must be correct — leave JS/TS entirely and run as WASM.
2. **Shrink TS/React to a view shell** over that module, exactly as Kotlin/Swift are shrunk to view shells over the mobile core. JS owns DOM and events; WASM owns truth. This is the *same thin-shell pattern* as mobile, with JS as the captive language.
3. **Replace the shell when the Rust-native UI frameworks are ready.** **Leptos** and **Dioxus** (the 🟢 Watch/Primary for the sovereign future) render the view itself in Rust→WASM, removing the TS layer entirely — the browser-native analogue of the Dioxus/Slint endgame on mobile and desktop. Fallback for the transition is **Gleam→JS** (typed BEAM compiled to JavaScript) where staying in the JS-emit world is pragmatic.

PlausiDen already instantiates the destination, not just the plan: [`Linux-File-Manager`](https://github.com/thepictishbeast/Linux-File-Manager-and-UI-for-Remote-Access-) is Leptos WASM over an Axum backend, and [`PlausiDen-Browser-Ext`](https://github.com/thepictishbeast/PlausiDen-Browser-Ext) compiles the engine to WASM. The honest pacing the doctrine keeps: **Sacred.Vote's TS/React stays until this matures** — the migration is priced and deliberate, not a flag-day.

**The WASM Component Model is the durable boundary that makes the swap safe.** Pushing logic into WASM only buys sovereignty if the *boundary* between the WASM core and whatever UI calls it is stable and language-agnostic — otherwise you have merely traded one captive coupling for another. The [WASM Component Model](https://component-model.bytecodealliance.org/) (Bytecode Alliance; the foundation underneath WASI) is that boundary: components describe their imports/exports in **WIT** (WebAssembly Interface Types), a language-neutral interface contract, and compose across languages without a shared linker or ABI hacks. This is the **same architectural bet as protobuf, SQL, and the UniFFI FFI seam** — the interface is the durable unit; the implementation behind it swaps freely. Define the core's surface in WIT, and a TS shell today, a Leptos shell tomorrow, and a non-browser host after that all consume the *identical* component. WASM-the-runtime is still hosted by the captive browser, so this does not escape the runtime — but it makes the logic portable *off* the browser the day a better host exists, which is precisely the sovereignty the layer is buying.

</details>

---

## ⌨️ Layer 10 — CLI, Scripting, Shell

*TL;DR — Rust vs Go is a genuine tie; run the discriminator chain in order. Bash stays the portability floor; Python is glue you wrap, not load-bearing logic.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| CLI tools | **Rust** (clap) 🟢 | **Go** (cobra) 🟡 | — | Genuine tie. Most cloud CLIs are Go; Rust wins on perf/correctness. Tie-breaker below. |
| Glue / automation | **Python** 🟢 | — | — | Ecosystem reach. Unavoidable. Wrap it; don't put load-bearing logic in it. |
| Shell scripting | Bash 🟢 (portability floor) | — | **Nushell** 🟢 (Rust, structured data), fish | Nushell is the modern structured-data shell. Bash stays the floor: the one interpreter present on every target. |

> **Default Rust; reach for Go only when ecosystem gravity or distribution mechanics dominate — and log which rule fired.**

<details><summary><strong>The Rust-vs-Go CLI tie-breaker — the discriminator chain</strong></summary>

### The Rust-vs-Go CLI tie-breaker

Genuine tie — both ship single static binaries, both have mature arg-parsers (clap, cobra), both build fast. Run the discriminator chain in order, take the first hit:

1. **Shares/links an existing Rust core?** → **Rust.** A CLI over `PlausiDen-Engine`, a Shard lib, or any in-house crate keeps the through-line: no FFI seam, one language across the logic boundary. Dominates the other axes — a Go CLI over a Rust core re-introduces the wrapper boundary Layer 7 spends its effort deleting.
2. **Primarily glue into the Go-native cloud ecosystem?** (kubectl plugins, controllers/operators, k8s/cloud-SDK APIs) → **Go.** Same call as Layer 4's "ops/infra services → Go 🟡": swim with the current instead of hand-rolling Rust bindings to Go-first APIs. A downstream application of that stance, not a competing rule.
3. **Correctness of parsing/state is the point?** (config interpreters, migration tools — anywhere a missed case is silent corruption) → **Rust.** Sum types + exhaustiveness + no `nil` refuse the bug class Go leaves to discipline.
4. **Trivial cross-compilation / build speed is the binding constraint?** → **Go** edges it. Rust binaries are larger/slower to compile, but a `musl` static target plus `strip`/`opt-level` closes most of the gap.

Governance breaks any remaining tie: **Rust 🟢 over Go 🟡.** Default Rust; reach for Go only when ecosystem gravity (rule 2) or distribution mechanics (rule 4) dominate — and log which rule fired.

</details>

---

## 📦 Layer 11 — Infra-as-Code & Config

*TL;DR — Nix is a real lazy-functional language with real bug surface; treat it as code. Use OpenTofu, not Terraform — the BSL relicense is the textbook capture event the governance axis exists to catch.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Reproducible systems | **Nix / NixOS** 🟢 | — | — | The supersociety answer for reproducible infra. But Nix is a **real lazy-functional language with real bug surface** — see doctrine below. Treat it as code, not config. |
| Cloud provisioning | **OpenTofu** 🟢 (HCL) | — | — | **Use OpenTofu, not Terraform.** HashiCorp's BSL relicense is the exact vendor-capture event your filter exists to prevent; OpenTofu is the Linux Foundation FOSS fork. Case study below. |
| Typed config | **Nickel** 🟢 (Rust/Nix ecosystem) | CUE 🟢, Dhall 🟢 | — | Escape YAML. Nickel carries contracts + a Nix lineage; CUE for constraint-heavy schema unification; Dhall for total, non-Turing-complete config. All three are config languages, not policy — Layer 11b holds the doctrine. |

> **A point-in-time license is not governance.** The filter's real question is not "is it FOSS today?" but "**who can unilaterally change the terms, and is that structurally impossible?**" Score the governance structure, not the current SPDX tag.

<details><summary><strong>The OpenTofu/HashiCorp-BSL capture case study, and the Nix language doctrine</strong></summary>

### Case study: the OpenTofu / HashiCorp-BSL capture event

The textbook instance of the governance axis paying off — exactly the failure the FOSS/foundation filter is built to catch.

- **The bait.** Terraform shipped for years under MPL 2.0 — permissive, OSI-approved. By the filter's naive form ("is the license FOSS?") it passed cleanly; a whole ecosystem (modules, providers, CI tooling, careers) accreted on that assumption.
- **The switch.** HashiCorp relicensed Terraform (and its other core products) MPL 2.0 → **Business Source License (BSL)** — source-available, with a non-compete barring competing commercial use. Going forward only; prior history stayed MPL. A permissive license today did **not** bar a relicense tomorrow, because ownership sat with a single vendor able to change terms unilaterally.
- **The fork.** The community forked the last MPL Terraform — first OpenTF, renamed **OpenTofu** when accepted under the **Linux Foundation** (not CNCF), reaching a production-ready stable release shortly after. Foundation stewardship is the structural fix: no single owner can relicense it.
- **The capstone.** HashiCorp was subsequently acquired by **IBM**, folding the BSL incumbent into a large vendor — the clean endpoint of the arc the fork pre-empted.

**The durable lesson (the payload, not the trivia):** *a point-in-time license is not governance.* A permissive license protects you only if ownership is **distributed** — foundation-held, multi-stakeholder, or copyleft with no CLA-assignment escape hatch. Single-vendor + permissive is a relicense waiting for a business reason. The filter's real question is not "is it FOSS today?" but "**who can unilaterally change the terms, and is that structurally impossible?**" Score the governance structure, not the current SPDX tag.

### Nix language doctrine (it's a language, give it one)

Lazy, dynamically typed, impure-at-the-edges. The footguns are not where intuition points:

- **Headline risk — the store is world-readable.** Any secret string-interpolated into a derivation, `builtins.readFile`'d into config, or otherwise touched during evaluation lands in `/nix/store` readable by *every local user*. This is the AVP-2-relevant Nix failure, worse than shell injection. **Mandate out-of-store secrets (sops-nix or agenix); forbid secret material in derivation inputs entirely.** Assume-breach + blast-radius both bite here.
- **`with` is the real scoping footgun, not `let … in`.** `let … in` is lexically scoped and well-behaved. `with pkgs;` introduces names that don't shadow lexical bindings predictably — silently masking or failing to mask, producing wrong-but-evaluating configs. Constrain or ban `with` at module scope.
- **`rec` self-reference** → accidental infinite recursion. Prefer explicit `let` bindings.
- **Type-tighten `mkOption`.** Unconstrained option types (`types.attrs`, raw `types.unspecified`) defeat the module system's only real safety check. Use precise submodule types.
- **IFD (import-from-derivation) discipline** — IFD serializes evaluation and hides build steps inside eval; ban it in CI-critical paths.
- **`pkgs.runCommand` / `writeShellScript`** are shell-injection surface — quote and validate inputs as you would any `sh -c`.
- **String-context loss silently drops dependency edges.** A store path carries an invisible *string context* recording which derivations it depends on. `toString`, and especially `builtins.unsafeDiscardStringContext`, strip it — the path still looks valid but is no longer registered as a build input. Result: the GC reclaims it, or it's absent on another machine, and the failure surfaces far from the cause. Never discard context to "make the type check pass"; fix the actual reference.
- **Impure evaluation defeats the entire point.** Ambient `<nixpkgs>` via `NIX_PATH`, `--impure`, `builtins.currentTime`, `getEnv`, and channel-based (vs flake-pinned) inputs all let the outside world leak into evaluation — so the "reproducible" build reproduces differently per machine and per day. Channels drift; flakes pin. Pin inputs in a lockfile and forbid `--impure` in CI; an impure build that happens to work is a reproducibility bug that hasn't fired yet.
- **Laziness defers errors to force-time.** An expression can "evaluate" fine and then explode only when a downstream consumer *forces* the offending thunk — a config passes a shallow check and fails at deploy. "It evaluated" ≠ "it's correct"; force the values you depend on (build the actual derivation in CI) rather than trusting that evaluation reaching the top level proves anything about the leaves.

</details>

---

## 🛡️ Layer 11b — Config-as-Policy (format-neutral security doctrine)

*TL;DR — config formats are not languages; they get policy doctrine. Lint the policy class (W^X, ingress guarantees), not the file syntax — it survives the tool swap.*

> **Lint the *policy*, not the file.** Write the doctrine against the *capabilities*, not the directive names — it survives the migration off any one tool.

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

Two corollaries make the rule operational:
- **Express the class once, render to each format.** The class is the source of truth; the systemd unit, the SecurityContext, the seccomp profile are *renderings* of it. A typed-config layer (Layer 11's Nickel/CUE/Dhall) is the natural home for the canonical statement, emitting per-tool output — so the W^X-or-die assertion is authored once and a swap is a re-render, not a rewrite.
- **The capture lesson transfers.** Just as a rule bound to "Caddyfile" dies when you swap Caddy, a policy *engine* bound to a single vendor is itself a capture risk — prefer enforcement on portable primitives (the kernel's seccomp/LSM interfaces, the OCI spec) over a proprietary policy SaaS. Config-as-policy is governed by the same axis as everything else here.

---

## ⛓️ Layer 12 — Blockchain & ZK

*TL;DR — a zkVM proves ordinary Rust compiled to RISC-V; default to that and drop to a circuit DSL only for hot fixed gadgets. STARK-only is the sovereignty position carried into cryptography.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Chain logic / zkVM | **Rust** 🟢 (SP1, RISC Zero, Substrate, Solana) | — | — | Already your stack. Correct. A zkVM proves execution of ordinary Rust compiled to a RISC-V target — the same logic crate runs natively *and* gets proven, no DSL rewrite. The through-line applied to ZK: prove the real code, not a reimplementation. |
| EVM contracts | Solidity 🟢 | Vyper 🟢 | — | Unavoidable for Ethereum. Vyper trades expressiveness for a smaller, more auditable surface (no inheritance, bounded loops) — reach for it when the contract is security-critical and simple enough to fit the constraint. |
| ZK circuits | **Noir** 🟢, **Cairo** 🟢 (Starknet/STARK) | Circom, Halo2 (Rust) | — | Cairo aligns with your STARK-only/SP1 posture. Noir is backend-agnostic (targets multiple proving systems); Cairo is STARK-native. Circom/Halo2 are the lower-level fallback for hand-tuning the constraint system the higher-level languages generate. |

> **Default to the zkVM (Rust) for application logic; drop to a circuit DSL only for the gadget where prover cost justifies the rewrite.**

<details><summary><strong>zkVM vs circuit DSL, and the STARK-only posture — rationale</strong></summary>

The split to keep straight: a **zkVM** proves a *program* (write normal Rust, get a proof of its execution); a **circuit DSL** (Noir, Cairo, Circom) makes you express computation *as* an arithmetic constraint system. zkVM is cheaper in developer time and stays one language; hand-written circuits win on proof size/cost for hot, fixed primitives. Default to the zkVM (Rust) for application logic; drop to a circuit DSL only for the gadget where prover cost justifies the rewrite. The STARK-only posture (transparent setup, post-quantum-plausible hashes, no trusted ceremony) is the governance/sovereignty position carried into cryptography — it favors SP1/Cairo over pairing-based SNARKs that need a trusted setup.

</details>

---

## ✅ Layer 13 — Formal Methods & Correctness

*TL;DR — Lean 4 is now a real language, not just a prover; Verus/Creusot prove the shipping Rust, not a model; spec the consensus/tally protocol in TLA+ before writing Rust.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Theorem proving / verified programs | **Lean 4** 🟢 | Rocq (Coq) 🟢, Agda 🟢, Idris 2 🟢 | — | Lean is now a real programming language, not just a prover — see note below. |
| Verified systems code | **Verus / Creusot / Aeneas** 🟢 (Rust) | SPARK/Ada 🟢 | — | Prove your actual production code, not a model of it. Verus annotates Rust with pre/post/invariants checked by an SMT solver; Creusot/Aeneas translate Rust to a proof backend. The proof stays attached to the shipping code — no separate model to drift. |
| Protocol / consensus design | **TLA+** 🟢 | Quint 🟢 (modern TLA+), Alloy | — | **Spec the dual-chain consensus and Sacred.Vote tally protocol in TLA+/Quint before writing Rust.** Catches the bugs tests never will. Concrete workflow below. |

> **Spec the dual-chain consensus and Sacred.Vote tally protocol in TLA+/Quint before writing Rust.** Find the consensus/tally bug *before the code exists*, when the fix is a spec edit, not a re-architecture.

<details><summary><strong>Lean 4 as a language, and the "spec in TLA+ before Rust" workflow</strong></summary>

### Lean 4 as a language (not just a prover)

Why Lean 4 is Primary over Rocq/Agda/Idris: it crossed from "proof assistant with a toy execution model" into a genuine general-purpose language.

- **Self-hosting.** Lean 4's compiler and much of its tooling are written in Lean — a signal the language is expressive enough for real systems work, not just stating theorems.
- **Compiles to native via C**, with monadic `IO` and an FFI — you write actual programs (Lean's own toolchain, `mathlib` automation) and run them at near-native speed, not just check proofs.
- **Metaprogramming in Lean.** Tactics, macros, and elaboration are extensible *in Lean*, so automation and DSLs live in the same language as the proofs.
- **One language, no extraction step.** Dependent types unify program and proof: the thing you run *is* the thing you proved, eliminating the Coq→OCaml extraction gap where extracted code can diverge from the verified model. Same "prove the real artifact" principle as Verus above, at its limit.
- **Ecosystem anchor: `mathlib`** — the large communal mathematics library gives Lean a network effect the other provers' libraries don't match.
- **Governance bonus** (supports the 🟢): development moved to an independent foundation (the Lean FRO) and the implementation is Apache-2.0 — not single-vendor-captive, which is why it earns 🟢 not 🟡 despite originating in a corporate research lab.

### The "spec in TLA+ before Rust" workflow, made concrete

1. **Model the protocol as a state machine.** Declare state variables (each node's view of chain height, set of votes received, committed values), an `Init` predicate for legal starting states, and a `Next` action = disjunction of every atomic step a node can take (receive vote, propose, commit, time out).
2. **State the properties separately from the model:**
   - a **safety invariant** — must *never* be false, e.g. "no two honest nodes commit different values at the same height" (agreement), or for the tally: "the published count equals the multiset of validly cast ballots."
   - a **liveness property** — must *eventually* hold, e.g. "every validly submitted vote is eventually tallied," under fairness assumptions on message delivery.
3. **Model-check on a small finite instance.** Run **TLC** (explicit-state checker) over a deliberately tiny config — a handful of nodes, a couple of heights, one Byzantine node — to exhaustively explore the interleavings a reviewer and a test suite both miss. For state too large to enumerate, **Apalache** does symbolic (SMT-backed) checking. **Quint** is the modern surface syntax that transpiles to TLA+ when the classic syntax is a barrier.
4. **Carry the invariants into the implementation.** The safety invariant becomes a Rust `proptest`/model-based test plus runtime `debug_assert!`s; the liveness property informs timeout/retry design.

Load-bearing caveat: **this is a design-time exercise and the spec does not auto-bind to the Rust.** TLA+ proves the *protocol* sound; it does not prove your implementation refines it. The whole value is finding the consensus/tally bug *before the code exists*, when the fix is a spec edit, not a re-architecture — which is exactly why the ecosystem-mapping row for Layer 13 targets Sacred.Vote's consensus/tally for this treatment.

</details>

---

## 🔍 Layer 14 — Security / RE / Offensive

*TL;DR — Rust for tooling (memory safety in the tool that probes memory-unsafe targets); Python for one-shot PoCs (pwntools/scapy); Ghidra is genuinely FOSS despite the JVM it rides on.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Tooling | **Rust** 🟢 / C | — | — | Memory safety in the tool that probes memory-unsafe targets; the same crates back production and offensive use. |
| Exploit PoC / scripting | Python 🟢 | — | — | The ecosystem (pwntools, scapy, fuzzing harnesses) is Python; speed of iteration beats runtime perf for one-shot PoCs. |
| Reverse engineering | Assembly (x86/ARM) + Ghidra 🟢 (Java platform) | — | — | Ghidra is NSA-released but genuinely FOSS (Apache-2.0); the asset is its decompiler + scriptable analysis, not the JVM it rides on. |

> **The asset is the decompiler + scriptable analysis, not the JVM it rides on.**

---

## 🎮 Layer 15 — Game Dev (if ever relevant)

*TL;DR — Godot (MIT, foundation-governed) is the sovereign default; Bevy if the game can ride the Rust core. The Unity runtime-fee episode is itself a capture lesson.*

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| FOSS engine | **Godot/GDScript** 🟢 or **Bevy/Rust** 🟢 | — | — | Avoid Unity (proprietary, licensing-volatile) and Unreal/C++ unless AAA. |

> **The Unity runtime-fee episode is itself a Layer-11-style capture lesson — a single vendor changed the terms unilaterally.**

Godot (MIT-licensed, foundation-governed) is the sovereign default for shipping; Bevy if the game can ride the existing Rust core. Avoid Unity (proprietary, with a demonstrated history of licensing volatility) and Unreal/C++ unless genuinely AAA. The Unity runtime-fee episode is itself a Layer-11-style capture lesson — a single vendor changed the terms unilaterally; FOSS engines clear that bar.

---

## 🌉 Layer 16 — Cross-cutting infrastructure

*TL;DR — pick the engine for fitness, pick the format for sovereignty. The densest concentration of capture case studies in the stack: Redis→Valkey, Elasticsearch→OpenSearch, Terraform→OpenTofu — the same event across data, search, and infra.*

> **The durable unit is the interface — the wire format, the schema, the contract — not the engine that speaks it.** The format is what survives the engine swap.

These domains sit *across* every layer rather than inside one, and they are where the through-line is most literal. Pick the engine for fitness; pick the *format* for sovereignty, because the format is what survives the engine swap. This layer is also the densest concentration of vendor-capture case studies in the whole stack: Redis→Valkey, Elasticsearch→OpenSearch, Terraform→OpenTofu (Layer 11) are the same event playing out across data, search, and infra. Treat each fork as proof the governance axis earns its keep.

### Serialization & data interchange

| Domain | Primary | Fallback / Legacy | Watch | Notes |
|---|---|---|---|---|
| Human-readable interchange | **JSON** 🟢 (IETF/ECMA std) | YAML 🟢 (config only — see Layer 11) | — | The ubiquitous floor. Not the most efficient, but the most universally decodable; pay its tax where reach matters more than bytes. |
| Schema'd binary (data-at-rest / interchange) | **protobuf** 🟢 (BSD-3-Clause) | — | — | Schema-first, compact, polyglot codegen. License is FOSS but **governance is Google-controlled** — the schema you author is yours and portable; the toolchain's direction is not. For **RPC/wire** (gRPC, Cap'n Proto) defer to [Layer 4](#-layer-4--backend-servers-concurrency); that row already owns the durable-interface call. This row is encoding, not transport. |
| Sovereign standards-track binary | **CBOR** 🟢 (IETF RFC 8949) | MessagePack 🟢 | — | CBOR is the standards-body binary format — it underpins COSE and WebAuthn/passkeys, so it is already load-bearing in the security stack. Prefer it when you want a binary encoding with *no vendor behind it*. MessagePack is the simpler, faster-to-reach equivalent without the standards pedigree. |
| Zero-copy / mmap-able | Cap'n Proto 🟢, FlatBuffers 🟢 (Apache-2.0) | — | rkyv 🟢 (Rust) | When deserialization cost dominates (game state, IPC, on-disk indices). rkyv is the Rust-native zero-copy path; fits a Rust spine without a separate schema language. |

### Messaging & streaming

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Lightweight pub/sub & request-reply | **NATS** 🟢 (CNCF; Apache-2.0) | — | — | Small, fast, operationally simple; JetStream adds persistence. **Governance note:** a recent NATS trademark dispute (a move to withdraw the trademark donation, later resolved with the mark remaining at CNCF) was a near-miss capture event — the project stayed in the foundation, but it is a reminder to verify governance hasn't shifted before you bet a control plane on it. |
| High-throughput durable log | **Apache Kafka** 🟢 (ASF) | — | Redpanda 🔴/🟡 (Rust/C++, Kafka-API-compatible, **BSL single-vendor**), Iggy 🟢 ⏳ (Rust, CNCF sandbox) | Kafka is foundation-governed and the de-facto event-log standard; the cost is JVM operational weight. Redpanda is the tempting drop-in (no JVM, Kafka wire-compatible) but its BSL license + single-vendor control is exactly the capture pattern to price before adopting — the Kafka *protocol* is the durable interface, so a future swap back is cheaper than it looks. |
| Traditional broker / AMQP | RabbitMQ 🟡 (MPL-2.0) | — | — | Mature, flexible routing. **Governance flag:** now under Broadcom (via the VMware acquisition) — license stays open, but steward incentives changed; the same vigilance OpenTofu's story teaches. |

### Search & indexing

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Embedded full-text search library | **Tantivy** 🟢 (Rust, MIT) | — | Quickwit 🟢 (Rust, distributed, built on Tantivy) | Lucene-class search as a *library you embed*, not a server you operate — the sovereign default and consistent with [Layer 5](#-layer-5--data--databases): new engines are Rust. No JVM, no separate cluster, no vendor. |
| Turnkey search server | **Meilisearch** 🟡 (Rust, MIT) | Typesense 🟢 | — | Rust, fast, good DX. 🟡 because it is single-vendor open-core (advanced/cloud features gated) — fine as a forced dependency kept thin, not a governance ideal. |
| Distributed search cluster | **OpenSearch** 🟢 (Apache-2.0, Linux Foundation) | — | — | **Capture case study.** Elasticsearch relicensed from Apache-2.0 to SSPL/Elastic License — the exact vendor-capture event the governance filter exists to catch; AWS forked OpenSearch under Apache-2.0, later moving it to an independent foundation. Elastic *re-added* an AGPLv3 option, but that walk-back does not undo the fork or the lesson. **Primary stays the foundation fork.** Elasticsearch itself is 🔴 on the governance axis despite the partial relicense. |

### Caching & KV

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| In-memory cache / KV store | **Valkey** 🟢 (Linux Foundation, BSD-3) | — | Dragonfly 🔴/🟡 (BSL single-vendor), KeyDB | **The cleanest capture case study in the stack.** Redis relicensed away from BSD to a source-available license (SSPL/RSAL); the maintainer community + cloud backers forked **Valkey** under the Linux Foundation, keeping the BSD-3 lineage and Redis wire-protocol compatibility. Redis later re-added an AGPLv3 option but — as with Elasticsearch — the walk-back follows the fork rather than reversing it. **Primary = Valkey.** Redis is 🔴→🟡: the relicense already happened; the protocol is the durable interface, so existing Redis clients keep working against Valkey. Dragonfly is faster but BSL — price the capture. |

### Observability backends

The jackpot row for the through-line: **OpenTelemetry/OTLP is the durable wire format; backends swap underneath it.** Instrument once against OTel, and Prometheus, Tempo/Jaeger, Loki, or any vendor SaaS becomes a swappable implementation behind a stable interface — the same logic as protobuf, SQL, and WASM elsewhere in this doc. Never instrument against a backend's native SDK; instrument against OTel.

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| Instrumentation / wire format | **OpenTelemetry (OTLP)** 🟢 (CNCF) | — | — | The durable interface. Vendor-neutral traces/metrics/logs; the one piece you bind to permanently so everything else stays swappable. |
| Metrics store + alerting | **Prometheus** 🟢 (CNCF, graduated) | VictoriaMetrics 🟢 | Mimir 🟡 (Grafana-steered) | Foundation-governed, OTLP-ingesting. The sovereign metrics floor. |
| Traces backend | **Tempo** 🟢 / **Jaeger** 🟢 (CNCF) | — | — | Both OTLP-native; pick on operational fit. The trace data is portable because it arrived via OTel. |
| Dashboards / visualization | Grafana 🟡 (AGPLv3) | Perses 🟢 ⏳ (CNCF, dashboards-as-code) | — | **Governance flag:** Grafana relicensed to AGPLv3 and is single-vendor-steered. Usable, dominant, but keep dashboards portable (export JSON/as-code) so a move to Perses or another renderer costs days, not a rebuild. |

### API schema & contracts

| Domain | Primary | Fallback | Watch | Notes |
|---|---|---|---|---|
| REST/HTTP API contract | **OpenAPI** 🟢 (Linux Foundation / OAI) | — | — | Foundation-governed, vendor-neutral. The contract is the durable artifact; generate clients/servers/mocks from it, don't hand-write them. |
| RPC / service contract | **protobuf + gRPC** 🟢 | — | — | Cross-ref [Layer 4](#-layer-4--backend-servers-concurrency) (wire protocols). The `.proto` is the contract of record. |
| Event-driven contract | **AsyncAPI** 🟢 (Linux Foundation) | — | — | The OpenAPI-equivalent for the messaging row above — describe NATS/Kafka topics and payloads as a versioned contract, not tribal knowledge. |
| Payload/data shape validation | **JSON Schema** 🟢 (IETF-track) | — | — | Validates the JSON interchange row; the shared substrate beneath OpenAPI and AsyncAPI. |

> **Canon tie-in.** API contracts are to services what the policy *class* is to config (Layer 11b): the durable, lintable unit that outlives any one implementation. Just as Canon lints the policy class (W^X, ingress guarantees) rather than the Caddyfile syntax, Canon should lint **contract conformance** — does the running service match its OpenAPI/protobuf/AsyncAPI contract; do schema changes stay backward-compatible — rather than any single framework's annotations. A test tied to "the FastAPI route" dies on a Rust/Axum rewrite; a test tied to "conforms to `openapi.yaml`" survives the swap. Contracts are the durable unit; enforce against the contract, exactly as [`PlausiDen-Canon`](https://github.com/thepictishbeast/PlausiDen-Canon) enforces against the policy class, and feed the result back into [`SECURITY_BASELINE.md`](SECURITY_BASELINE.md).

---

## 🧷 The interop spine

*TL;DR — four stable boundaries (UniFFI, WASM Component Model, protobuf/Cap'n Proto, SQL) carry the architecture; languages are chosen so implementations swap while the interface holds.*

> **Durable abstraction over swappable instantiation, applied to language boundaries.** Choose languages so these four boundaries stay stable while everything behind them remains free to swap.

The durable unit is the **interface**, not the language. Languages churn on a ~10–20yr cycle; a well-chosen interface outlives the implementations behind it. So the architecture is built around four stable boundaries, and languages are selected so that *implementations can be replaced while the interface holds*. This is the same pattern that recurs elsewhere in the doctrine — build on seL4's *proof corpus* rather than its C (Layer 2), and attach security doctrine to the *policy class* rather than the config format (Layer 11b / the Config-as-Policy corollary). Durable abstraction over swappable instantiation, applied to language boundaries.

The four load-bearing interfaces:

| Boundary | Interface | Governance | What it lets you swap | Where it's instantiated |
|---|---|---|---|---|
| FFI / mobile core | **UniFFI** 🟢 | FOSS (Mozilla-origin, community) | The platform UI shell (Kotlin, Swift) over a single Rust logic core — and, in principle, the binding generator itself. The *core* is the asset; the shells are generated, disposable, and gatekeeper-replaceable. | [`PlausiDen-Android`](https://github.com/thepictishbeast/PlausiDen-Android) (Rust core + thin Kotlin) — see Layer 7 |
| Component / web / plugins | **WASM Component Model** 🟢 | FOSS (Bytecode Alliance) | The language *inside* a component (Rust, and others as the model matures) behind a typed, language-agnostic boundary. Still maturing — it is the *migration target* off TS/React, not yet a displacement; TS stays the browser floor until it lands. | [`PlausiDen-Browser-Ext`](https://github.com/thepictishbeast/PlausiDen-Browser-Ext) (engine→WASM), [`Linux-File-Manager`](https://github.com/thepictishbeast/Linux-File-Manager-and-UI-for-Remote-Access-) (Leptos WASM) — see Layer 9 |
| Wire / RPC | **protobuf/gRPC, Cap'n Proto** 🟢 | FOSS, language-agnostic by design | The service implementation on either end of the wire — rewrite a backend Rust→BEAM or split a monolith without renegotiating the contract. The schema is the durable artifact. | Layer 4 (wire protocols) |
| Data | **SQL** 🟢 | open standard, ~50yr, multi-vendor | The storage engine under the query (Postgres, SQLite, a Rust-built engine) — SQL has outlived every paradigm that tried to replace it. Bet hard on it; it is the most future-proof boundary you have. | [`PlausiDen-CRM`](https://github.com/thepictishbeast/PlausiDen-CRM), [`PlausiDen-Salesman`](https://github.com/thepictishbeast/PlausiDen-Salesman) — see Layer 5 |

Read across any row: the *interface* is 🟢 and durable, so even when the implementation behind it is forced 🔴 (an Apple-gatekept Swift shell over a UniFFI core; a CUDA kernel behind a wgpu-shaped boundary), the capture is contained at the edge and the logic stays sovereign and portable. That containment is what makes the [decision heuristic](#-how-to-choose--the-decision-heuristic)'s "take it, keep it thin" survivable in practice: the thinness is enforced *by* the interface. Choose languages so these four boundaries stay stable while everything behind them remains free to swap.

---

## 🚫 Anti-patterns & disqualified

*TL;DR — two tiers split by the hinge rule: disqualified (a viable escape exists, so don't pay the capture cost) vs forced-mitigate (no equal escape today, so adopt thin behind a durable interface). The tier is the load-bearing distinction.*

> **Disqualified = a viable alternative exists, so the capture cost is not worth paying. Forced-mitigate = no equal escape today, so you adopt deliberately and keep it thin.** Do not collapse the two tiers.

The avoid-list, consolidated with the governance WHY for each. The tier split is the hinge rule from the [decision heuristic](#-how-to-choose--the-decision-heuristic).

| Avoid | Tier | Governance WHY | What to reach for instead |
|---|---|---|---|
| **Mojo** | disqualified 🔴 | Technically exciting, but the **compiler is closed** (Modular Community License), single-vendor. It fails the FOSS/PSA filter on governance, not on capability. Revisit *only* if the compiler becomes OSI-licensed + foundation-governed. | **Rust** (candle, burn) for production ML inference — 🟢 and already the LFI core's language. (Layer 6.) |
| **Electron** | disqualified 🔴(bloat) | Not a governance-capture case but a resource/footprint anti-pattern: a full Chromium per app. A sovereign, tiny-binary alternative exists, so paying the bloat tax is a default-in-disguise, not a deliberate trade. | **Tauri** (Rust core + web UI) 🟢; Dioxus 🟢, Slint 🟢. (Layer 8.) |
| **Unity** | disqualified 🔴 | Proprietary engine with a demonstrated history of **licensing volatility** — the governance risk is the vendor's freedom to change pricing/terms unilaterally, which is exactly the capture event the filter exists to catch. FOSS engines clear it. | **Godot** 🟢 or **Bevy/Rust** 🟢. (Layer 15.) Avoid Unreal/C++ too, unless genuinely AAA. |
| **Terraform** | disqualified 🔴 | HashiCorp's **BSL relicense is the textbook vendor-capture event** this doctrine's governance axis was built to flag — a once-open tool relicensed out from under its users. A drop-in FOSS fork under neutral governance exists, so there is no excuse to stay captured. | **OpenTofu** (Linux Foundation FOSS fork, HCL-compatible) 🟢. *Use OpenTofu, not Terraform.* (Layer 11.) |
| **CUDA** | forced-mitigate 🔴 | NVIDIA-captive — *the deepest moat in computing*. **Not disqualified**, because the sovereign escape (wgpu) is genuinely **slower today**; when the perf gap is unacceptable the escape isn't yet viable, so the hinge rule says you take CUDA and keep it thin. Adopt deliberately, isolate behind a kernel boundary, and migrate to wgpu as it closes the gap. | **wgpu/WGSL** 🟢 (the sovereign target); ROCm/HIP 🟡 as a partial hedge. (Layer 0.) |
| **iOS / Swift platform** | forced-mitigate 🔴 | Swift-the-language is open; the **platform and tooling are Apple-gatekept** — the highest capture risk in the stack. No escape exists (you cannot ship to iOS off-platform), so this is forced, not chosen. Mitigate, don't disqualify. | Keep it a **thin shell over a Rust + UniFFI core** (Layer 7 / [interop spine](#-the-interop-spine)); never let logic live in the captive layer. |

Everything in the **disqualified** tier shares one property: a viable 🟢 alternative exists *right now*, so depending on the captured tool is paying capture cost by default — the discipline forbids it. Everything in **forced-mitigate** lacks that escape today; you adopt it consciously, price the risk in your decision log, and keep the captive surface as thin and swappable as the interop spine allows. The instant a forced-mitigate dependency grows a viable escape (wgpu reaching parity; an open iOS target), re-run the [heuristic](#-how-to-choose--the-decision-heuristic) — it moves to disqualified. That re-derivation is the quarterly job; this is a snapshot, not prophecy.

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

- **2026.06-r4** — Major depth expansion. Every layer (0–16) deepened with explicit Primary-vs-Fallback reasoning, migration paths, and maturity caveats folded into collapsible rationale blocks. Added three cross-cutting reference sections — **How to choose** (the ordered decision heuristic + hinge rule), **The interop spine** (the four durable boundaries), and **Anti-patterns & disqualified** (the consolidated avoid-list, two-tier) — plus a new **Layer 16 — Cross-cutting infrastructure** (serialization, messaging, search, caching/KV, observability, API contracts; governance case studies Redis→Valkey, Elasticsearch→OpenSearch, the OTel-as-durable-interface pattern). Added a governance scorecard and a uniform visual system (per-layer emoji + TL;DR, `>` doctrine callouts, `<details>` rationale). Layer picks, flags, Legend, through-line, and all r3 doctrine preserved unchanged.
- **2026.06-r3** — Adopted into PlausiDen-Meta as ecosystem doctrine. Added: layer index + nav, how-to-use, the PlausiDen ecosystem mapping, doctrine cross-references (baselines, AVP gates), badges, and a cross-link to `PlausiDen-AI/docs/SUPERSOCIETY_STACK.md`. Layer content unchanged from r2.
- **2026.05-r2** — Expanded Nix from a one-liner to a real language-doctrine entry; added Layer 11b (Config-as-Policy); added the "doctrine attaches to the policy class, not the file format" principle.

**Cadence:** reviewed quarterly (next: 2026-09). Amendments tracked in [`CHANGELOG.toml`](CHANGELOG.toml); decisions in [`DECISION_LOG.md`](DECISION_LOG.md). Language dominance churns on a decade cycle — this is a snapshot, re-derive it as material conditions change.
