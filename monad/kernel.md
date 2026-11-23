---
title: "The kernel"
---

<a id="the&#45;kernel"></a>

`monad-kernel` provides Rust types for states, observers, resources and intervention records, plus numerical operations on finite probability distributions\. The crate has no external dependencies and runs inside the caller’s process\. It represents part of MONAD’s finite observer model; the Lean library defines the wider mathematics\.

## What the kernel types establish {#what-the-kernel-types-establish}

The kernel stores numerical observations and metadata\. Its types establish the following, with the stated limits:

- `DirectInfluenceRecords` stores observed direct pairs\. It does not check reflexivity or transitivity\. The paper proves that detectable influence is not transitive in general\.
- `ReliableRecord` holds metadata for the paper's reliable record\. Its method checks that the supplied error bound is finite and in `[0, 1/2)`\. It does not verify the decision rule, trials or persistence\.
- `ChannelCapacityNats` holds a finite value in nats per use that you supply\. Multiply a value in bits by `ln(2)` before passing it\. The Lean library defines capacity in nats as a noncomputable supremum over the declared admissible input laws\. That definition is not an executable capacity estimator\.
- `ResourceVector::fits_within` checks that the receiver's coordinates are less than or equal to the argument's, including finite nonnegative `epsilon` values\. The meaning and order of `epsilon` still need a profile; comparing fields establishes no inclusion of available protocols\.

`StateSpace::new` stores a regulator and dimension without validating nonzero values or state indices\. A stored cardinality bound does not construct dynamics, a measure or a proof that a consumer's states obey it\.

The traits `Evolution`, `CoarseGraining`, `RecordChannel` and `AllowedOps` have no implementations\. A consumer that implements them must establish the required laws\. `TotalVariation` computes a finite\-vector distance\. `ChernoffInformation` uses a grid approximation without a certified numerical error bound; its `error_probability_bound` method does not provide a certified finite\-sample guarantee\.

## Compare a type with its formal reference {#compare-a-type-with-its-formal-reference}

Each public type's Rust documentation names a Lean declaration to compare or explains why it has no counterpart\. The [formal guide](/monad/formal#kernel-references) describes the checked `Lean:` lines\. Running the correspondence checker resolves those names and audits their axioms; it does not prove that a numeric implementation satisfies the named definition or theorem\.

The tests in `kernel/tests/correspondence.rs` compare total variation with all events on small finite carriers, check the Chernoff grid against independent decimal reference values, and compute the three\-bit intervention model's cancelling effect\. The xor example uses the zero preparation and the intervention functions in `MonadFormal.model`, `MonadFormal.intA` and `MonadFormal.intB`\. It establishes behaviour for those inputs, not a general causal\-identification result\. The other kernel tests cover their own numerical domains and failure cases\.

## How to depend on it {#how-to-depend-on-it}

Research systems clone MONAD beside their own repository and declare a Cargo path dependency on MONAD's `kernel` folder\. The dependency name is `monad-kernel`\. The crate is not published to a registry\.

### A first Rust call {#a-first-rust-call}

You need Rust, Cargo and a local MONAD checkout\. Lean, Python and TeX are needed only for the other checks in [getting started](/monad/getting-started)\.

The checked\-in [first\-use example](/monad/source/kernel/examples/first_use-rs) contains the calculation below\. Run it from this repository with `make example-rust`, or follow the consumer setup to use the crate from a separate project\.

Find `MONAD/kernel/Cargo.toml` in your checkout\. Create `monad-first-use` beside the `MONAD` directory and create a `src` directory inside it\. The consumer needs the two files below\.

<Steps>

<Step title="Declare the dependency" id="gazette-layout-0">

Create `monad-first-use/Cargo.toml`:

```toml
[package]
name = "monad-first-use"
version = "0.1.0"
edition = "2021"

[dependencies]
monad-kernel = { path = "../MONAD/kernel" }
```

The path is relative to this manifest\. Change it if your MONAD checkout is elsewhere\. There is no registry release to use instead\.

</Step>

<Step title="Compare two distributions" id="gazette-layout-1">

Create `monad-first-use/src/main.rs`\. Both vectors describe the ordered outcomes `[record absent, record present]` under two hypothetical preparations\. The example supplies these laws; the kernel does not measure or infer them\.

```rust
use monad_kernel::causality::{
    CausalRelation, DetectabilityThreshold, Distinguishability, TotalVariation,
};

fn main() {
    let p = [0.8, 0.2];
    let q = [0.2, 0.8];
    let distance = TotalVariation.divergence(&p, &q);
    assert!((distance - 0.6).abs() < 1e-12);

    let observation = CausalRelation {
        cause: 0,
        effect: 1,
        distinguishability: distance,
        threshold: DetectabilityThreshold(0.9),
    };

    println!("total variation: {distance:.3}");
    println!("meets threshold: {}", observation.is_detected());
}
```

</Step>

<Step title="Run the calculation" id="gazette-layout-2">

From `monad-first-use`, run:

```sh
cargo run --offline
```

The result is:

```text
total variation: 0.600
meets threshold: false
```

</Step>

</Steps>

Total variation is half the sum of absolute coordinate differences: here `(0.6 + 0.6) / 2 = 0.6`\. Equivalently, it is the largest difference the two laws assign to the same event\. A threshold of `0.9` rejects this numerical observation; changing it to `0.5` makes the second line `true`\. Neither result establishes that the laws came from valid matched interventions or reliable records\. The event identifiers and threshold are metadata supplied by the caller\.

<Warning>

**Invalid probability vectors panic\.** Supply equal nonzero lengths, finite entries in `[0, 1]`, and a total within `1e-10` of one for each vector\. Keep the same outcome order in both\. Validate external data before calling the API; raw counts need conversion into a justified probability model first\.

</Warning>

If Cargo cannot find the kernel, check the dependency path relative to the consumer manifest\. The [kernel source](/monad/source/kernel/src/lib-rs) lists its module exports; [working on the mathematics](/monad/mathematics) explains the formal definitions and proof workflow\.

The [preservation\-under\-uncertainty explanation](https://causality.in/collections/changing-descriptions/preservation-under-uncertainty/) puts probabilistic comparison in the wider research question\. For exact answer preservation, return to [the summary example](https://causality.in/monad/what-a-summary-preserves/): its quotient and source\-dependent repair are separate from the distance calculation here\.

## The kernel changes only by addition within a stage {#the-kernel-changes-only-by-addition-within-a-stage}

- Within a stage, changes add types, implementations, methods, and tests\. They do not rename, remove, or change signatures\.
- Breaking changes happen only at a stage boundary\. Every consumer must compile again before that boundary closes\.
- A new dependency is a contract change\.
- New trait implementations arrive with law tests\. For example, an evolution must be bijective and measure\-preserving\.

## The protocol buffers are headers until a consumer needs a message {#the-protocol-buffers-are-headers-until-a-consumer-needs-a-message}

`protos/monad/` holds `state.proto`, `observer.proto`, and `causality.proto`\. Each file is a header with no messages\. Messages arrive when a cross\-language consumer needs them\. Each message will mirror a kernel type in name and shape\.
