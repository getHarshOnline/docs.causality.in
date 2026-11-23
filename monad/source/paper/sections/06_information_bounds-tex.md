---
title: "paper/sections/06_information_bounds.tex"
noindex: true
---

```tex
\section{Information-theoretic bounds}\label{sec:information-bounds}
% monad-open: information-profile

Detectable influence turned on a threshold: two record distributions
had to differ by at least $\eta$. The quantitative question is what
the record channel can preserve, how coarse-graining changes that
information, and how repeated observations sharpen distinguishability.

\input{MonadFormal/Information}

\paragraph{Operational coding interpretation.}
The preceding results concern the single-letter functional. Its
identification with a supremum of reliable coding rates is a separate
channel-coding theorem, not a consequence of the definition. Such an
identification must specify the channel class, independent repeated uses,
block codes, error criterion and block-level cost constraint, and prove
the converse and achievability under those assumptions
\cite{cover2006elements}. No operational coding theorem for the general
admissible-law family used here is asserted. Rates use nats per channel
use under the logarithm convention above. The functional notation
$\ChannelCapacityNats(\ResourceVec)$ also keeps the capacity
typographically distinct from the record channel $\RecordChannel$.

\input{MonadFormal/Information/Testing}
\input{MonadFormal/Information/Testing/Chernoff}
\input{MonadFormal/Information/Testing/Tilted}
\input{MonadFormal/Information/Testing/Sampling}
\input{MonadFormal/Information/Testing/Converse}
\input{MonadFormal/Information/Testing/Exponent}

\paragraph{Relation to other divergences.}
KL divergence appears as a one-sided exponent in Stein's lemma, where
one error probability is constrained and the other is minimized. Optimal
weighted Bayes error with fixed positive priors has the Chernoff exponent,
not raw $D_{\mathrm{KL}}$; equal priors are not required.
See Cover \& Thomas, Ch.~11~\cite{cover2006elements}.

\subsection{Local distinguishability and threshold assumptions}

Detectable intervention influence (\Cref{sec:operational-causality})
requires a distinguishability threshold $\eta$.  The threshold is
divergence-specific; there is no universal scaling in the sample size
$n$. In a regular local testing model with nondegenerate information,
parameter displacements of order $n^{-1/2}$ can describe the local
testing scale. Translating this into a divergence scale requires another
argument: \Cref{thm:local-density-distances} derives the first-order
total-variation and second-order squared-Hellinger coefficients from
square-root regularity. A second-order KL expansion is a different claim.
Quadratic-mean differentiability alone does not
justify the KL expansion; support and integrability conditions matter.
Per-observation divergences must also be distinguished from divergences
between the $n$-sample product laws. The moving-support construction
below gives an exact inverse-sample parameter scale.

Consequently a threshold $\eta_D(n,\alpha,\beta)$ is meaningful only
after specifying the hypothesis family, statistic, sampling model and
Type-I/Type-II error requirements. Chernoff information concerns the
optimal weighted Bayes error under its testing assumptions, not an
unqualified individual false-negative exponent.

This connects information theory and operational causality without an
ontological claim: FORC models operationally detectable influence, not
causation in itself.  \emph{FORC causal evidence is bounded by what an
observer can distinguish under its channel and resource budget.}
Undetectable influence is not asserted to be absent; the programme's
own principle separates currently indistinguishable from identical
(\Cref{sec:problem-statement}).

\input{MonadFormal/Information/Testing/Hellinger}
\input{MonadFormal/Information/Testing/Conditional}
\input{MonadFormal/Information/Testing/MovingSupport}
```
