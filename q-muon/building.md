---
title: "Building"
---

<a id="building"></a>

Run these commands from the repository root\. The kernel manifest pins MONAD to one commit, so every checkout compiles against the same MONAD source\.

## Check the kernel crate {#check-the-kernel-crate}

Install Rust and Cargo\. The [kernel manifest](/q-muon/source/kernel/Cargo-toml) selects [MONAD](https://causality.in/monad/)’s kernel as the only dependency, which Cargo fetches from MONAD’s repository at the pinned commit; the [crate root](/q-muon/source/kernel/src/lib-rs) re\-exports its types\.

1. Compile the kernel:
   
   ```bash
   cargo check --manifest-path kernel/Cargo.toml
   ```

2. Read the result\. A successful check means the kernel crate compiles with the pinned MONAD kernel\. There is no quantum\-unit behavior to exercise yet\.

If all Cargo dependencies are already cached, use `cargo check --offline --manifest-path kernel/Cargo.toml` to check without network access\.

## Build the paper {#build-the-paper}

The paper imports MONAD’s, MUON’s and Q\-MUON’s definitions from its own `paper/shared/` folder, so it builds without the other repositories\.

Install Make, Bash and a TeX distribution with `pdflatex` and `bibtex`\. Install `pdftotext` for the rendered\-text check; without it, the validator reports that check as skipped\.

1. Typeset the paper:
   
   ```bash
   (cd paper && make all && ./validate.sh)
   ```

2. Inspect the output\. The paper contains an abstract\. Open the PDF and review the validator’s warnings and skipped checks; a zero exit does not mean every check ran\.

These commands check that the kernel crate and the paper's source build\. They do not test a quantum implementation or an advantage over a classical unit\.

Return to the [developer overview](/q-muon/overview)\.
