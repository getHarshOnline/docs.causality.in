---
title: "MONAD Formal Mathematics"
---

<a id="monad&#45;formal&#45;mathematics"></a>

The Lean library defines MONAD’s mathematical objects and proves their properties\. Use this guide to locate a definition, match paper notation to Lean, or maintain the library\. The paper explains the same mathematics through one TeX companion per module\.

`lean-toolchain` pins Lean 4; `lakefile.toml` pins mathlib to its matching v4\.34\.0 commit\. `lake-manifest.json` records the transitive dependency revisions\.

For a first encounter with the definitions, [read a preservation claim in Lean](/monad/mathematics#read-a-preservation-claim-in-lean)\. It carries a four\-state example through `ProbeFamily`, `Factors`, `Sufficient`, the observational quotient and the compression obstruction\. The [product worked example](https://causality.in/monad/what-a-summary-preserves/) supplies the mathematical interpretation without requiring Lean\. The index below is for locating the exact branch after that orientation\.

## Organization and parity {#organization-and-parity}

Every mathematical module has the same relative path and stem in both trees:

```text
formal/MonadFormal/Quotient.lean
paper/MonadFormal/Quotient.tex
```

Dependencies are imported, never reimplemented\. General definitions live in `Sandbox`; specialized modules add hypotheses and reuse those definitions\. The TeX companions carry `leanproof` and `leandef` bindings to fully qualified Lean declarations\. The bindings do not add code identifiers to the typeset mathematics\. Use stable result names and labels for references\. The same rule applies below a module: `Repair/Optimal.lean` mirrors `Repair/Optimal.tex`, and `Repair/Supplement.lean` mirrors `Repair/Supplement.tex`\. Parent modules own shared interfaces; children import them rather than restating their definitions\.

The correspondence checker verifies both directions of the module/companion map, rejects duplicate inclusions and stale or duplicate public imports, and checks every displayed proof's binding, declaration resolution, trusted axiom dependencies, and local notation rules\. It cannot infer that prose and Lean have identical hypotheses: that statement\-level review remains mandatory\. A theorem about finite states cannot discharge a measurable\-space claim merely because its name matches\.

## Contents {#contents}

| Module | TeX companion \(same stem\) | Result |
| --- | --- | --- |
| `Sandbox` | `Sandbox.tex` | Probe families, equivalence, factorization and sufficiency\. |
| `Quotient` | `Quotient.tex` | Exact quotient, universal property, unique descent and relabeling\. |
| `Monotonicity` | `Monotonicity.tex` | Family growth refines equivalence\. |
| `NoGo` | `NoGo.tex` | Joint separation plus sufficiency forces injectivity\. |
| `Defect` | `Defect.tex` | Empty extension defect is equivalent to pulled\-back sufficiency\. |
| `Extension` | `Extension.tex` | Covered\-family closure, non\-recovery and fiber constancy\. |
| `Refinement` | `Refinement.tex` | Least pair refinement, obtainability and carrier extension\. |
| `Counterexample` | `Counterexample.tex` | Old\-bit revelation on an unchanged carrier\. |
| `Budget` | `Budget.tex` | Residual budgets, one\-step recurrence and concatenation\. |
| `Abstraction` | `Abstraction.tex` | Exact partition backward\-completeness correspondence\. |
| `Repair` | `Repair.tex` | Adaptive feasibility, non\-recovery and free\-information bounds\. |
| `Repair/Optimal` | `Repair/Optimal.tex` | Attained minimum cost, productive\-query Bellman equation and cardinality\-founded recursion\. |
| `Repair/Supplement` | `Repair/Supplement.tex` | Exact side\-information alphabet and binary\-length bounds; question\-timing obstruction\. |
| `Approximation` | `Approximation.tex` | Normalized measurable\-test total variation and Markov contraction on general measurable spaces\. |
| `Approximation/Events` | `Approximation/Events.tex` | Event/test equivalence by layer\-cake integration; zero distance is equality of measures\. |
| `Approximation/Simulation` | `Approximation/Simulation.tex` | Common\-kernel deficiency, bounded\-risk transfer with explicit tolerance, and composition without assumed minimizers\. |
| `Profile` | `Profile.tex` | Protocol\-record correspondence and postprocessing\. |
| `Influence` | `Influence.tex` | Deterministic protocol monotonicity and nontransitivity\. |
| `Entropy` | `Entropy.tex` | Measure\-theoretic entropy conservation with integrability\. |
| `Witnesses` | `Witnesses.tex` | Pairwise/joint obstruction, macro intervention and threshold counterexamples; Bernoulli measures realize the same general total variation\. |
| `Temporal` | `Temporal.tex` | Banach\-space variation of constants, exact hidden\-state memory and terminal\-future integrals, and the paired\-state derivative\. |
| `Temporal/MixedBoundary` | `Temporal/MixedBoundary.tex` | Actual mixed\-delay path, exact existence criterion, exhaustive resonance obstruction, and separate terminal compatibility\. |
| `Temporal/Variational` | `Temporal/Variational.tex` | Coercive Hilbert\-space minimization, stationarity and exact energy gap\. |
| `Temporal/Variational/Interval` | `Temporal/Variational/Interval.tex` | Bounded primitive and shifted\-difference operators, endpoint\-constrained absolutely continuous paths, integral energy identity, unique minimizing velocity and weak Euler equation\. |
| `Temporal/Variational/Paths` | `Temporal/Variational/Paths.tex` | Exact correspondence with the full admissible H¹ path class; energy invariance and uniqueness of the minimizing path on \[0,1\]\. |
| `Temporal/Variational/Shift` | `Temporal/Variational/Shift.tex` | Shifted\-difference adjoint with separate forward/backward boundary supports and an L² force\. |
| `Temporal/Variational/Regularity` | `Temporal/Variational/Regularity.tex` | Mean\-zero orthogonality, absolutely continuous integration by parts, and velocity regularity derived from the weak equation\. |
| `Temporal/Variational/Euler` | `Temporal/Variational/Euler.tex` | Nonlocal Euler equation for the unique minimizing path, with an absolutely continuous velocity and an L² second derivative almost everywhere\. |
| `Information` | `Information.tex` | KL mutual information on probability laws, measurable and Markov processing, single\-letter capacity and admissible input costs\. |
| `Information/Likelihood` | `Information/Likelihood.tex` | Joint\-density pullback and exact extended\-valued relative\-entropy transport on arbitrary measurable carriers, without disintegration\. |
| `Information/Input` | `Information/Input.tex` | General deterministic coarse\-graining identity, capacity comparison, cost transport and measurable\-section realizability; separate standard\-Borel conditional\-law reconstruction\. |
| `Information/Testing` | `Information/Testing.tex` | Actual Bayes\-risk optimization over all randomized tests, with an attained likelihood\-ratio rule and finite\-alphabet overlap formula\. |
| `Information/Testing/Chernoff` | `Information/Testing/Chernoff.tex` | Prior\-weighted finite\-sample bound, independent\-product tensorization, coefficient bounds and extended\-valued information with exact support cases\. |
| `Information/Testing/Tilted` | `Information/Testing/Tilted.tex` | Actual common\-support tilted laws, continuous endpoint extension, attained minimum, directional optimality and likelihood transport\. |
| `Information/Testing/Sampling` | `Information/Testing/Sampling.tex` | Product\-law mean and variance, Chebyshev concentration and an explicit typical\-set sample budget\. |
| `Information/Testing/Converse` | `Information/Testing/Converse.tex` | Matching finite\-sample lower bound with exponential slack, including endpoint minimizers\. |
| `Information/Testing/Exponent` | `Information/Testing/Exponent.tex` | Full extended\-valued finite\-alphabet Chernoff exponent for fixed positive priors; unequal and disjoint supports retained\. |
| `Information/Testing/Conditional` | `Information/Testing/Conditional.tex` | Exact optimal risk from a restriction likelihood identity, on arbitrary measurable spaces, and independent\-product transport\. |
| `Information/Testing/MovingSupport` | `Information/Testing/MovingSupport.tex` | Actual Lebesgue\-uniform interval laws, exact finite\-sample risk and a nontrivial inverse\-sample local\-testing limit\. |
| `Information/Testing/Hellinger` | `Information/Testing/Hellinger.tex` | Actual probability laws from nonnegative unit L² roots, uniqueness, affinity identity and Hellinger separation\. |
| `Information/Testing/Hellinger/Variation` | `Information/Testing/Hellinger/Variation.tex` | Density formula for the existing operational TV and both sharp Hellinger–TV inequalities\. |
| `Information/Testing/Hellinger/Regularity` | `Information/Testing/Hellinger/Regularity.tex` | Scalar L² root differentiation, tangent condition, quadratic Hellinger coefficient, continuous L²\-product map into L¹ and first\-order TV coefficient\. |

## Notation correspondence {#notation-correspondence}

These are reader\-facing aliases of Lean\-owned definitions, not a second definition store\. Exact result bindings live beside each TeX statement\.

| Paper notation | Lean identifier |
| --- | --- |
| probe family `𝒫_C`; continuation\-probe family `ℱ_C` | `ProbeFamily` |
| `x ∼⁰_C y` and `x ≈_C y` | `indist F x y` \(the family instance selects the relation\) |
| `f` factors through `q` | `Factors f q` |
| `q` sufficient for `F_C` | `Sufficient F q` |
| coarsest quotient `X_C / ≈_C` | `IndistQuot`, `toQuot`, `toQuot_sufficient`, `indist_of_sufficient` |
| family growth `𝒦₁ ⊆ 𝒦₂` \(context form\) and `Γ₁ ⊆ Γ₂` \(endpoint form\) | `ProbeFamily.extend`, `indist_mono` |
| jointly separating extensions | `JointlySeparating` |
| the no\-go obstruction | `no_go` |
| pulled\-back family `f' ∘ e_*` | `ProbeFamily.pullback` |
| defect `Δ(e, q)` | `InDefect`, `DefectEmpty`, `defectEmpty_iff_sufficient` |
| class split under an extension | `extended_split`, `const_insufficient_after` |
| interventions `Int_R(A, a)` | `Interv` |
| detectable intervention influence `A ⇝_R B` | `Influences` |
| resource monotonicity from protocol inclusion | `influences_mono` |
| the finite model without transitivity | `influences_A_B`, `influences_B_C`, `not_influences_A_C` |
| covered extensions split no class | `indist_pullback_of_factors` |
| detected loss is not recovered | `no_recovery` |
| factorization is fiber constancy \(surjective `q`\) | `factors_iff_fiber_const` |
| normalized total variation `d_TV` | `Approximation.tv`; `tv_eq_eventTV` proves the event formulation |
| common\-kernel simulation error `e(K;E,F)` | `Approximation.simulationError` |
| directed deficiency `δ_sim(E,F)` | `Approximation.deficiency` |
| bounded\-loss risk `R_E(D,ℓ;θ)` | `Approximation.risk` |
| mutual information `I(ρ)`, in nats | `Information.mutualInfo` |
| channel information `I(μ,K)` and capacity `C(K;𝒜)` | `Information.channelInfo`, `Information.capacity` |
| expected\-cost constraint `ℬ_c,b` | `Information.costAdmissible` |
| optimal binary error `e_π(P,Q)` | `Information.binaryError` |
| independent sample law `P^{⊗n}` | `Information.iidLaw` |
| Chernoff coefficient `Z_s(P,Q)` and information `ξ(P,Q)` | `Information.chernoffCoeff`, `Information.chernoffInfo` |
| common support `S`, log likelihood `ℓ`, weights `w_s`, continuous partition `Z̄_s` | `Information.commonSupport`, `Information.logLikelihood`, `Information.supportWeight`, `Information.supportPartition` |
| tilted probability law `R_s` and sample sum `S_n(f)` | `Information.tiltedLaw`, `Information.sampleSum` |
| normalized negative log error `r_n` | `Information.binaryErrorRate` |
| event test on `A` and uniform interval law `U_θ` | `Information.eventTest`, `Information.intervalLaw` |
| square\-root density `r`, law `P_r`, affinity `ρ`, squared distance `H²` | `Information.RootDensity`, its `law`, `affinity`, and `hellingerSq` |
| L² root derivative `v` at `θ` | `Information.HasRootDerivAt` |
| L¹ density `r²` and product `fg` | `Information.RootDensity.density`, `Information.l2Product` |

### Approximation and simulation {#approximation-and-simulation}

The approximation branch owns its probability/Markov interfaces, not a surrogate finite test metric\. Its `δ_sim` is distinct from the observer profile's record\-error tolerance, exact extension defect, decision regret, recovery error and repair cost\. The chosen simulator is independent of the unknown parameter\. Infimum\-based risk transfer keeps an explicit positive tolerance; it does not assert an attained best simulator\. The forward bound does not prove the converse randomization theorem or resource feasibility\.

### Information and channel capacity {#information-and-channel-capacity}

The information branch reuses the same Markov\-channel interface\. Its KL and mutual information use natural logarithms and retain infinite values\. The Rust `ChannelCapacityNats` value\-holder uses the same natural\-log units but stores only a supplied finite number; it is not the formal capacity functional\. Values expressed in bits need conversion by multiplication by ln\(2\), and any projection still needs input\-law provenance\. The input theorem holds on arbitrary measurable carriers: the `Likelihood` child pulls back a joint density by testing each output event separately, without assuming a common null set for every event\. Extended\-valued relative entropy is preserved, including infinite values\. The alternative standard\-Borel disintegration proof remains mathematical conditioning, not an admitted recovery operation\.

### Binary testing {#binary-testing}

Binary testing uses mathlib's Bayes risk, an infimum over all Markov decision kernels, rather than defining optimality by the overlap formula\. Its finite bound keeps the prior factor and derives repetition from product probability laws\. Zero masses are allowed\. Chernoff information uses the extended logarithm and an open exponent interval: disjoint supports give infinity, not the finite value produced by applying `Real.log` at zero\. Coefficient bounds, identical\-law and disjoint\-support cases are proved\. The full finite\-alphabet asymptotic equality now follows from a matching lower bound and a logarithmic squeeze\. The continuous common\-support partition has endpoint values `Q(S)` and `P(S)`, not necessarily one\. Its attained minimum equals the original open\-interval infimum\. Directional optimality, rather than an assumed zero\-mean tilt, handles boundary minimizers\. The typical\-set bound uses the actual product measure and an explicit variance budget\. The limit requires both priors positive and fixed; no equal\-prior or positive\-point\-mass restriction is imposed\. Repeated\-use coding and local\-testing regularity remain separate obligations; this finite\-alphabet proof is not a theorem for arbitrary observation spaces or sample\-dependent priors\.

### Conditional laws and moving support {#conditional-laws-and-moving-support}

The separate `Conditional` child has no finite\-alphabet assumption\. A measurable restriction identity `Q|A = Q(A) P` determines actual Bayes risk; independent products multiply its likelihood factor\. `MovingSupport` applies this to Lebesgue\-uniform intervals, not counting\-measure uniform laws\. An alternative endpoint `θ + h/n` yields limiting error `min(a, b exp(-h/θ))`, with a strictly nontrivial balanced\-prior limit for `h > 0`\. This is a sample\-dependent local alternative, not the fixed\-alternative Chernoff theorem\. It establishes a counterexample to a universal square\-root sample scale, not the general quadratic\-mean differentiability, LAN or KL\-expansion theory\.

### Hellinger distance and regularity {#hellinger-distance-and-regularity}

The Hellinger branch uses an arbitrary common reference measure, not a finite alphabet\. Nonnegative unit L² elements construct probability laws, and equal laws have equal roots in that L² space\. Its half\-squared\-norm convention gives `H² = 1 - ρ`\. The `Variation` child derives the density formula from the existing test/event TV definition and proves `H² ≤ TV` and `TV² ≤ H²(2-H²)`\. There is no second operational distance\. The `Regularity` child owns scalar root differentiation: normalization implies `⟨r,v⟩ = 0`, while Hellinger squared has coefficient `‖v‖₂²/2`\. The actual bounded product `L² × L² → L¹` gives density derivative `2rv` and TV coefficient `‖rv‖₁`\. No pointwise derivative is substituted for norm differentiability\. These are single\-observation, scalar\-parameter results; product\-law local testing, score/Fisher correspondence, LAN and KL expansion/failure claims are separate outstanding work\. The common measure is fixed throughout these statements\.

## Encoding notes {#encoding-notes}

Carriers are types\. Probes have per\-index codomains\. Present and continuation\-sensitive relations share one `ProbeFamily` representation\. The no\-go module absorbs each composite `p ∘ e_*` into a family member\. Analytic results import mathlib instead of replacing real numbers, measures, integrals or topology with surrogate assumptions\.

Use Unicode mathematical notation \(`→`, `↦`, `∀`, `∃`, `≤`, `≠`, `∘`\) in Lean and ordinary ASCII TeX commands in the paper\. Match arms retain Lean's required `=>` syntax\. Input\-method escape hints belong in the editor, not the mathematical source\. Keep type/definition names and theorem names consistent with Lean conventions; do not rename public identifiers merely to simulate visual equality with a TeX symbol\. Automatic implicit\-variable creation is disabled for the library, so a misspelled or unimported symbol cannot silently become a fresh type parameter\.

## Documentation and refactoring {#documentation-and-refactoring}

Follow mathlib's [documentation](https://mathlib.org/contribute/doc.html) and [naming](https://leanprover-community.github.io/contribute/naming.html) conventions\. Module introductions identify the mathematical purpose, companion and any non\-obvious encoding choice\. Definition and major\-theorem docstrings explain meaning and essential scope; they do not narrate tactics or repeat the paper\. Use declaration references for dependencies and named TeX labels for results, not unstable theorem numbers\. Keep terminology, parameter roles and assumptions consistent across related modules\. Record necessary notation differences once at their owner; for example, `weight` denotes λ in the variational modules because Lean reserves the bare symbol\.

A refactor moves ownership, not just text\. Remove the old implementation or replace its exposition with a reference; update imports, bindings and callers together\. Review both mathematical meaning and executable behavior after the move\. Source synchronization must not leave two competing definitions or stale explanations\. The PDF contains mathematical substance and research limits; maintenance policy and proof\-coverage tracking stay in this guide and in `coverage.toml`, outside the typeset narrative\.

## Outstanding proof obligations {#outstanding-proof-obligations}

Calibration and further proving are paused\. [`coverage.toml`](/monad/source/formal/coverage-toml) lists the open work on coding and local testing, quantum correspondence and review of the paper's statements\. Every result used in a paper proof needs a proof in that module or a reference to the Lean declaration with the required hypotheses\.

No `sorry`, assumed conclusion, custom axiom or unsafe proof substitute is acceptable\. The axiom audit permits only Lean's standard `propext`, `Classical.choice` and `Quot.sound`\. Passing the paper build or the Lean build alone does not establish full correspondence or Rust implementation parity\.

## Kernel references {#kernel-references}

Each public type in `kernel/src/` carries one rustdoc ` Lean:  ` line: a fully qualified declaration in backticks, or `none.` followed by the missing correspondence\. Fields and associated methods use that type's reference and qualifications\. The checker rejects absent or malformed lines and resolves all named declarations through the same Lean existence and axiom audit as the paper bindings\.

A reference identifies the mathematical definition or result to compare\. It does not prove that the Rust type implements it: an event identifier still needs a predicate, an intervention still needs its allowed transformation, the resource comparison does not prove protocol inclusion, and a finite grid is not a continuous optimiser\. Types without a counterpart keep that gap explicit\. The public\-API tests in `kernel/tests/` exercise numeric behaviour separately; the whole\-paper and formal/kernel correspondence review remains open\.

## Build {#build}

Run `lake build` from the repository’s `formal/` directory\. Then choose the correspondence check appropriate to the question\. Both need Python 3\.11 or later\.

<CodeGroup>

```bash Strict coverage
python3 check_correspondence.py
```

```bash Allow open obligations
python3 check_correspondence.py --allow-open
```

</CodeGroup>

Strict mode fails while any source\-declared obligation remains open\. The partial check permits open work only when the register and the included paper sources agree\. Both modes still reject missing proof bindings, unresolved declarations and untrusted axioms\.

Place a standalone `% monad-open: id` comment beside a claim whose proof or review is unfinished; a whole\-paper review belongs in the master document\. Its `id` refers to one `[[open]]` entry in `coverage.toml`, which owns the scope description\. One obligation may cover several source passages\. The checker rejects missing or extra entries, duplicate IDs, malformed markers and an unreadable or invalid register\. Emptying the register while source markers remain therefore fails in both modes\. Close the markers and their entry together only after the proof or review is complete\.

These comments stay out of the rendered paper\. They record identified work; the checker cannot infer every missing hypothesis or unproved assertion from prose\. Neither a matching register nor resolved Lean declarations substitute for comparing the paper’s exact statement with Lean\.

The checker has regression tests, run from this directory with `python3 -m unittest -v test_correspondence`\. These test the gate's behavior, not the mathematical truth of a paper statement\.

## Editor verification {#editor-verification}

During proof work, check VS Code’s Problems and Lean InfoView as well as the fresh build\. Open the `formal` Lake project with its manifest and pinned toolchain\.

<Accordion title="Lean opened without the project dependencies" id="gazette-layout-0" className="gazette-layout-disclosure" _disabled>

If the server started before setup, restart the project server after installing the dependencies\. Restarting a single file can leave the old configuration active\. Recheck the edited files and public import root; an environment error is not a reason to change a proof\.

</Accordion>

Select Python 3\.11 or later for the correspondence helper; it uses the standard\-library `tomllib` module\. Keep editor diagnostics enabled\. A clean file does not establish whole\-paper coverage\.
