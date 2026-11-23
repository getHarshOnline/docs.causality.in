---
title: "Working on the mathematics"
---

<a id="working&#45;on&#45;the&#45;mathematics"></a>

Read a preservation claim by asking which questions the summary must still answer and which hypotheses the proof needs\. Start with the four\-state parts\-and\-fit example below\. Calibration and further proving are paused\.

## Read a preservation claim in Lean {#read-a-preservation-claim-in-lean}

The [four\-state product example](https://causality.in/monad/what-a-summary-preserves/) asks whether a part is available and fits a machine\. Its state is a pair of Boolean values\. Retaining availability alone merges two states with different repair answers\. To express that issue in Lean, a `ProbeFamily X` supplies an index type `I`, an answer type `O i` for each question, and a function `f i` from the carrier `X` to that answer type\. The answers need not all have the same type\.

Build the [pinned Lean environment](/monad/getting-started#build-the-lean-library) first\.

<Steps>

<Step title="Define the example" id="gazette-layout-0">

The checked\-in [reading example](/monad/source/formal/examples/ReadExample-lean) &#105;mports the library and defines these inputs:

```lean
import MonadFormal.NoGo
import MonadFormal.Quotient

open MonadFormal

namespace ReadExample

abbrev PartState := Bool × Bool

def availability (x : PartState) : Bool := x.1
def repair (x : PartState) : Bool := x.1 && x.2

def repairFamily : ProbeFamily PartState where
  I := Unit
  O := fun _ ↦ Bool
  f := fun _ ↦ repair

#eval availability (true, false)
#eval availability (true, true)
#eval repair (true, false)
#eval repair (true, true)

#check @Factors
#check @Sufficient
#check @toQuot_sufficient
#check @quotient_universal
#check @no_go

end ReadExample
```

</Step>

<Step title="Inspect the definitions and results" id="gazette-layout-1">

From the repository root, run:

```bash
make example-lean
```

The evaluations print `true`, `true`, `false`, `true`: availability agrees, but repair does not\. The remaining output shows the declarations and their parameters\. The command builds the library before reading the example\. The example stays outside the library’s import tree and needs no paper companion\.

</Step>

</Steps>

`Factors repair availability` would require a function `g : Bool → Bool` with `repair x = g (availability x)` for every state\. Both evaluated states have availability `true`, so `g true` would have to give two different repair answers\. `Sufficient repairFamily q` requires every question in the family to factor through the chosen summary `q`\. Here the family contains one question\. The requirement holds with `q = repair` and fails with `q = availability`\.

`toQuot_sufficient repairFamily` proves sufficiency of the observational quotient in general, without enumerating these four inputs\. Two states have the same quotient value exactly when every admitted question agrees on them\. In this instance, the quotient has the repair\-false class and the repair\-true class\. The example file does not compute a quotient classifier: the library's quotient is a semantic construction, with no extracted efficient encoder implied\.

Read the direction of `quotient_universal` carefully\. Given a surjective sufficient summary `q : X → Y`, it supplies a unique map **from `Y` to the observational quotient**, recovering each state's equivalence class from its summary\. It does not promise to reconstruct the original state from that class\. The summary may retain more distinctions than the quotient\. Surjectivity also matters to uniqueness: values outside the image would otherwise leave a comparison function unconstrained\.

The `no_go` signature has two premises: `JointlySeparating F` and `Sufficient F q`\. Together they imply `Function.Injective q`\. For example, admitting both coordinate questions on `Bool × Bool` separates every pair, so an exact sufficient summary can merge none of the four states\. A family containing only the repair question is not jointly separating\. The theorem therefore leaves its useful two\-class compression available\. It proves no obstruction to every approximate, resource\-bounded or revisable summary\.

Return to [recovering information or enriching the description](https://causality.in/monad/what-a-summary-preserves/#recover-information-or-enrich-the-description) for the interpretation of a missing answer\. For exact library lookup, the [formal guide's notation table](/monad/formal#notation-correspondence) maps factorisation, quotients, refinement, extension defects and repair to their declarations\.

## Each module has one Lean file and one TeX file {#each-module-has-one-lean-file-and-one-tex-file}

Each mathematical module has the same relative path and name in both trees\. For example, `formal/MonadFormal/Quotient.lean` has the companion `paper/MonadFormal/Quotient.tex`\.

- Put general definitions in the `Sandbox` module\. Specialized modules add hypotheses and import\. They do not restate\.
- A parent module owns a shared interface\. Its children import it\.
- Each displayed proof in the paper carries a hidden binding to the fully qualified Lean declaration that proves it\.
- Name a result for what it says\. Use labels, not theorem numbers, for cross\-references\.

The [formal guide](/monad/formal) lists every module and the notation that links the paper to Lean\.

## Add a result {#add-a-result}

When proof work resumes, extend the library at the module that owns the mathematical question\. A new application example does not change the shared definitions merely because it gives them a useful interpretation\.

<Steps>

<Step title="Prove the result" id="gazette-layout-2">

Add the theorem to the Lean module that owns it\. Keep its domains and hypotheses explicit\.

</Step>

<Step title="Explain it in the paper" id="gazette-layout-3">

Update the matching TeX companion and bind each displayed proof to its declaration\.

</Step>

<Step title="Check the change" id="gazette-layout-4">

Run the full check set in [getting started](/monad/getting-started)\. Record unfinished work in both its source markers and `formal/coverage.toml`, following the [formal guide’s paired\-source procedure](/monad/formal#build); leave the strict check in place\.

</Step>

</Steps>

## Proofs use standard axioms and exact hypotheses {#proofs-use-standard-axioms-and-exact-hypotheses}

- Use only the standard axioms `propext`, `Classical.choice`, and `Quot.sound`\. Do not use `sorry` or custom axioms\.
- Keep domains and hypotheses exact\. A finite example is not a general proof\.
- Import established results from mathlib\. Check their assumptions before use\.
- Automatic implicit variables are off\. Declare every variable\.

## Writing follows the notation of each language {#writing-follows-the-notation-of-each-language}

- In Lean, use Unicode notation such as `→`, `↦`, `∀`, and `∘`\. Keep `=>` only where match arms need it\.
- In the paper, use ordinary TeX commands\.
- Keep docstrings short\. Say what a definition means, not how a tactic works\.
- Keep maintenance notes out of the rendered paper\. Keep real assumptions and limits in it\.
- When you move a result, remove or replace the old version\. Update imports, bindings, and callers in the same change\.

## Reuse comes before invention {#reuse-comes-before-invention}

Solve a problem by reuse, translation, composition, and extension of known mathematics before you invent\. A reduction to known mathematics is a successful result\.
