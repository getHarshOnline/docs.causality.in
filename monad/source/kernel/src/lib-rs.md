---
title: "kernel/src/lib.rs"
noindex: true
---

```rs
//! monad-kernel: foundational formal objects for the Causality research stack.
//!
//! Research systems that compare descriptions need one typed vocabulary for state spaces,
//! observers, resources and records, so their results mean the same thing. This crate supplies
//! that vocabulary and the finite-distribution calculations implemented so far. MUON and HENA
//! import this kernel. Reckon's manual uses MONAD's formal
//! definitions; Reckon has no current dependency on this kernel.

pub mod causality;
pub mod information;
pub mod observer;
pub mod state_space;
```
