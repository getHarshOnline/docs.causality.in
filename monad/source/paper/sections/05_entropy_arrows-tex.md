---
title: "paper/sections/05_entropy_arrows.tex"
noindex: true
---

```tex
\section{Entropy, coarse-graining, and thermodynamic arrows}\label{sec:entropy-arrows}

\subsection{No arrow from fine-grained entropy}

Under closed reversible microdynamics, fine-grained entropy is invariant
(\Cref{thm:conservation}).  The claim is deliberately narrow: this
chosen fine-grained entropy supplies no monotonic arrow.  Invertibility
and measure preservation alone do not prove that every notion of
temporal orientation is absent.  This narrow invariance is the baseline
from which all directional structure must be
derived~\cite{lebowitz1993boltzmann}.

\subsection{Macro-entropy as a functional of the coarse-graining}

Select a macrospace $\mathcal{M}_s$ and coarse-graining $\CoarseGrain$.  Define
the macro-region (fiber) for macrostate $m$:
\begin{equation}
  \Gamma_m = \{ x \in \mathcal{X} : \CoarseGrain(x) = m \}\label{eq:macro-region}
\end{equation}
The fiber symbol $\Gamma_m$ is local to this section.  It is distinct from
the continuation-family symbol $\Gamma$ of \Cref{sec:problem-statement}.

Boltzmann macro-entropy:
\begin{equation}
  S_B^{(s)}(m) = k \log \Measure(\Gamma_m)\label{eq:boltzmann-entropy}
\end{equation}

\subsection{Why monotonicity is not automatic}

Even if $S_B$ often increases forward in time for macroscopic systems, the sign
of $dS_B/dt$ is \emph{not} a theorem of reversibility alone.  Monotonic increase
typically requires:
\begin{itemize}
  \item special boundary conditions (Past Hypothesis / low-entropy initial
        macrostate),
  \item typicality arguments under the dynamics, and
  \item a macro-description that supports approximately autonomous evolution.
\end{itemize}

Entropy-like quantities in learning, such as spectral entropies, are
functionals of a selected description. Their interpretation as physical
entropy, and any monotonicity claim, requires the corresponding
coarse-graining, dynamics and boundary assumptions.
```
