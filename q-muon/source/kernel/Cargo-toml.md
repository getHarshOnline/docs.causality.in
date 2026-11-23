---
title: "kernel/Cargo.toml"
noindex: true
---

```toml
[package]
name = "q-muon-kernel"
version = "0.1.0"
edition = "2021"
description = "Q-MUON's quantum learning unit, behind MUON's common interface for Q-HENA swarms"
license = "AGPL-3.0-or-later"

[dependencies]
monad-kernel = { git = "https://github.com/getHarshOnline/MONAD.git", rev = "9f5347382f27c12ac8676e62c4e3d54e81f3c212" }

[dev-dependencies]
# muon-kernel = { path = "../../MUON/kernel" }  # for parity testing only
```
