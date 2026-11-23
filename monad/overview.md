---
title: "MONAD"
---

<a id="monad"></a>

MONAD studies what a description can tell apart and which distinctions survive compression or a change of description\. Its Lean library defines and proves mathematical results\. A paper explains them, and a small Rust kernel provides types and numerical operations for other research code\.

Use the Rust kernel to compare finite observation laws from a local checkout\. Use Lean to follow a proof or extend the formal library\. [Descriptions and repair](https://causality.in/monad/descriptions-and-repair/) and its [four\-state worked example](https://causality.in/monad/what-a-summary-preserves/) explain the research without requiring a build\. The name is short for Minimal Ontological Notions for Axiomatic Distinction\. Category theory and programming languages use “monad” for a different concept\.

## MONAD has three surfaces {#monad-has-three-surfaces}

Lean owns the formal definitions and proofs\. Each mathematical module has a matching TeX chapter, so the paper explains the same objects and results\. The Rust kernel represents only part of a finite observer model\. Its type names and operations need the interpretation in [the kernel guide](/monad/kernel#what-the-kernel-types-establish)\.

MONAD runs in a caller’s process\. There is no daemon, CLI or service to start\.

## The mathematics is partly proved and nothing is published {#the-mathematics-is-partly-proved-and-nothing-is-published}

The Lean library and paper share 42 mathematical modules\. The correspondence checker links 82 displayed proofs to Lean declarations\. The [coverage register](/monad/source/formal/coverage-toml) lists open work on coding and local testing, quantum correspondence and review of the paper's statements\. Private calibration and further proving are paused\.

<Info>

**Build from a source checkout\.** No package is published\. The Lean library and paper are in `formal/` and `paper/`, and the wider SDKs and integrations are designs with no implementation\. The instructions below assume access to a local checkout\.

</Info>

## Read these pages next {#read-these-pages-next}

<CardGroup>

<Card href="&#47;monad&#47;kernel&#35;a&#45;first&#45;rust&#45;call" title="Use the Rust kernel" id="gazette-layout-0">

Create a consumer project and compare two finite probability distributions\.

</Card>

<Card href="&#47;monad&#47;mathematics&#35;read&#45;a&#45;preservation&#45;claim&#45;in&#45;lean" title="Read a theorem in Lean" id="gazette-layout-1">

Translate the four\-state example into definitions and inspect the preservation and quotient results\.

</Card>

<Card href="&#47;monad&#47;getting&#45;started" title="Prepare the toolchains" id="gazette-layout-2">

Build the kernel, Lean library or paper and understand what each check covers\.

</Card>

<Card href="&#47;monad&#47;formal&#35;contents" title="Find a formal definition" id="gazette-layout-3">

Look up the mathematical modules and the notation that connects Lean to the paper\.

</Card>

</CardGroup>

The [repository overview](/monad/repository) explains the dependency direction and licensing\. Return to [the product explanation](https://causality.in/monad/descriptions-and-repair/) for the broader research question\.

The [library design](/monad/library-design) explains the planned language interfaces, shared scene records and acceleration, and what must exist before they are implemented\.

Maintainers use `docs/stage-checks.md` in the source checkout to distinguish resolved dependencies from mathematical correspondence when a Causality candidate uses MONAD\.
