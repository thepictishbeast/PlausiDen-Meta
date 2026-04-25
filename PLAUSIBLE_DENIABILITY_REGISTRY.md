# Plausible-Deniability Tool Registry

Authoritative triage of public plausible-deniability projects evaluated
for value to the PlausiDen ecosystem. Cross-repo registry: each candidate
is tagged with the target PlausiDen-* repo(s) that would consume it.

Mirrors the per-repo registry discipline established in
[PlausiDen-Audits TOOL_REGISTRY](https://github.com/thepictishbeast/PlausiDen-Audits/blob/main/TOOL_REGISTRY.md),
[PlausiDen-Crawler CRAWLER_REGISTRY](https://github.com/thepictishbeast/PlausiDen-Crawler/blob/main/CRAWLER_REGISTRY.md),
[PlausiDen-Tests TEST_TOOL_REGISTRY](https://github.com/thepictishbeast/PlausiDen-Tests/blob/main/TEST_TOOL_REGISTRY.md),
and [Vulnerability-Scanner SCANNER_REGISTRY](https://github.com/thepictishbeast/Vulnerability-Scanner/blob/main/SCANNER_REGISTRY.md).

## Target PlausiDen-* repos

| Repo | What it does | Profile of useful PD candidate |
|---|---|---|
| **PlausiDen-Pollution** | Synthetic analytics event generation, HMAC-tagged fake traffic | Synthetic-data primitives, pattern realism |
| **PlausiDen-Engine** | Core data-pollution library: synthetic files, browser history, contacts, GPS, network | Generative primitives, hidden-volume patterns, stego |
| **PlausiDen-Android** | Background services for Android PD: files, contacts, calendar, usage patterns | Android-specific PD frameworks |
| **PlausiDen-Swarm** | P2P fragment pooling on Iroh; encrypted fragment redistribution | Distributed deniability protocols, dead-drop patterns |
| **PlausiDenOS** + **PlausiDen-OS-for-Mobile** | Sovereign mobile OS (seL4 microkernel, Pixel 10 Pro XL target) | Deniable filesystems, kernel-level PD |
| **PlausiDen-USB** | Hardware PD device (RP2040 / iCE40), generates cryptographic proof of connection | Hardware deniability primitives |
| **PlausiDen-Suite** | Distribution / packaging of all PD tools | Anything user-facing across the family |

## Status definitions

| Status | Meaning |
|---|---|
| **adopted** | Vendored or wired in to a target repo. |
| **adopted-as-dep** | Used directly via package manager / git submodule; no fork. |
| **deferred** | Genuine value but waiting on a specific trigger. |
| **reference-only** | Pattern source; we read the design / paper, did not absorb. |
| **rejected** | Considered and ruled out; **do not re-evaluate without new evidence**. |

---

## Strong absorption / reference candidates (deferred with triggers)

| Tool | Stars | Lang | Status | Target repo(s) | Notes |
|---|---|---|---|---|---|
| **atbarker/Artifice** | 15 | C | deferred | **PlausiDenOS / PDFS** | **Top architectural reference.** Plausibly-deniable block device on Linux Device Mapper. Recent updates (2026-03). The closest open-source analog to PDFS's threat model. Don't absorb (PDFS is Rust on seL4); read the paper + code for hidden-volume math + decoy-write strategy. |
| **srv1n/kurpod** | 161 | JS | deferred | **PlausiDen-Engine / PlausiDen-USB** | **Most-starred candidate on the list.** Self-hosted encrypted file storage with PD features. Recent maintenance. Read the storage layout for cross-platform PD storage patterns. JS-based; not absorbing the engine but pattern reference. |
| **Commitant/RabbitHole** | 21 | Inno Setup | reference-only | **PlausiDen-Engine** | TrueCrypt-style 256-bit encrypted archive with N hidden volumes. Windows/installer-focused. Reference for hidden-volume metadata layout (the single most replicated PD pattern in the wild — get the math right or the deniability is theatrical). |
| **mahmoudimus/synthetic-data-generation-framework** | 2 | C++ | deferred | **PlausiDen-Engine / PlausiDen-Pollution** | From Bindschaedler et al., "Plausible Deniability for Privacy-Preserving Data Synthesis" — the **canonical academic primitive** for our entire data-synthesis stack. Worth a careful read; cite the paper directly in PlausiDen-Engine's design docs. |
| **Ayush-Umu/Federated-Unlearning-under-Plausible-Deniability** | 3 | Python | reference-only | **PlausiDen-Swarm / PlausiDen-Engine** | Recent ACML 2024 paper on federated unlearning with PD guarantees. Relevant to Swarm's "any data on any device could belong to anyone" thesis. |
| **anirudhagarwal-dev/Shadow_Fold** | 2 | JS+WASM | reference-only | **PlausiDen-Engine** (browser surface) | Browser-native steganography via WebAssembly. Modern angle for the Engine's web-context outputs. |
| **AbsSadhu/ghost-protocol** | 0 | — | reference-only | **PlausiDen-Engine** | Steganography in JPEG DCT coefficients. Read the math; classic PD primitive. |
| **1nfocalypse/ROXy** | 0 | C++ | reference-only | **PlausiDen-Engine** | Toy implementation of Canetti et al. PDE construction — the academic foundation. Worth scanning for the algorithm rendering; not for absorption (it's a toy). |
| **byhsx/Refugedroid** | 0 | C | reference-only | **PlausiDen-Android** | Android security framework for hiding sensitive data. **Abandoned 2017** but the only Android-PD framework on the list. Read the threat model + UX patterns; do NOT absorb dead Android code. |
| **jimjimvalkema/ultra-anon** | 15 | Solidity | reference-only | **PlausiDen-Swarm / PlausiDen-Suite** | Privacy token with max PD + max anonymity set, joining public + private state. Solidity, on-chain — NOT directly absorbable but the "join public+private state" thesis maps onto Swarm's fragment pooling. Read the paper. |
| **jimjimvalkema/zktranswarp** | 4 | Solidity | reference-only | **PlausiDen-Suite** | ZK + cross-chain anonymity set. Same author as ultra-anon. Reference for cross-system anonymity architectures. |
| **whackashoe/plause** | 7 | Rust | reference-only | **PlausiDen-Engine** | Generic Rust PD library. Small (~128KB), abandoned 2021, but Rust-native — worth reading for any patterns we missed. |

## Single-purpose niche tools (reference-only)

These are small / single-issue projects — useful for one technique but not
worth absorbing whole. Vendor a 50-line equivalent if we want the pattern.

| Tool | Stars | Status | Target repo | Notes |
|---|---|---|---|---|
| **Amarilu84/afdw-secure-drive-wiper** | 0 | reference-only | PlausiDen-Engine (purge) | Anti-forensic drive wiper, Linux. Reference for purge-with-PD pattern; we have separate purge work and the standard `shred`/`wipe` are usually enough. |
| **kristophercasey/RHUBARBCIPHER** | 0 | reference-only | PlausiDen-Engine | Multi-key file encryption with PDE. Linux/BSD. Read the multi-key trick. |
| **godzillachan/plausible** | 7 | reference-only | PlausiDen-Engine | "Plausibly-deniable zone for activism." Python pattern. Read it for the social/operational framing. |
| **davidmcnabnz/PhoneBook** | 3 | reference-only | PlausiDenOS | Encrypted filesystem with PD, Python. Pattern reference (Python perf is unsuitable for our stack). |
| **vectorcrown/blue-wallet-project** | 0 | reject-as-template | — | Actually mature BlueWallet fork (105MB). Wallet UX with PD encryption — useful exemplar but off-topic for our PD repos. |
| **jonny255/ultimate-CyberSIGINT-phone** | 0 | reference-only | PlausiDenOS | Cuttlefish-based offload-to-cloud SIGINT toolkit. Conceptual reference for OS-level PD; not absorbable (cloud dependence violates SUPERSOCIETY_BASELINE). |
| **storopoli/WTF-CPF** | 1 | reference-only | PlausiDen-Pollution | Generates fake-but-format-valid Brazilian tax IDs (CPFs). Localized realism trick — generalize to other ID formats if we add geo-localized realism to Pollution. |
| **dhan4043/randomized-response** | 0 | reference-only | PlausiDen-Engine | Demonstrates randomized-response (Warner 1965) via ASCII images. Read the technique — RR is a foundational PD primitive. |
| **Aayu2810/file-encryption-tool** | 3 | reference-only | PlausiDen-Engine | Generic file encryption with stego + self-destruct. Pattern reference. |
| **rfinnie/tor_opd** | 2 | reference-only | PlausiDen-Swarm | "Operation Plausible Deniability" — 3KB shell script. Conceptual only; substance is in the README, not the code. |
| **martindale/byrd** | 1 | reference-only | PlausiDen-Swarm | GitHub mirror of distributed PD network. Abandoned 2019. Read for protocol pattern; assume the upstream gitlab is dead. |

## Rejected (too small, off-topic, abandoned, duplicate)

| Tool | Reason |
|---|---|
| **thefourthone/plausibly-deniable-generals** | Intersection of two unrelated projects (PD lib + generals.io game). Not on-topic. |
| **abramhindle/openidstinks** | 2014 OpenID rant ("spam.la of openIDs"). Off-topic. |
| **seethe529/PlausiblyDeniable** | 0★, no description, no language. Empty placeholder. |
| **genesisdotre/sensualconsensual.com** | A website, not a tool. Off-topic. |
| **Thomas717/Tom_Plausible_Deniability_Signer** | 0★, no detail, 17KB. Toy. |
| **angry-cupcake/DoPD** | "Department of Plausible Deniability" — name only, no apparent substance. |
| **mcnabola/plausible-deniability** | "Blog for plausible deniability." Not a tool. |
| **saifibnaezhararko/deniable-quantum-key-exchange-qiskit-pennylane** | Research toy (8KB), no implementation depth. Re-evaluate if PQ-deniable becomes a Suite requirement. |
| **stefan-rass/ml-with-plausible-deniability** | MATLAB (!), 2021, 27KB. Supplementary material to a paper. Read the paper; don't touch the MATLAB. |
| **paradigmp-ragna/Deniable-Encryption** | Self-described "Simulator that demonstrates concepts." Demo-tier. |
| **AsHfIEXE/BlackInk** | Distraction-free writing app. Off-topic for our PD repos (could fit a hypothetical PlausiDen-Notes that doesn't exist). |
| **princeton-ddss/safely-report** | Academic survey app. Niche; not in PD repo scope. |
| **ZeroDeadDrop/ZeroDeadDrop** | HTML/JS web app, 1★, recently updated but unproven. Watchlist; re-evaluate if it gains traction. |
| **lokomembershit-cloud/Euro-Chat** | 4KB, no language, marketing-only README. |
| **medivh-dll/excuse-machine** | "Plausible deniability generator — handcrafted excuses." Joke / toy. |
| **chandranshgupta/zero_point_proto** | "Incomplete prototype for HackSecure 2026 (NIT Hamirpur x MeitY)." Author flags it as incomplete. |

---

## Per-repo summary — what each PD-* repo could pull from this list

### PlausiDen-Engine
**Take:** the academic primitives (Bindschaedler synthesis paper, Canetti PDE
construction via ROXy, RabbitHole hidden-volume math, randomized-response
foundation). **Don't take:** code from any single project — all the engine's
code is original Rust; absorbing C/JS implementations would create
language-mismatch debt.

### PlausiDen-Pollution
**Take:** the localized-realism pattern from WTF-CPF (geo-valid synthetic
identifiers), Bindschaedler synthesis primitives (re-use from Engine).
**Don't take:** standalone synthesis frameworks — Pollution should leverage
Engine, not parallel-build.

### PlausiDen-Android
**Take:** the threat-model survey from Refugedroid (paper-only — code is
abandoned 2017). **Don't take:** any Refugedroid code; rebuild the few
useful patterns in Kotlin against modern Android APIs.

### PlausiDen-Swarm
**Take:** ultra-anon's "join public + private state" thesis as design
inspiration; Federated-Unlearning under PD as research backing for the
"any device could host any data" claim. **Don't take:** any Solidity
code (wrong runtime, wrong threat model overlap).

### PlausiDenOS / PlausiDen-OS-for-Mobile
**Take:** Artifice's deniable-block-device design as the architectural
reference for PDFS. **Don't take:** Artifice code (C on Linux DM; PDFS
is Rust on seL4).

### PlausiDen-USB
**Take:** kurpod's storage layout patterns for cross-platform PD data
shape. **Don't take:** kurpod engine (JS-based; USB device is RP2040
firmware).

### PlausiDen-Suite
**Take:** ultra-anon + zktranswarp as cross-system anonymity-set
references for any future Suite-level cross-product anonymity claim.
**Don't take:** Solidity contracts.

---

## Decision rules

1. **Stack alignment**: Rust is preferred for absorption. C / Python /
   JS / Solidity are pattern-reference only.
2. **Maintenance**: no commits in 18 months and < 100 stars → reject
   unless it's a paper-grade reference (then reference-only).
3. **PSA filter**: requires SaaS, phones home, depends on a single
   service-provider → reject per
   [`SUPERSOCIETY_BASELINE`](SUPERSOCIETY_BASELINE.md).
4. **Single-purpose tools**: reference-only at most. Vendor a small
   equivalent if we want the pattern.
5. **Pattern reference > code absorption**: PD is mostly about *getting
   the math right*. Reading papers + designs is higher-leverage than
   absorbing implementations.

## Triggers for the deferred set

| Tool | Trigger to upgrade from `deferred` → `adopted` |
|---|---|
| atbarker/Artifice | Starting active PDFS implementation in PlausiDen-OS-for-Mobile. |
| srv1n/kurpod | First cross-platform PD storage requirement that the Engine alone can't satisfy. |
| mahmoudimus/synthetic-data-generation-framework | Engine adds a paper-citation + algorithm-implementation pass for Bindschaedler primitives. |
| Ayush-Umu/Federated-Unlearning-under-Plausible-Deniability | Swarm adds federated-unlearning as a documented capability. |
| anirudhagarwal-dev/Shadow_Fold | Engine adds a browser-context output channel. |

## Refresh cadence

Quarterly, alongside the other registries (PlausiDen-Audits TOOL_REGISTRY,
PlausiDen-Crawler CRAWLER_REGISTRY, PlausiDen-Tests TEST_TOOL_REGISTRY,
Vulnerability-Scanner SCANNER_REGISTRY). Rejected items stay rejected
unless new evidence arrives (new maintainer, capability addition,
peer-reviewed publication, etc.).
