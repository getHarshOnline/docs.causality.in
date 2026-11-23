---
title: "kernel/examples/first_use.rs"
noindex: true
---

```rs
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
