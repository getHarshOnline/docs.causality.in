---
title: "paper/sections/03_observer_interface.tex"
noindex: true
---

```tex
\section{Observer interface}\label{sec:observer}

To avoid ambiguity, the formalism separates two interfaces.
Coarse-graining is a deterministic measurable map; the record process is
a channel, a Markov kernel between measurable spaces.  This explicit
separation prevents category errors: it never conflates coarse-graining
with time, and never treats ``entropy'' as unique.

\subsection{Coarse-graining map}

\begin{equation}
  \CoarseGrain : \mathcal{X} \to \mathcal{M}_s\label{eq:coarse-graining}
\end{equation}

A measurable map from microstates to macrostates at scale $s$.  The
choice of $\CoarseGrain$ is not unique; different scales and partitions
yield different macro-descriptions, each with its own entropy
functional.  The deterministic form makes macro-events and fibers well
defined (\Cref{sec:operational-causality,sec:entropy-arrows}).  A
stochastic coarse-graining would make $\CoarseGrain(x)$ a distribution
rather than a macrostate; that generalization replaces the map by a
Markov kernel, restates every fiber and macro-event through induced
distributions, and is not considered here.

\subsection{Record channel}

\begin{equation}
  \RecordChannel : \mathcal{M}_s \to \Delta(\mathcal{Y}_R)\label{eq:record-channel}
\end{equation}

A noisy storage/measurement channel induced by resources $\ResourceVec$.  The
record channel models the physical process of writing an observation into a
persistent medium, subject to noise, capacity limits, and energy costs
determined by $\ResourceVec$.

\subsection{Resource vector}

The resource vector
$\ResourceVec = (M_s, M_b, M_t, \varepsilon, m_{\max})$
encodes the finite budgets available to an observer:
\begin{itemize}
  \item $M_s$: maximum number of record slots,
  \item $M_b$: maximum bytes of storage,
  \item $M_t$: maximum tokens (or time steps),
  \item $\varepsilon$: energy/precision budget,
  \item $m_{\max}$: maximum number of repeated trials available to any
    decision rule.
\end{itemize}

All downstream constructs (the record channel, allowed operations, and
detectable influence) are indexed by $\ResourceVec$.

\subsection{Resource order}

A resource vector is a vector, not a set, so resource comparison is a
partial order, not set inclusion.  Define
$\ResourceVec \preceq \ResourceVec'$ componentwise, with each component
oriented so that $\ResourceVec \preceq \ResourceVec'$ means
$\ResourceVec'$ affords at least the capability of $\ResourceVec$
(at least as many slots, bytes, time steps, and trials, and at least as
much energy).  Every monotonicity statement in this profile quantifies
over this order.  The order is foundational rather than notational:
FORC is resource-indexed, and resource-monotone claims are claims about
$\preceq$.

\subsection{Allowed operations}

\begin{equation}
  \AllowedOps \subseteq
    \{ f : \mathcal{X} \to \mathcal{X} \mid
       f \text{ physically implementable under } \ResourceVec \}\label{eq:allowed-ops}
\end{equation}

The set of interventions, preparations, and controls that the observer can
perform given its resource budget.  $\AllowedOps$ is monotone in the
resource order:
$\ResourceVec \preceq \ResourceVec' \implies
 \AllowedOps \subseteq \AllowedOpsAt{R'}$, and likewise
$m_{\max}(\ResourceVec) \le m_{\max}(\ResourceVec')$.
```
