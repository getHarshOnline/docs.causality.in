---
title: "Reckon"
---

<a id="reckon"></a>

<a id="the&#45;reckon&#45;laboratory&#45;manual"></a>

Use this manual to build the `reckon` Python package, integrate its memory manager or send a run to the debugger\. The build gives you a Python extension backed by Rust\. Your task supplies the observations and outcomes\.

For an introduction and experiments you can follow from start to finish, use [Reckon’s product guides](https://causality.in/reckon/understand-the-memory-laboratory/)\.

## Build and integrate the laboratory {#build-and-integrate-the-laboratory}

<CardGroup>

<Card href="&#47;reckon&#47;installation" title="Build the Python extension" id="gazette-layout-0">

Build the wheel against your interpreter and verify the Rust import\.

</Card>

<Card href="&#47;reckon&#47;python&#47;overview&#35;connect&#45;retrieval&#45;to&#45;an&#45;outcome" title="Connect retrieval to an outcome" id="gazette-layout-1">

Wire writes, reads and an outcome into a task you own\.

</Card>

<Card href="&#47;reckon&#47;memory&#45;manager" title="Understand memory decisions" id="gazette-layout-2">

Find which limits apply when you write a record and how retrieval orders matches\.

</Card>

<Card href="&#47;reckon&#47;debugger" title="Supply debugger data" id="gazette-layout-3">

Supply the fields the timeline, graph and cost plot need\.

</Card>

</CardGroup>

Before you compare two runs, check which [task](/reckon/tasks-and-benchmarks) produced each [metric](/reckon/evaluation-metrics)\. Save the configuration, seeds, software versions and outputs together so someone can [repeat the run](/reckon/reproducibility)\. The [notebooks](/reckon/notebooks) show how those inputs reach a figure\.

If you came here to [compare replay budgets](https://causality.in/reckon/compare-replay-budgets/) or [inspect a saved run](https://causality.in/reckon/inspect-an-exported-run/), return to that task after installation\.

## If you want to extend it or review the ideas {#if-you-want-to-extend-it-or-review-the-ideas}

Read [the problem](/reckon/problem) and [framework](/reckon/framework) first\. The remaining chapters develop:

- [The mathematical core](/reckon/mathematical-core): resource\-indexed influence, its empirical estimate and retention under budgets\.
- [Operational causality](/reckon/operational-causality) and [entropy and coarse\-graining](/reckon/entropy-and-coarse-graining): the definitions Reckon takes from MONAD\.
- [Retrodiction bounds](/reckon/retrodiction): the assumptions limiting recovery of earlier states\.
- [Learning](/reckon/learning): training as an open reduction and representation geometry as an observable\.

Use the [glossary](/reckon/glossary), [FAQ](/reckon/faq) and [references](/reckon/references) for specific questions\.

The [planned evaluation contract](/reckon/evaluation-contract) specifies a future service for fixed experiments and signed results\. It is separate from the laboratory's working evaluation helpers\.

## Where the code is {#where-the-code-is}

| To change or understand | Read |
| --- | --- |
| System boundaries and data flow | [Architecture](/reckon/architecture) |
| Python integration | [Main API](/reckon/python/overview) |
| Resources, channels and relation diagnostics | [Core reference](/reckon/python/core) |
| Records, queries and retention | [Memory reference](/reckon/python/memory) |
| Task runners | [Task interface](/reckon/python/tasks) |
| Measurement helpers | [Metrics reference](/reckon/python/metrics) |
| Run storage and streaming | [Debugger server](/reckon/debugger/server) |
| Rust storage, logging and Python bindings | [Kernel](/reckon/kernel) |
| Browser application and reusable views | [Debugger interface](/reckon/debugger/interface) |
| Test execution and new cases | [Run and extend the tests](/reckon/contributing/tests) |

<Warning>

**Keep the daemon on a trusted local machine\.** It accepts loopback connections and uses plaintext without caller authentication\. Its [record\-only export](/reckon/daemon) returns the records a named run wrote or retrieved; event, metric and archive attachments remain unimplemented\. This is separate from a complete debugger run bundle\. See the [security policy](/reckon/security)\.

</Warning>
