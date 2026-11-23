---
title: "Glossary"
---

<a id="glossary"></a>

Look up the terms used in the memory API, the mathematical chapters and task results\. For a field from a saved run, start with the metric’s producer to find how it was computed\.

## Core Terms {#core-terms}

**Arrow\-neutral**: The microscopic law assumes no preferred direction of time\. In MONAD’s model, the fine\-grained entropy is invariant under the chosen evolution and so supplies no direction\.

**Causal Memory Manager \(CMM\)**: The core component of the laboratory that manages record storage, retention, and retrieval under resource budgets\.

**Resource\-indexed influence \($\preceq_R$\)**: A resource\-indexed relation where $A \preceq_R B$ means varying $A$ produces detectable changes in reliable records of $B$ under allowed protocols\. It is neither reflexive nor transitive in general; `DirectInfluenceRecords` stores observations of its pairs\.

**Distinguishability threshold \($\eta$\)**: The minimum divergence required to declare $A \preceq_R B$\. Resolving a smaller effect may require more observations; the cost depends on the experiment, noise model and required error rates\. Changing a stored threshold alone does not carry out that measurement\.

**Divergence $D(\cdot \| \cdot)$**: A measure of distinguishability between distributions\. Allowed choices include KL divergence, total variation, and Hellinger distance\. See [Math\-Core Section 5](/reckon/mathematical-core)\.

**Coarse\-graining \($\Pi_s$\)**: A map from microstates to macrostates at scale $s$, determining which distinctions are observed\. MONAD’s formal profile uses a deterministic measurable map, followed by a separate noisy record channel; the abstract laboratory interface does not supply a stochastic generalization\.

**Fine\-grained entropy**: Entropy of the full microstate distribution\. It is invariant under invertible evolution that preserves the reference measure\.

**Macro\-entropy**: Entropy computed relative to a chosen coarse\-graining; can increase even when fine\-grained entropy is constant\.

**MemoryRecord**: A typed object storing payload, cost, tags, creation metadata and utility estimates\.

**Operational causality**: Causality defined by what an agent can reliably record and influence under its resource constraints\.

**Mutual influence**: A and B influence each other when $A \preceq_R B$ and $B \preceq_R A$\. It is a symmetric relation, not an equivalence, because influence is neither reflexive nor transitive in general\.

**Allowed interventions** \($\text{Op}_R$\): The set of physically implementable preparations/controls under resource constraints $R$\. See [Operational Causality](/reckon/operational-causality)\.

**Protocol class**: The set of allowed experimental procedures \(preparation, evolution, coarse\-graining, recording\) under given resources\.

**Record channel** \($\mathcal{C}_R$\): A noisy storage/measurement channel mapping observations to stored records under resources $R$\.

**Resource vector \($R$\)**: The multi\-dimensional budget an agent operates under: memory slots, tokens, bytes, bandwidth, compute, noise floor\.

**Retrodiction**: Inferring a past state or property from current records\. The answer depends on what the observations can distinguish, how errors grow and how noisy the records are\.

**Reliable record**: A record that lets you distinguish alternatives within the allowed trials and error rates\. A record can persist without passing that test\.

## Benchmark Terms {#benchmark-terms}

**Causal precision**: A proposed evaluation target: the fraction of stored records meeting the declared positive\-usefulness intervention test\. The working evaluator instead reports `contract_precision` over distinct evaluated target labels, counting only beneficial significant effects\. Its two\-sided `is_significant` flag also admits harmful effects, so it is not the usefulness numerator\. See [Evaluation Metrics](/reckon/evaluation-metrics#evaluation-protocol)\.

**Causal recall**: The target fraction of ground\-truth necessary records retained\. The record\-summary helper names this supplied\-label calculation `necessary_cue_retention`\. The evaluator's `contract_recall` asks a different question: what fraction of supplied ground\-truth target labels had beneficial significant effects? Its recall is unavailable without a non\-empty label set\. [Evaluation Metrics](/reckon/evaluation-metrics) defines both denominators\.

**Useful memory rate**: The proposed fraction of stored items retrieved and shown to affect a decision\. Current activity ratios have narrower names: `useful_reads_per_read` uses supplied useful reads, and Key\-Door's `successful_key_episodes_per_write` uses useful writes\. Neither denominator is distinct stored items, and neither calculation performs an ablation\. See [Evaluation Metrics](/reckon/evaluation-metrics#causal-record-contract-v0)\.

<Note>

**Read historical fields through their producer\.** Earlier saved results used these broad names for several different quantities\. Interpret them through their captured producer and source revision; they are not aliases for the current fields\.

</Note>

## Implementation References {#implementation-references}

For code implementations of these concepts, see:

- [Core Module README](/reckon/python/core): `ResourceVector`, `MemoryBudget`, `CoarseGraining`, `RecordChannel`, `DirectInfluenceRecords`
- [Memory Module README](/reckon/python/memory): `CausalMemoryManager`, `MemoryRecord`, retention policies
- [Metrics Module README](/reckon/python/metrics): task outcomes, memory activity, record summaries and intervention results
