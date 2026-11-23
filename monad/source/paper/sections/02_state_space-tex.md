---
title: "paper/sections/02_state_space.tex"
noindex: true
---

```tex
\section{State space}\label{sec:state-space}

\subsection{Regulated closed world}

Fix a regulator $N$ (a finite discretization scale, a truncation, or a
finite dimension).  Let $(\StateSpace, \Sigma, \Measure)$ be the regulated
closed-world state space with $\sigma$-algebra $\Sigma$ and finite reference
measure $\Measure$ (for example, Liouville measure on a compact region of
classical phase space, or trace or Haar-induced structures in
finite-dimensional quantum settings).

\begin{equation}
  \StateSpace \subseteq \mathcal{X}, \quad \Measure(\StateSpace) < \infty\label{eq:regulated-state-space}
\end{equation}

The regulator bounds the resolution and the dimension of the description.
It does not make the carrier a finite set: a finite-dimensional Hilbert
space still contains uncountably many state vectors, so finite dimension
and finite cardinality are different finiteness assumptions.  FORC~0.2
assumes the measure-theoretic one.  The finite-set specialization (a
finite carrier with permutation dynamics in discrete time) is a valid
sub-profile, with stronger finiteness assumptions.

\subsection{Closed evolution (arrow-neutral assumption)}

Let $\Evolution : \StateSpace \to \StateSpace$ be the closed evolution.

\paragraph{Assumptions.}
\begin{enumerate}
  \item \textbf{Invertibility}: $\Evolution$ is a measurable bijection
        with measurable inverse, and $\Phi_{-t} = \Phi_t^{-1}$.
  \item \textbf{Measure preservation}:
        $\Measure(\Phi_t^{-1}(A)) = \Measure(A)$ for all measurable $A$.
\end{enumerate}

No topological, smooth, or continuity structure is assumed for this
closed-evolution model; the group parameter $t$ may range over
$\mathbb{Z}$ (discrete time) or $\mathbb{R}$. The entropy-conservation
result uses only the measurable structure. Additional regularity, where
needed for another result, is stated there explicitly.

Assume additionally that the evolution forms a one-parameter group:
\begin{equation}
  \Evolution \circ \Phi_s = \Phi_{t+s}, \quad \Phi_0 = \mathrm{id}\label{eq:evolution-group}
\end{equation}

This captures classical Hamiltonian flows (Liouville theorem) and the closed
quantum analogue at the ensemble level.

\input{MonadFormal/Entropy}
```
