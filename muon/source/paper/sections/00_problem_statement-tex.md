---
title: "paper/sections/00_problem_statement.tex"
noindex: true
---

```tex
\section{The research problem}

Can a reasoning system use small learning units through one stable interface, improve their internal representations, and replace them without rebuilding its consumers? MUON separates the engineering question of a common interface from the empirical question of whether many inexpensive units can do useful work more effectively than a stronger model. The micro-transformer unit is the long-term design to investigate; a swarm's advantage is a hypothesis, not a requirement imposed on the result.

The central object is a \emph{slot}: the contract through which a caller uses a unit. Stock open-weight models fill it first, through one integration adapter per backend family. Those adapters remain as controls, reference implementations and fallbacks when a new unit arrives. An adapter standardises access; it does not give a model capabilities it lacks. Offline fine-tuning and learning during operation are separate changes, neither established by building the adapter.

The present repository contains a kernel that re-exports MONAD's types. It has no slot runtime, model adapter or micro-transformer implementation. The contract is a version-0 design. This paper states the questions and comparisons for future work; it reports no trained model, benchmark result or accepted replacement.

\section{What the slot must promise}

The proposed contract has five commitments:
\begin{enumerate}
\item Infer from an explicit request, with declared sampling controls, streaming support and seed behaviour.
\item Expose state at a declared fidelity: an opaque checkpoint, a summary or typed internal state. A summary must not masquerade as access to internal representations.
\item Emit ordered records for a store maintained by the unit's owner. At minimum these identify the inference event, input and output digests, resource use and episode, step and sequence.
\item Refuse work without an admissible request and a matching live resource reservation. Silently truncating or degrading the request would change the work being accepted.
\item Report exactly which unit is running and what it supports, so selection depends on capabilities rather than a model name.
\end{enumerate}

These commitments are served by inference, state, observer, budget and lifecycle calls, with a capability report alongside them. Initialisation, caller-driven steps, checkpoint and restore, and shutdown must preserve the contract. Checkpoint fidelity and observed determinism must be explicit. The report binds the model revision, tokenizer, quantisation, runtime, device, supported operations and state fidelity. Changing an identity-defining field changes the unit's identity; currently free capacity belongs to the allocator instead.

Admission and reservation answer different questions. The caller supplies requirements, an estimator predicts demand, the unit states its capacity and guarantees, and the allocator reserves what is available. Two requests might each fit a snapshot while their combined demand does not. Only an atomic reservation prevents them from spending the same capacity. A stale, expired or mismatched reservation must therefore cause refusal. An error tolerance is a quality requirement, not a consumable quantity to reserve.

Three claims require separate evidence. \emph{Conformance} establishes the callable surfaces, accurate identity and capability reports, ordered records, honest refusal and checkpoint recovery. \emph{Task adequacy} establishes that a unit meets a minimum performance fixed in advance. \emph{Substitution} establishes that it can replace another under named tasks, observations, resource limits and tolerances. A shared interface alone proves neither equal behaviour nor preserved meaning.

\section{The proposed learning unit}

The micro-transformer unit would compute from its own state and inputs. HENA would route sparse messages between units. Memory would be distributed across units and the stores their owners maintain, with observations and retention expressed through Reckon's record semantics. The unit emits records and may supply scores; the store's owner decides what is kept. The design has no central coordinating hub and installs no separate production memory system inside each unit.

Selective state-space processing and routed experts are candidate ingredients, not a selected architecture. Sparse autoencoders remain instruments for examining representations, rather than a mandatory component of the unit. A small unit is expected to be weaker than the small open models of its day. The task's capability floor must therefore be measured before apparent savings from reducing size can count as an advantage.

Surprise-driven learning supplies three questions: whether a bounded signal can usefully control learning rate, help score records for retention, or propose changes to sparse structure. Each raw component needs declared units, finite-value checks and a pinned calibration into $[0,1]$. Non-negative weights summing to one can then combine calibrated components on the same bounded scale. Raw gradient magnitudes and distribution divergences cannot simply be added as if their scales were interchangeable.

An admitted learning-rate controller must stay between declared positive finite limits. A keep score must also be finite and bounded, with its profile and threshold recoverable during replay. Structural search may emit versioned hypotheses; it cannot change authoritative structure on the strength of surprise alone. Each mechanism needs ablations, held-out tasks, negative controls, thresholds fixed in advance and rollback to the last admitted profile.

A surprising residual may reflect noise, an inadequate model, drift or insufficient resources. It does not establish a missing causal variable. A structural claim requires separate interventional or quasi-experimental evidence, negative controls and compatibility with MONAD's account of what can be distinguished. Rejection or an inconclusive comparison leaves the structure unchanged. Any adaptive state must remain bounded, versioned, checkpointed and auditable behind the unchanged slot; it cannot authorise an external action or activate its own successor.

\section{What a combined design must show}

A unit that combines temporal processing with routed experts must give the temporal output a direct path to predictions, not only influence over which expert is selected. A candidate must compare that direct output path with a routing-only design and account for the cost of both. A neural coordinating graph and Mamba experts are possible directions; neither is selected. Readable routes, sparse features and predictive information transfer likewise do not establish causal identification.

\section{Learning across two cores}

The cognitive-core question connects a small learning unit with experience accumulated by different people on different machines: what could two separately learning cores share, and how might their weights combine? Averaging and model soups are candidate methods; neither is evidence that a merge preserves either person's learning.

The experiment would first establish compatible checkpoints and training histories and identify what each core had learned. A candidate combination must be compared with both unchanged cores for retained capability, interference, privacy and resource cost. Each owner separately permits any use of records, derived training data or weights. Compatibility or a shared household does not supply that permission. Keeping the cores separate, declining a transfer, rejecting a harmful merge and returning to either baseline must remain possible.

This question neither makes MUON an operational judgment service nor selects it for one. A future consumer must define its own supported interface, comparison, admission and recovery requirements. Permission to process someone's work does not establish permission to train on it.

\section{The comparison that decides the thesis}

One evaluation manifest, fixed before the run, must compare four arms on the same permitted tasks and splits:
\begin{enumerate}
\item many identical units;
\item one stronger unit;
\item a mixture of admitted units;
\item a control with no swarm.
\end{enumerate}

The manifest must specify the no-swarm control and its distinction from the stronger-unit arm, rather than treating an unspecified baseline as evidence. Match total compute, latency class, energy, memory, tokens and communication under the declared resource comparison. Include routing, coordination, record handling and retained state in the costs. Record exact model, runtime and hardware identities, seeds, repetitions, quality, reliability and uncertainty, with thresholds for winning, not losing and stopping set in advance.

Every arm must meet the task's capability floor. A cheaper failure is not an adequate solution, and counting active parameters alone cannot establish lower end-to-end cost. Report interface conformance, adequacy and substitution separately even when all arise from the same run.

If the many-unit arm falls below the floor or loses once coordination costs count, retain the strongest single or mixed filler behind the unchanged slot and record the negative result. The stack can keep a useful interface without retaining the many-unit thesis. Stock adapters remain available for comparison and recovery.

\section{Responsibilities}

MONAD supplies the vocabulary for state spaces, coarse-grainings, record channels and resources. Its stored direct-influence pairs are not a proven ordering, and the meaning and order of the legacy resource coordinate \texttt{epsilon} remain unsettled. A vector comparison is therefore not permission to execute a request. The first stock implementation can proceed on today's kernel types independently of MONAD's paused mathematical research; later innovations and stage acceptance retain their own evidence requirements.

MUON owns one unit and its internal learning. HENA owns composition, routing, population dynamics and emergence, and supplies the swarm arm of the comparison. Consumers use the slot and capability report, with no model-specific branch into a filler's internals. Once the contract hardens, changes within a stage must be additive, and breaking changes must wait for a stage boundary. Classical improvements can pass to Q-MUON through fork and merge without silently changing the interface's meaning.

The immediate work is to implement the stock interface and its conformance checks, then establish adequate baselines. The micro-transformer design, surprise mechanisms, two-core experiment and matched comparisons remain proposed work. Finishing this statement opens none of those experiments and establishes no operational replacement.
```
