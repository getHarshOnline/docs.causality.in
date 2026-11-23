---
title: "Learning as a special case"
---

<a id="learning&#45;as&#45;a&#45;special&#45;case"></a>

<a id="learning&#45;optimization&#45;as&#45;a&#45;special&#45;case&#45;no&#45;double&#45;standards"></a>

When two training runs end with similar parameters, those parameters alone can hide different histories\. This chapter explains what to log for a reconstruction, what a budgeted learner can store and how to interpret a spectral diagnostic\.

## 1\. Training is an Open Reduction {#1-training-is-an-open-reduction}

Let the full microstate be $z = (\theta, \xi)$:

- $\theta$: model parameters
- $\xi$: minibatch order, RNG state, data pipeline state, optimizer buffers, hardware nondeterminism, etc\.

What we observe as \"training on $\theta$\" is a coarse\-grained/open projection:

$$
\Pi(\theta, \xi) = \theta
$$

Thus, apparent irreversibility in training is expected: many\-to\-one projection merges histories\.

## 2\. Memory as Reliable Records in Learning Loops {#2-memory-as-reliable-records-in-learning-loops}

A \"record\" may be:

- a gradient summary
- a training example kept for replay
- a compressed representation snapshot
- a reasoning trace
- an environment cue

If you can keep only some records, compare how the task behaves when you change one of them under the same test conditions\. A [reliable record](/reckon/operational-causality) also needs to let you distinguish the alternatives within the allowed trials and error rates\.

## 3\. Representation Geometry as a Macro\-Observable {#3-representation-geometry-as-a-macro-observable}

Given embeddings $h_\theta^{(\ell)}(x_i)$, construct a similarity graph and Laplacian $L^{(\ell)}(\theta)$\. Spectral statistics are coarse observables of representation geometry\.

For an undirected graph with non\-negative weights, form a symmetric adjacency matrix\. Decide whether to use a normalized or unnormalized Laplacian\. Define spectral entropy from the positive eigenvalues $\lambda_i$:

$$
p_i = \frac{\lambda_i}{\sum_{j:\, \lambda_j > 0} \lambda_j}, \quad S_{\text{spec}} = -\sum_{i:\, \lambda_i > 0} p_i \log p_i
$$

This is a deliberately chosen summary of the graph, a macro\-functional, rather than a unique entropy of the representation\. The similarity rule, edge weights and Laplacian choice determine which quantity it measures\. It is separate from the [fine\-grained entropy conserved in MONAD’s formal profile](/reckon/entropy-and-coarse-graining); that conservation does not impose a direction of change on this statistic\.

**Notes**:

- The ideal formula excludes zero modes\. The implementation uses eigenvalues greater than `epsilon`\. The normalized Laplacian gives an isolated node a diagonal entry of 1\. In the unnormalized Laplacian it contributes a zero mode\.
- Log base determines units \(bits for $\log_2$, nats for $\ln$\)\. Unless stated otherwise, we use natural log; implementations MAY use $\log_2$: comparisons are invariant up to a constant factor\.
- The implementation normalizes those retained eigenvalues, then computes the log of each probability plus `epsilon`\. This is a numerical approximation to the displayed formula\.

If you use the statistic to investigate collapse, drift or a phase transition, report how you built the graph and what you compared\. The same change in spectral entropy can have several explanations\.

**Implementation**: [`compute_spectral_entropy(adjacency, normalized=False, log_base="natural")`](/reckon/source/src/reckon/metrics/spectral-py) accepts the supplied adjacency matrix and defaults to the unnormalized Laplacian\. The caller must supply the symmetric graph required by this interpretation\. [`spectral_entropy_from_edges()`](/reckon/source/src/reckon/metrics/spectral-py) builds an unweighted undirected projection: each supplied edge fills both adjacency directions\. It therefore discards direction in a record\-influence graph\. Neither function constructs an embedding similarity graph for the caller\. Units are nats by default; use `log_base="2"` for bits\.

<Danger>

**Spectral entropy can be expensive\.** This is an **opt\-in** metric \(O\(n³\) eigenvalue decomposition\)\. Compute only when explicitly requested; do not include in default metric collection for large graphs\.

</Danger>

## 4\. Retrodiction Lens on Training {#4-retrodiction-lens-on-training}

To reconstruct early $\theta_0$ from late $\theta_\tau$, remember that $\Pi$ maps several histories to the same parameters\. Missing $\xi$ acts as effective noise\. Logging $\xi$ can reduce uncertainty at a storage and compute cost\. Apply the [retrodiction estimate](/reckon/retrodiction) only if you can identify the target and describe how errors grow\. A property of the parameters can be identifiable even when the full training state is not\.

## Implementation {#implementation}

- [Memory Module README](/reckon/python/memory): `MemoryRecord` types \(gradient summary, exemplar, trace, representation\)
- [Tasks Module README](/reckon/python/tasks): Benchmark tasks that exercise memory under budget constraints
