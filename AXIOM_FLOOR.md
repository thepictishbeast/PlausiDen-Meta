# Axiom Floor

Every doctrine system that audits itself eventually bottoms out. Audits audits Audits. AVP grades AVP. Doctrine has meta-doctrine. This is intellectually satisfying and sometimes genuinely useful — but at some level you assert axioms by fiat and stop, or every discussion recurses into meta-meta-meta territory and nothing ships.

**This document declares the bottom explicitly.**

## The floor

**`PlausiDen-AVP-Doctrine` Tier 0 — "does this exist and is it internally consistent" — is the axiom floor.** Below Tier 0, claims are asserted by fiat.

The asserted-by-fiat claims:

1. **PlausiDen-AVP-Doctrine exists and is authoritative for grading other PlausiDen artifacts.** Not because we proved it; because we declared it.

2. **The tier system in PlausiDen-AVP-Doctrine is a useful organizing principle for validation rigor.** Asserted, not proven.

3. **The four operating principles in [`OPERATING_PRINCIPLES.md`](OPERATING_PRINCIPLES.md) are the right principles.** Asserted, not derived from a higher source.

4. **`PlausiDen-Meta`'s priority doc is the legitimate gate for "should I build X" decisions.** Asserted by the maintainer.

5. **The maintainer's judgment is the final court of appeal.** No higher authority. (As more contributors join, this devolves to "the consensus of named maintainers.")

## Why declare a floor

Without an explicit floor, three failure modes:

- **Infinite recursion in design conversations.** "But who grades AVP-Doctrine? And who grades the grader?" Productive discussion stops being productive at some recursion depth.
- **Weaponized meta-skepticism.** "Your doctrine is invalid because the grading system is itself ungraded." With a floor, the response is: "Correct, the floor is asserted. Argue with the floor if you want to argue with the floor."
- **Paralysis on amendments.** Without a floor, every change requires recursive validation. With a floor, changes are validated up to the floor and then asserted onward.

## What's NOT in the floor

These look foundational but aren't axioms — they're derived from the floor + the principles:

- The choice to ship `plausiden-obs` first (Tier 0). *Derived from*: time budget + LFI line count + observability leverage analysis.
- The decision to write doctrine in TOML. *Derived from*: tooling consideration + readability + zero-dep parsing.
- The MIT license choice. *Derived from*: FOSS-first stance + contributor friction minimization.

These are revisable with rationale. The floor is not.

## How the floor changes

The floor itself can change — but only by an explicit `AXIOM_FLOOR_AMENDMENT` PR with a 30-day public comment period (vs. the standard 7-day for amendments) and a mandatory post-merge documented justification. The friction is intentional. Floor changes are constitutional, not legislative.
