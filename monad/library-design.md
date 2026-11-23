---
title: "MONAD as a library"
---

<a id="monad&#45;as&#45;a&#45;library"></a>

MONAD lets a caller use a limited executable part of its mathematics inside its own process\. Research kernels already depend on `monad-kernel`; the wider language interfaces and visualisation tools described here are planned\. The [Lean library](/monad/formal) owns definitions and proofs, the paper explains them, and [the Rust kernel](/monad/kernel) supplies separately tested types and finite\-distribution calculations\.

## Two kinds of consumer {#two-kinds-of-consumer}

Research repositories use `kernel/` directly through a Cargo path dependency\. Changing that shared API requires a coordinated migration and a build of every consumer, as the [kernel guide](/monad/kernel#the-kernel-changes-only-by-addition-within-a-stage) explains\.

Other tools will use one Rust facade, `monad-sdk`, directly or through a language interface\. Each interface wraps or generates from that facade; it does not reimplement the mathematics\. MONAD has no daemon, command line or service\. A caller needing a service supplies its own adapter and decides who may use it\. Protobuf definitions describe wire data, not a server\.

## One Rust facade, several language interfaces {#one-rust-facade-several-language-interfaces}

| Home | Intended use |
| --- | --- |
| `sdk/rust/` | The complete, narrow Rust facade and its headless scene compiler, tested through the public API and conformance cases\. Computation is effect\-free\. |
| `sdk/python/` | A Rust extension through PyO3 and maturin, with generated Python type stubs, NumPy\-native projections and step traces\. It works with Manim without depending on it\. |
| `sdk/js/` | A native Node module through napi\-rs for services and tooling\. |
| `sdk/go/` | A pure\-Go decoder and validator for profile manifests and conformance records\. It does not reproduce the kernel's calculations\. |
| `sdk/wasm/` | The full effect\-free sandbox in the browser, avoiding a server round trip for its computations\. |
| `sdk/viz/` | React views of scene records\. |

Each SDK is intended to become an independent repository\. Its licence, notice and citation files arrive with its implementation\. Directory names and the proposed Rust facade name reserve no package registry coordinates\.

Framework integrations depend on those interfaces\. `integrations/torch/` supplies tensor wrappers over the Python SDK; `integrations/hf/` supplies model\-side wrappers over it\. `integrations/manim/` animates scene records using the Python SDK and an exactly pinned ManimCE release\. The SDKs do not depend on these integrations\.

## Pictures read the same scene records {#pictures-read-the-same-scene-records}

The Rust facade's headless compiler will emit typed scene records with stable cell identities and step traces for refinement, splits, merges and transport\. Manim animations, React views and browser computations use that shared representation\. Renderers read records; they originate no mathematical facts\. TikZ remains the source of the paper's figures, and exploratory notebooks in `notebooks/` illustrate results rather than establish them\.

The five picture primitives express the following mathematical content\. A scene must identify the definitions, assumptions or result it illustrates; an animation does not prove a programme\-level claim\.

| Primitive | What it shows |
| --- | --- |
| Class split | Generative openness: an equivalence cell divides when an extension admits a separating question\. |
| Quotient and refine | The exact quotient and monotonicity: merge states into the coarsest sufficient partition, then split cells when the admitted family grows\. |
| Transport flow | Invariant accounting: the preserved, hidden, collapsed and novel parts of one transformation\. |
| Path pair | Trajectory\-sensitive relevance: two paths with the same endpoint can carry different relevance\. |
| No\-go forcing | The compression obstruction: under the separating\-extension assumptions, each proposed merge fails until the summary is injective\. |

The scene contract must be available to its renderer before implementation\. Calibration renders are internal evidence; publishing them is a separate decision\. Causality's Manim animations, planned widgets and site imagery share this picture vocabulary\. A shared visual language keeps the illustrated relationships consistent; each use still identifies the mathematics it actually illustrates\.

## Portable contracts wait for a second language {#portable-contracts-wait-for-a-second-language}

Bundle and hash machinery is deferred until a second language needs it\. The intended arrangement is one canonical schema and a per\-symbol profile map, generated into a portable profile bundle\. A manifest in `contracts/manifests/` would identify the corresponding paper section, Lean declaration and kernel symbol, with their source references and hashes\. A private deterministic generator, proposed as `crates/monad-contract-codegen`, would write `contracts/bundle/<profile>/`; a proposed `crates/monad-contracts` crate would embed and validate it offline\.

Each SDK would embed the same bundle and reject a mismatched profile hash\. No SDK would copy a schema by hand\. These checks establish which material a caller has, not mathematical equivalence: correspondence still needs the stated hypotheses, Lean proofs, executable tests and semantic review\. Wire vocabulary stays in `protos/`\.

`contracts/conformance/` is the home for executable comparisons: direct pairs versus closure, cardinality, measure and entropy reference cases, and negative cases rejecting unsupported continuous or quantum interpretations\. A sealed FORC 0\.1 bundle would identify the historical profile, including its known divergences; it would not become the public API\. No such bundle or suite exists yet\.

## Measure a defined update rule before accelerating it {#measure-a-defined-update-rule-before-accelerating-it}

SIMD and GPU work starts only after a well\-posed compressed update rule exists with its invariant class declared\. `benchmarks/` measures error growth against horizon and cost against that class\. Every benchmark states whether it tests trajectory fidelity or invariant statistics; those are different accuracy promises\. `kernel/gpu/cuda/` and `kernel/gpu/mlx/` will hold the corresponding accelerated kernels\.

## What starts implementation and release {#what-starts-implementation-and-release}

The finite observer profile is accepted as 0\.2, with the detectability threshold and the interpretation of intervention influence still open\. Acceptance does not supply an implemented SDK, symbol map or conformance suite\. Those remain planned work; a second language's actual need comes before bundle machinery\. A defined scene contract comes before renderers, and the update\-rule condition above comes before acceleration\. These conditions do not resume the paused calibration or further proving\.

Package names and registry coordinates still need a decision and an availability check at that time\. Publication posture also remains a separate decision; the bill of materials and publication quickstart follow it\. The existing [notice](/monad/source/NOTICE) states current attribution, and the [contributing guide](/monad/source/CONTRIBUTING-md) and [contributor agreement](/monad/source/CLA-md) state the contribution terms\. This design specifies where future code belongs and what it must preserve; it does not announce a package or a release\.
