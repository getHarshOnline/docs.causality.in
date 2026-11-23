---
title: "Building"
---

<a id="building"></a>

With source access, check that the HENA crate compiles or build the draft paper\. The kernel needs Reckon as well as MONAD and MUON\. The paper carries copies of those definitions in `paper/shared/`, so it builds alone\.

## Check the kernel {#check-the-kernel}

<Steps>

<Step title="Fetch the pinned dependencies" id="gazette-layout-0">

The [kernel manifest](/hena/assets/kernel/Cargo.toml) pins `MONAD`, `MUON` and `reckon` to one commit each of their GitHub repositories\. Cargo fetches them on the first check and reuses its copies afterwards, so the first check needs network access\. Reckon's repository name is lowercase `reckon`\.

HENA uses Reckon's default Rust API\. Its optional Python binding stays disabled, so this check needs no Python environment, Reckon wheel, debugger or notebook packages\.

</Step>

<Step title="Provide the build tools" id="gazette-layout-1">

Use Rust and Cargo\. Reckon's build supplies a pinned Protocol Buffers compiler through Cargo, so a separate `protoc` installation is optional\. Set `PROTOC` to an executable path when selecting a compiler explicitly; `PROTOC_INCLUDE` can select its include directory\. Offline builds need the packaged compiler dependencies in Cargo's cache too\.

</Step>

<Step title="Compile the dependency graph" id="gazette-layout-2">

From HENA's repository root, run:

```sh
cargo check --manifest-path kernel/Cargo.toml
```

A successful command compiles `hena-kernel` and its three dependency re\-exports\. If it fails in a dependency crate, resolve that dependency's error before retrying HENA\.

</Step>

</Steps>

<Info>

The current crate contains no scheduler, message routing or swarm store\. This check does not run units or demonstrate collective behaviour\.

</Info>

## Build the paper {#build-the-paper}

Install Make, Bash and a TeX distribution with `pdflatex` and `bibtex`\. Use `pdftotext` to inspect the rendered text as well as opening the PDF\.

From HENA's repository root:

```sh
cd paper
make all
```

The output is `paper/HENA.pdf`\. The main source imports the MONAD, MUON and HENA definitions in `paper/shared/`, so the paper builds without the other repositories\.

The [problem statement](/hena/source/paper/sections/00_problem_statement-tex) presents the proposed differentiable collective, its equations, resource targets and comparison requirements\. Read the generated PDF and review the build warnings; successful typesetting establishes neither the proposal’s performance nor its feasibility\.

Return to the [overview](/hena/overview)\.
