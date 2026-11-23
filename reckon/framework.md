---
title: "Framework overview"
---

<a id="framework&#45;overview"></a>

<a id="framework&#45;overview&#45;arrow&#45;neutral&#45;core&#45;operational&#45;causality&#45;memory&#45;manager"></a>

Reckon uses a research model to ask which observations an agent can keep and test under resource limits\. Start here to see how those questions relate to the memory manager, evaluation code and debugger\.

## Layers {#layers}

The model distinguishes the underlying dynamics, the observations an agent can make and the conclusions it can draw from them\.

### Layer 1: Microdynamics \(arrow\-neutral\) {#layer-1-microdynamics-arrow-neutral}

Assume closed evolution is reversible and measure\-preserving \(classical\) or unitary \(quantum\)\. Fine\-grained entropy is invariant here\. No arrow is assumed in the micro\-law\.

### Layer 2: Observer interface \(where arrows enter\) {#layer-2-observer-interface-where-arrows-enter}

For scale s and resources R, the interface has three parts\. The coarse\-graining Π\_s determines what the agent observes\. The record channel C\_R determines what it can store and read back under noise\. The allowed operations Op\_R determine what it can change in an experiment\.

Thermodynamic arrows arise only after:

- choosing Π\_s and resolution,
- adding boundary conditions \(e\.g\., Past Hypothesis\),
- invoking typicality arguments\.

### Layer 3: Operational causality and retrodiction {#layer-3-operational-causality-and-retrodiction}

Use [operational causality](/reckon/operational-causality) to ask whether an intervention changes what can be reliably observed under R\. The resulting relation can fail reflexivity and transitivity\. Use [retrodiction](/reckon/retrodiction) to ask whether you can recover an earlier state from later records, given what the observation map preserves, how inverse errors grow and how noisy the records are\.

## The engineering translation {#the-engineering-translation}

The memory manager stores typed records and decides when to remove them\. Evaluation helpers summarize outcomes and test supplied comparisons\. The debugger shows activity from a run you saved\.

<Note>

**Abstract interfaces do not establish the model\.** The [channel interfaces](/reckon/python/core#resource-and-channel-boundaries) are abstract classes, not concrete implementations of mathematical coarse\-graining or reliable recording\. The [memory manager](/reckon/memory-manager) explains current budget enforcement and retrieval\. The [evaluation protocol](/reckon/evaluation-metrics#evaluation-protocol) describes the intervention\-timing limits; the [debugger guide](/reckon/debugger) distinguishes recorded operations from snapshots and measured contribution evidence\.

</Note>

## Implementation {#implementation}

| Component | Design Doc | Implementation |
| --- | --- | --- |
| Core abstractions | [03\-Math\-Core\.md](/reckon/mathematical-core) | [core/README\.md](/reckon/python/core) |
| Causal Memory Manager | [08\-Causal\-Memory\-Manager\-Design\.md](/reckon/memory-manager) | [memory/README\.md](/reckon/python/memory) |
| Debugger | [10\-Debugger\-Visualization\.md](/reckon/debugger) | [viz/README\.md](/reckon/debugger/server), [viz\-ui/README\.md](/reckon/debugger/interface) |
| Performance layer | none | [kernel/README\.md](/reckon/kernel) |
