---
title: "Building"
---

<a id="building"></a>

Run the commands from the repository root\. The kernel manifest pins each dependency to one commit, so every result compiles against the same sources\.

## Check the kernel crate {#check-the-kernel-crate}

This task needs Rust and Cargo\. The [kernel manifest](/q-hena/source/kernel/Cargo-toml) declares MONAD, Q\-MUON and Reckon as git dependencies pinned to one commit each, which Cargo fetches on the first check; the [crate root](/q-hena/source/kernel/src/lib-rs) re\-exports them\. HENA supplies the classical work that Q\-HENA takes in by merge at each stage, not a current Cargo dependency\.

Reckon's kernel build supplies its pinned Protocol Buffers compiler through Cargo\. A separate `protoc` installation is optional; `PROTOC` selects another executable and `PROTOC_INCLUDE` can select its include directory\. Q\-HENA uses Reckon's default Rust API with its Python binding disabled; this check needs no Python interpreter\.

```bash
cargo check --manifest-path kernel/Cargo.toml
```

Success means the kernel crate's dependency re\-exports compile\. The command does not exercise a quantum swarm, store or recovery mechanism\.

If all Cargo dependencies are already cached, use `cargo check --offline --manifest-path kernel/Cargo.toml` to check without network access\. For Q\-HENA this includes Reckon’s packaged Protocol Buffers compiler dependencies\.

## Build the paper {#build-the-paper}

The paper imports MONAD's and Q\-MUON's definitions plus Q\-HENA's own\. The shared build needs Make, Bash and a TeX distribution with `pdflatex` and `bibtex`\. Install `pdftotext` for the rendered\-text check; the validator reports a skip when it is unavailable\.

```bash
(cd paper && make all && ./validate.sh)
```

The current paper contains an abstract\. It uses `\QuantumSwarmRouting` for the swarm expression and `\QuantumRouting` for Q\-MUON's routing between units\. Review the PDF and the validator’s warnings and skipped checks after a successful build\. Check for missing references and verify that definitions and mathematical symbols remain readable in the rendered text\. Typesetting supplies no experimental result about emergence, recovery or quantum advantage\.

Return to the [developer overview](/q-hena/overview)\.
