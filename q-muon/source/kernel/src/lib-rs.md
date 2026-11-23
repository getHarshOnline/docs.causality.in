---
title: "kernel/src/lib.rs"
noindex: true
---

```rs
//! Quantum learning units of Causality's research stack, behind MUON's common interface so
//! that Q-HENA swarms can compose them.
//!
//! A quantum unit raises two separate questions: whether it learns anything useful, and whether
//! it does so better than an adequate classical unit at the same total cost. Neither is shown
//! for Q-MUON, and each capability has to be shown on its own before it is claimed. This crate
//! builds on MONAD's formal types and keeps MUON's unit boundary, with quantum gates,
//! entanglement and communication inside each unit. It re-exports MONAD today; mirroring a
//! fixed classical MUON candidate is the work in front of it.

pub use monad_kernel;
```
