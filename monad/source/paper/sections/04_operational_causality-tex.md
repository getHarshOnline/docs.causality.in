---
title: "paper/sections/04_operational_causality.tex"
noindex: true
---

```tex
\section{Operational causality}\label{sec:operational-causality}

\subsection{Events and predicates}

An event $A$ is a measurable predicate on $\mathcal{X}$:
\begin{equation}
  A : \mathcal{X} \to \{0, 1\}\label{eq:event}
\end{equation}

Macrostates are special cases via $\CoarseGrain$:
\begin{equation}
  A_E(x) = \mathbf{1}\{ \CoarseGrain(x) \in E \}
  \quad \text{for measurable } E \subseteq \mathcal{M}_s\label{eq:macro-event}
\end{equation}

\subsection{Protocol class (allowed operations)}

Fix resources $\ResourceVec$ and define:
\begin{itemize}
  \item $\AllowedOps$: allowed interventions (preparation/controls),
  \item $\CoarseGrain$: coarse-graining map,
  \item $\RecordChannel$: record channel.
\end{itemize}

A protocol is a composition (read left-to-right as temporal sequence, not
function composition order):
\begin{equation}
  x_0 \sim \rho
  \xrightarrow{U \in \AllowedOps} x_0'
  \xrightarrow{\Evolution} x_\tau
  \xrightarrow{\CoarseGrain} m_\tau
  \xrightarrow{\RecordChannel} y_\tau\label{eq:protocol}
\end{equation}

Write $\mathsf{Prot}_R$ for the class of protocols admissible under
$\ResourceVec$: compositions of the displayed form whose intervention
lies in $\AllowedOps$ and whose decision rules respect the trial bound
$m \le m_{\max}(\ResourceVec)$.  The profile requires protocol
monotonicity:
\begin{equation}
  \ResourceVec \preceq \ResourceVec'
  \implies
  \mathsf{Prot}_R \subseteq \mathsf{Prot}_{R'}.\label{eq:protocol-monotone}
\end{equation}
Monotonicity of $\AllowedOps$ and $m_{\max}$ alone would not suffice:
detectable influence also depends on the record channel, the decision
rules, the divergence, and the threshold, and each dependency would
need its own monotonicity clause.  The protocol-level requirement
subsumes them all: the richer observer can run every protocol of the
poorer one.

\subsection{Intervention semantics}\label{sec:intervention-semantics}

The operator $\Intervention$ is defined through the allowed operations;
it is not assumed.  For an event $A$ and a value $a \in \{0, 1\}$, the
admissible interventions are
\begin{equation}
  \operatorname{Int}_R(A, a)
  = \{\, U \in \AllowedOps :
     A(U(x)) = a \text{ for all } x \in \StateSpace \,\}.\label{eq:intervention-set}
\end{equation}
$A$ is \emph{$R$-intervenable} when $\operatorname{Int}_R(A, a)$ is
nonempty for both values of $a$, and \emph{$R$-recordable} when some
allowed protocol gives $A$ a reliable record
(\Cref{sec:reliable-record}).  The notation
$P(y \mid \Intervention(A{=}a))$ abbreviates the record distribution of
the protocol obtained by substituting some
$U \in \operatorname{Int}_R(A, a)$ at the preparation stage.  The
distribution depends on the choice of $U$; the influence relation below
quantifies existentially over that choice.  The choice is not
innocent: an intervention that sets $A$ can alter arbitrarily many
other properties of the state, and two chosen interventions $U_0, U_1$
can differ beyond the value of $A$.  The relation below therefore
certifies the influence of an intervention pair; it does not isolate
the contribution of $A$ alone.  Recovering stronger causal semantics
without a structural model needs additional structure: intervention
pairs matched outside the target event, an equivalence of
interventions off $A$, or robustness of the detected change over a
class of interventions.  The profile leaves this as an open
refinement. Interventions therefore
occur at the preparation stage: $A$ is a property of the prepared
initial state (or a controlled experimental parameter), not a literal
past event being edited.  This operator differs from the do-operator of
a structural causal model, which acts on model variables by replacing
their mechanisms~\cite{pearl1995causal,pearl2009causality}; FORC
defines intervention directly through $\AllowedOps$ because no
structural model is assumed.

\subsection{Reliable record criterion (repeatable discrimination)}\label{sec:reliable-record}

A record $y_\tau$ is a \textbf{reliable record} of $A$ at horizon $\tau$
under $(\ResourceVec, s)$ if there exist a decision rule $g$ and
$m \le m_{\max}(\ResourceVec)$ repeated trials such that:
\begin{align}
  P[g(y_\tau^{1:m}) = 1 \mid A = 1] &\geq 1 - \delta
  \label{eq:reliable-record-power} \\
  P[g(y_\tau^{1:m}) = 1 \mid A = 0] &\leq \delta\label{eq:reliable-record-size}
\end{align}
for some $0\le\delta < \frac{1}{2}$.  This is the minimal operational
notion: a record is what can be reliably tested.  The criterion
establishes reliable discrimination, not temporal persistence.  A persistence requirement (the discrimination
surviving a later interval or a perturbation class) is a separate,
stronger axis, and this profile does not assume it.  The trial bound is
part of the budget: without $m \le m_{\max}(\ResourceVec)$, arbitrarily
weak effects would eventually become detectable through repetition, and
detectability would no longer be resource-indexed.

\subsection{Detectable intervention influence}\label{sec:detectable-influence-def}

Define $A \DirectInfluence B$ if there exists a protocol in
$\mathsf{Prot}_R$ such that:
\begin{enumerate}
  \item $A$ admits a reliable record under that protocol class, and
  \item Changing $A$ (via an intervention in $\operatorname{Int}_R$)
        produces a detectable change in the reliable-record
        distribution for $B$ downstream.
\end{enumerate}

``Detectable'' is implemented as a distinguishability threshold:
\begin{equation}
  \Distinguishability\bigl(
    P(y \mid \Intervention(A{=}1)),\;
    P(y \mid \Intervention(A{=}0))
  \bigr) \geq \eta\label{eq:causal-detectability}
\end{equation}
for a chosen divergence $\Distinguishability$ (total variation, KL
divergence, or another divergence) and threshold $\eta$.

The relation $\DirectInfluence$ is detectable intervention influence.
It is not called a preorder: neither reflexivity nor
transitivity holds in general under the resource-bounded definition.
Mutual influence still defines a symmetric relation
\begin{equation}
  A \MutualInfluence B \iff A \DirectInfluence B
  \text{ and } B \DirectInfluence A\label{eq:mutual-influence}
\end{equation}
with no order claim attached.  The relation is symmetric by
construction, but reflexivity and transitivity are not established,
so it is not written with a tilde, which would suggest an
equivalence.

\subsection{Properties of detectable intervention influence}

The relation $\DirectInfluence$ has the following structure:
\begin{enumerate}
  \item \textbf{Restricted reflexivity}: $R$-intervenability and
        $R$-recordability alone do not give $A \DirectInfluence A$.
        A reliably recordable event can sit below the declared
        threshold: one allowed trial through a binary channel with
        error probability $0.2$ is recordable with
        $\delta = 0.2 < \frac{1}{2}$, yet the two interventional
        record distributions differ by total variation $0.6$, and a
        declared threshold $\eta = 0.9$ detects nothing.
        For a reliably recordable $A$, $A \DirectInfluence A$ holds
        exactly when some admissible intervention pair on $A$ is
        itself detectable at the declared
        $(\Distinguishability, \eta)$: the witness must be
        threshold-compatible, not merely the two existence
        conditions.  And the relation fails outright when no allowed
        operation manipulates $A$ or no allowed protocol records it.
  \item \textbf{No transitivity}: causal influence is not transitive
        in general~\cite{halpern2016actual}.  \Cref{sec:finite-model}
        constructs a finite model of this profile, with all objects
        explicit, in which
        $A \DirectInfluence B$ and $B \DirectInfluence C$ hold while
        $A \not\DirectInfluence C$ holds by path cancellation.  A
        preorder can be recovered by taking the reflexive-transitive
        closure of $\DirectInfluence$, at the price of the
        detectability semantics: closure edges no longer certify a
        detectable influence.
  \item \textbf{Resource monotonicity}:
        $\ResourceVec \preceq \ResourceVec' \implies
         {\DirectInfluence} \subseteq {\DirectInfluenceAt{R'}}$.
        This is immediate from protocol monotonicity
        (\Cref{eq:protocol-monotone}): every witness protocol under
        $\ResourceVec$ is admissible under $\ResourceVec'$.  It is
        the property the profile relies on.
\end{enumerate}

\input{MonadFormal/Influence}
```
