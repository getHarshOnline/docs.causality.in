---
title: "kernel/Cargo.toml"
noindex: true
---

```toml
[package]
name = "q-hena-kernel"
version = "0.1.0"
edition = "2021"
description = "Q-HENA's quantum swarm, which adapts HENA's collective design to Q-MUON units"
license = "AGPL-3.0-or-later"

[dependencies]
monad-kernel = { git = "https://github.com/getHarshOnline/MONAD.git", rev = "9f5347382f27c12ac8676e62c4e3d54e81f3c212" }
q-muon-kernel = { git = "https://github.com/getHarshOnline/Q-MUON.git", rev = "c04c8d6039ed7321342f4d6a8f7607d2aed06b7f" }
reckon-kernel = { git = "https://github.com/getHarshOnline/reckon.git", rev = "6ecb7f64a47256ebe950bd1e7ad40ad5d0020cbf" }

[dev-dependencies]
# hena-kernel = { path = "../../HENA/kernel" }  # for parity testing only
```
