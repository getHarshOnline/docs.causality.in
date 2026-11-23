---
title: "kernel/src/lib.rs"
noindex: true
---

```rs
//! The quantum swarm of Causality's research stack: Q-HENA hosts would route, communicate
//! and keep memory, adapting HENA's collective design to Q-MUON units.
//!
//! Two questions are open before a quantum swarm can claim anything: nothing yet shows a
//! quantum advantage for a swarm, and HENA's classical swarm has still to show that it differs
//! from neighbouring multi-agent systems. Q-HENA claims neither. This crate builds on MONAD's
//! formal types, Q-MUON's units and Reckon's record protocol for the swarm store its host would
//! own, and re-exports them today; mirroring a fixed HENA candidate is the work in front of it.

pub use monad_kernel;
pub use q_muon_kernel;
pub use reckon_kernel;
```
