---
title: "MONAD: Minimal Ontological Notions for Axiomatic Distinction"
---

<a id="monad&#45;minimal&#45;ontological&#45;notions&#45;for&#45;axiomatic&#45;distinction"></a>

MONAD is the mathematics of [Causality](https://causality.in), whose unit and swarm kernels, MUON and HENA, compile against it\. It is a mathematical research programme with an executable kernel, and it asks how a mathematical system can define, compress, transport, and revise its own space of distinctions when future continuations, historical trajectories, and transformations can change what is distinguishable\.

For example, a description can compress two states into one because no admitted test separates them\. A later extension can then require exactly that lost distinction\. One checked obstruction makes the tradeoff precise: if the permitted questions separate every pair of states, preserving every answer exactly forces the summary to keep every distinct state apart\. No nontrivial merging remains possible under those assumptions\.

The repository has three surfaces\. A Lean 4 library owns the formal definitions and proofs\. A LaTeX paper explains the same mathematics, module by module\. A Rust crate, `monad-kernel`, holds typed formal objects that sibling research systems use as a shared vocabulary\.

<Info>

**MONAD 0\.1\.0 is the paper and the Lean library\.** Calibration and further proving are paused\. No package is published and no language SDK exists; research code uses `kernel/` directly\.

</Info>

## Use MONAD for a typed vocabulary or for the research itself {#use-monad-for-a-typed-vocabulary-or-for-the-research-itself}

Use MONAD when you need a typed, dependency\-free vocabulary for state spaces, observers, resource\-indexed distinguishability, and causal records, or when you work on the research programme itself\.

MONAD is a library and a body of mathematics\. It runs inside a consumer’s process, with no daemon, command\-line tool or service to start\.

## Start with the surface you need {#start-with-the-surface-you-need}

For mathematical understanding, [Descriptions and repair](https://causality.in/monad/descriptions-and-repair/) introduces the programme and [What a summary preserves](https://causality.in/monad/what-a-summary-preserves/) works through a finite compression and repair example\. The name MONAD is unrelated to the programming\-language or category\-theoretic use of “monad”\.

For a local source checkout, [Getting started](/monad/getting-started) owns the toolchain prerequisites and build commands\. [The kernel](/monad/kernel#a-first-rust-call) carries a Rust consumer through a finite\-distribution calculation\. [Working on the mathematics](/monad/mathematics#read-a-preservation-claim-in-lean) translates the product example into the existing Lean definitions and explains the quotient and obstruction hypotheses\. The Lean library and the paper are in `formal/` and `paper/`, and their instructions are in [the formal guide](/monad/formal)\.

Changes to the mathematics, paper or kernel require the full checks in that setup guide\. A passing Lean build checks formal proofs; the correspondence checker links paper proofs to declarations and separately refuses full coverage while obligations remain\. Neither check establishes that an executable consumer implements the entire mathematics\.

## Lean owns the mathematics and every other surface projects it {#lean-owns-the-mathematics-and-every-other-surface-projects-it}

| Surface | What a developer works with |
| --- | --- |
| Lean library | Definitions, theorems and their exact hypotheses |
| Paper | Mathematical explanations paired with the Lean modules |
| Rust kernel | Shared types and implemented finite\-distribution calculations |
| Planned SDKs and integrations | Language interfaces and renderers over one Rust facade; nothing is implemented, and each folder holds only a note on what it will be |

Research kernels use `kernel/` directly\. The planned language interfaces share one Rust facade, and renderers read the scene records it produces\. The [library design](/monad/library-design) owns that arrangement, the five picture primitives and the prerequisites for SDKs, portable contracts and GPU work\.

## Many results are proved and three groups remain open {#many-results-are-proved-and-three-groups-remain-open}

The Lean library and the paper share 42 mirrored modules\. Checked results include exact quotients and refinement, residual\-budget continuation laws, partition abstraction, adaptive repair feasibility and optimal cost, exact supplement coding, statistical simulation and bounded\-risk transfer, temporal elimination and a mixed\-boundary resonance obstruction, a nonlocal variational construction with a strong Euler equation almost everywhere, finite influence counterexamples, measure\-theoretic entropy conservation, mutual\-information processing and capacity transport on arbitrary measurable spaces, the full finite\-alphabet Chernoff exponent, a moving\-support counterexample, and scalar Hellinger and total\-variation coefficients\.

Three groups of obligations are open: operational coding and local testing, quantum correspondence, and a statement\-level review of the whole paper\. [formal/coverage\.toml](/monad/source/formal/coverage-toml) lists them, and the [formal guide](/monad/formal) explains the strict check\.

The kernel checks only some properties of its types\. Read [their guarantees and limits](/monad/kernel#what-the-kernel-types-establish) before using them\. The successor profile, contract bundles, SDKs, integrations and GPU paths have no implementations\.

## Reuse comes before invention {#reuse-comes-before-invention}

Solve the stated problem by reuse, translation, composition, extension and, only when necessary, invention\. A complete reduction to existing mathematics is a successful outcome\. Compare explicit signatures and assumptions, not metaphors\. Classify a mismatch as a definition, a conjecture, a theorem target, or a counterexample\. Keep equivalence and identity apart\. Keep mathematical representation and operational access apart\. Keep inaccessible and destroyed information apart\. Do not hide unresolved structure in a score\.

The [formal guide](/monad/formal) holds the documentation, notation, and refactoring conventions\.

## MONAD is the root of the Causality research stack {#monad-is-the-root-of-the-causality-research-stack}

The research moves from MONAD's axioms to MUON's adaptive unit, Reckon's records and HENA's swarm\. The table distinguishes direct code dependencies from the formal definitions used by Reckon’s manual\.

| Project | Relationship |
| --- | --- |
| [MUON](https://causality.in/muon/) | Consumer: the single unit |
| [Reckon](https://causality.in/reckon/) | Formal definitions used by the laboratory manual; no current kernel dependency |
| [HENA](https://causality.in/hena/) | Consumer: swarm composition |
| [Q\-MUON](https://causality.in/q-muon/) | Consumer: quantum unit fork |
| [Q\-HENA](https://causality.in/q-hena/) | Consumer: quantum swarm fork |
| Other downstream research projects | Consumers; this manual gives no integration guide for them |

MONAD is part of Causality, the research brand of RawThoughts Enterprises Private Limited, at [causality\.in](https://causality.in)\.

## MONAD is licensed AGPL\-3\.0\-or\-later {#monad-is-licensed-agpl-3-0-or-later}

MONAD's kernel, Lean formalization and paper use [AGPL\-3\.0\-or\-later](/monad/source/LICENSE)\. The Lean formalization builds on the pinned Lean 4 toolchain and mathlib \(Apache\-2\.0\); those and other third\-party material keep their own terms and required notices\.

Commercial use is permitted under the applicable licence\. Alternative terms require a separate written agreement with RawThoughts Enterprises Private Limited, which can grant them only for material it is entitled to relicense; this repository grants none\. See [NOTICE](/monad/source/NOTICE) for ownership and the dependency statements, [CITATION\.cff](/monad/source/CITATION-cff) for citation and [CONTRIBUTING\.md](/monad/source/CONTRIBUTING-md) for contribution requirements\. Outside contributions remain closed pending review of the [draft agreement](/monad/source/CLA-md) and a way to record acceptance\.
