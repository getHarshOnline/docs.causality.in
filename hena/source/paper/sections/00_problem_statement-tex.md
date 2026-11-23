---
title: "paper/sections/00_problem_statement.tex"
noindex: true
---

```tex
\section{Problem statement}

HENA, the Hierarchically Evolving Neural Automata, asks whether many small reasoning units can learn useful collective computation through sparse communication and limited memory. Four questions belong together: can cognition emerge from local units, can learned routing replace full attention, can bounded distributed memory replace a central context window, and can useful hierarchy emerge from interactions rather than be imposed in advance? A useful answer must improve the task outcome or its cost against simpler alternatives under a declared comparison.

The intended differentiable system would train routes, memory access and unit updates together. Its first operational baseline instead orchestrates stock open-weight models through MUON's unit interface. That baseline can establish a working collective and expose coordination failures; it does not by itself establish differentiable memory, learned communication or emergence. The repository currently provides dependency re-exports, not an implemented swarm. The equations below specify research targets and leave their mechanisms open.

\subsection{One idea at two scales}

The same inquiry concerns routing inside a model and coordination across a swarm of agents. Within a model, local computation would replace attention to the whole input with learned routes and memory access. At the collective scale, HENA asks how a hierarchical organisation might form and evolve from intent. This is one research direction at two scales; a result at either scale must be tested before it is carried to the other.

Pale supplies a practical comparison: it operates a declared world with existing facts and authorities, whereas HENA investigates how useful organisation might arise. HENA is a possible research successor through proved capability: Pale could grow thinner if learned organisation takes over explicit coordination. What thins is its organising; what remains is the guardrail, Pale as the harness around HENA, as Codex is around the model it runs. How those systems would meet remains open, and Pale proceeds independently. A future standalone HENA, an optional Pale supervisor, a RawFish engine and a model behind a coding harness would each need their own consumer interface and comparison. The analogy between Pale and HENA, and between Counsel and MUON, supplies a research direction rather than an implemented interface.

\subsection{Objective and resource limits}

Given an input sequence $X=(x_1,\ldots,x_T)$ with $T\gg10^4$, and budgets $B_c$ for compute, $B_m$ for communication and $B_r$ for memory, the objective is to process $X$ at less than quadratic cost while maintaining or improving accuracy, context retention, compositional reasoning and adaptability. These are proposed dimensions of evaluation, not established properties.

The comparison must specify what is held equal: computation, latency, memory, or a declared combination. It must count ingestion, route selection, messages, memory access, interaction rounds and output aggregation, rather than charging only the local unit update. Learning costs and inference costs must be reported separately. A smaller communication graph is useful only if constructing and using it earns its cost in the complete task.

\subsection{Local state and sparse messages}

Let $N$ units interact over rounds indexed by $t$, distinct from the input length $T$. Unit $i$ receives a local input slice $\tilde{x}_i^t$, messages $m_i^t$ and bounded memory reads $r_i^t$. The proposed update is
\begin{equation}
 h_i^{t+1}=f_\theta(h_i^t,\tilde{x}_i^t,m_i^t,r_i^t).
\end{equation}
Streaming, chunk encoding and progressive summarisation distribute the input as $X\to\{h_i^0\}_{i=1}^N$; no unit receives the whole sequence directly. Which distinctions this ingestion retains is part of the hypothesis, not guaranteed by the notation.

The sparse neighbourhood $\mathcal{N}_i^t$ may change over time. Learned weights $a_{ij}^t$ combine encoded messages:
\begin{equation}
 m_i^t=\sum_{j\in\mathcal{N}_i^t}a_{ij}^t g(h_j^t).
\end{equation}
A query would reach selected units and memory rather than every key. Routing is a learning objective in its own right: it determines which information can meet before an answer is produced. Neighbour selection, budget enforcement and the training mechanism remain to be specified. Sparse connectivity alone supplies no guarantee of useful communication.

MUON owns the unit's internal design. HENA uses the unit interface without branching on those internals, starting with stock models and later comparing replacements through the same interface. The update equation describes the local information available to the target unit; it does not claim that a stock-model adapter exposes a differentiable state update. A stronger replacement unit and better swarm coordination are separate explanations for an improved answer.

\subsection{Memory as part of the computation}

The target memory has fixed capacity $K$ and record width $d$:
\begin{align}
 M^t&\in\mathbb{R}^{K\times d},\\
 r_i^t&=\operatorname{Read}(h_i^t,M^t),\\
 M^{t+1}&=\operatorname{Write}(M^t,\{h_i^t\}_{i=1}^N).
\end{align}
The differentiable-memory target places reads and writes in the trained computation, so task-error gradients could guide access alongside routing and local updates. Saving and retrieving text does not supply that learning path. The read and write mechanisms, overwrite policy, retention rule and assignment of credit across rounds, units and memory remain open.

HENA owns its operational store, retention, pruning and recovery. Reckon's record protocol could support costed evaluation of permitted projections of that store, including comparisons of keep-or-drop policies with FIFO and LRU. An evaluator's score would inform HENA's own retention policy; it would not transfer control of the store. Establishing which reads changed an outcome requires a separate intervention comparison, with its uncertainty and measurement cost retained.

In each interaction round, units read memory, receive messages, update state and write back. After a declared number of rounds, selected units or memory produce the answer. For example, an early maintenance note may record a connector warning, while a later repair request reaches a different unit. The collective must preserve the warning, route it to the later decision and use it in the answer. Keeping a readable record, delivering it and changing the answer are three separate observations.

\subsection{Joint loss and complexity targets}

The proposed training objective is
\begin{equation}
\begin{split}
 L={}&L_{\mathrm{task}}+\lambda_1 L_{\mathrm{comm}}\\
     &+\lambda_2 L_{\mathrm{latency}}+\lambda_3 L_{\mathrm{entropy}}.
\end{split}
\end{equation}
The extra terms charge communication, interaction steps and routing entropy. Their definitions, coefficients and optimisation procedure require selection before a comparison. Adding penalties does not itself enforce the hard budgets or show that learning converges.

\begin{center}
\small
\setlength{\tabcolsep}{3pt}
\begin{tabular}{@{}lll@{}}
\hline
Part & Baseline target & HENA target\\
\hline
Attention & $O(T^2)$ & $O(T\log T)$ or $O(T)$\\
Memory & $O(T)$ & $O(K)$, $K\ll T$\\
Communication & All-to-all, $N^2$ & Sparse neighbourhoods\\
\hline
\end{tabular}
\end{center}

The attention and memory baselines refer to the proposed comparison with a standard transformer; the communication row compares all-to-all interaction among $N$ units with sparse routes. These are different axes, not one derived complexity bound. The memory table suppresses a fixed width $d$: the store itself occupies $O(Kd)$ values. Unit states, messages and routing buffers also consume memory and must fit the declared accounting. Fixed store capacity alone does not prove bounded total memory, and none of these targets is yet a measured result.

\subsection{What would establish useful hierarchy}

Compare with a single unit without a swarm, a flat collective, a central coordinator and an imposed hierarchy, at matched compute, communication, memory and interaction cost. The resource comparison with a transformer must be stated before experiments. An improvement must survive controls for stronger units, extra rounds and extra resources; gains in one metric must retain losses and costs in the others.

A hierarchy claim must identify the joint quantities measured, why they matter for the task and which distinctions the organisation preserves or loses. Nested groupings alone do not show that the organisation emerged or helped. Passing each unit's interface tests does not establish joint adequacy: correlated errors, incompatible representations, feedback, missing messages and resource contention belong to the collective. Pairwise agreement can leave a joint fact undetermined, and allocations that fit separately can exceed a shared budget together.

HENA shares ingredients with expert routing, recurrence, cellular automata, graph message passing and retrieval systems. Its proposed combination must be compared with existing systems rather than treating those ingredients as evidence of novelty. The first simulator runs on one machine after the specification and baseline are fixed; scaling or distributed infrastructure follows a working comparison.

\subsection{Five failure conditions}

The proposal can fail in five concrete ways. Routing may collapse into a monolith, defeating the distributed organisation. Communication may grow until it costs more than attention. Memory may fill with noise so retrieval stops helping. Training may remain unstable and fail to converge. Or the collective may centralise without developing the useful hierarchy sought by the hypothesis. A comparison must retain such outcomes, including cases where a simpler baseline is better.

\subsection{Ten open research questions}

\begin{enumerate}
\item How can units encode local context without losing global meaning?
\item Does routing converge, oscillate or collapse?
\item How is credit assigned across rounds, units and memory?
\item What is stored, for how long, and how is overwriting handled?
\item What sparsity of communication is useful, and how is it enforced?
\item Does the swarm self-organise, centralise or fragment?
\item Do more interaction rounds improve reasoning or only delay the answer?
\item Which comparison is appropriate: equal compute, equal latency or equal memory?
\item Which hardware fits the computation: GPUs, CPUs or distributed machines?
\item Does the result offer a better model or a better infrastructure layer?
\end{enumerate}

This statement defines the proposed investigation. It reports no completed swarm, complexity proof, emergence result or accepted successor. An unfavourable comparison remains an answer to the question.
```
