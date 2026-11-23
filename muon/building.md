---
title: "Building"
---

<a id="building"></a>

Compile MUON’s `muon-kernel` crate or build its draft paper locally\. The kernel uses MONAD’s crate, which Cargo fetches from MONAD’s repository at the commit the [kernel manifest](/muon/assets/kernel/Cargo.toml) pins\. The paper builds from its own copies of MONAD’s and MUON’s definitions in `paper/shared/`, so neither build needs another checkout\.

## Check the kernel {#check-the-kernel}

Use Rust and Cargo\. From the MUON repository root:

1. Make sure Cargo can reach GitHub\. The first check fetches `monad-kernel` at the pinned commit; later checks reuse Cargo’s copy, and `--offline` works once it is cached\.

2. Compile the kernel crate:
   
   ```bash
   cargo check --manifest-path kernel/Cargo.toml
   ```
   
   Cargo should finish the `dev` profile after checking `monad-kernel` and `muon-kernel`\. The latter re\-exports the former\. The command checks that the two crates compile together\.

## Build the paper {#build-the-paper}

Install Make, Bash and a TeX distribution providing `pdflatex` and `bibtex`\. Use `pdftotext` to inspect the rendered text as well as opening the PDF\.

From MUON’s repository root, run:

```bash
make -C paper all
```

Open `paper/MUON.pdf` and check the text\. The master imports MUON’s definitions and MONAD’s\. If TeX reports a missing definitions file, check that `paper/shared/monad/definitions.tex` exists\. This build uses only those two sets of shared definitions\.

The [research statement](/muon/source/paper/sections/00_problem_statement-tex) defines the proposed unit and its comparisons; it reports no working implementation or experiment\. A PDF build checks typesetting, not the proposed unit’s behavior\. Review missing\-reference warnings and check that the rendered definitions and mathematical symbols are readable\.

Return to the [unit interface](/muon/overview) to understand what a future implementation would need to provide\.
