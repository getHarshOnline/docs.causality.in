---
title: "Q-HENA"
---

<a id="q&#45;hena"></a>

Q\-HENA proposes a quantum swarm that composes Q\-MUON units behind HENA’s contracts\. The repository contains a Rust crate that re\-exports MONAD, Q\-MUON and Reckon, and a draft paper\. You can check the crate against those pinned sources\. HENA develops the classical swarm design, and Q\-MUON studies one quantum unit\.

<CardGroup>

<Card href="https&#58;&#47;&#47;causality&#46;in&#47;q&#45;hena&#47;quantum&#45;collective&#45;computation&#47;" title="Understand the research question" id="gazette-layout-0">

See what quantum collective computation would contribute and how to compare it with classical alternatives\.

</Card>

<Card href="&#47;q&#45;hena&#47;building" title="Check the kernel crate and the paper" id="gazette-layout-1">

Check the Rust dependencies or typeset the draft paper\.

</Card>

</CardGroup>

## How computation becomes collective {#how-computation-becomes-collective}

Q\-MUON would adapt a single unit behind MUON’s slot contract\. Q\-HENA would choose which units participate, which contributions reach each other and how their work produces the task response\. Its quantum adaptation covers routing, channels and entanglement\-mediated communication and memory\. HENA supplies the classical coordination, population and recovery contracts\. Keeping those responsibilities stable would help distinguish a substrate change from an organizational change\.

An implementation must define actual preparations, interactions and measurements\. Entanglement by itself specifies neither a message protocol nor a memory operation\. Q\-HENA would own a swarm\-shaped operational store, while Reckon supplies the record protocol\. Records of work, resources and observations remain distinct from quantum state and from a checkpoint’s recovery guarantee\.

MONAD is another direct kernel dependency\. Its re\-export makes types available; a mathematical model of the swarm remains research work\.

## Follow a two\-unit contribution {#follow-a-two-unit-contribution}

Consider a proposed joint operation between units A and B:

<Steps>

<Step title="Schedule the interaction" id="gazette-layout-2">

The operation needs both units’ resources at once\. An allocation that supports only one cannot execute that interaction by scheduling A and B successively unless a separate, equivalent arrangement has been established\. Q\-HENA would need to wait for capacity or use a supported alternative whose effect on the task is understood\.

</Step>

<Step title="Recover after one unit restarts" id="gazette-layout-3">

The operation begins, but B must restart before the response is complete\. A can recover its local state from a checkpoint\. That checkpoint and a saved message record leave the state shared between the units unresolved\. Recovery would need to determine which preparations or interactions to repeat and which measurements it could reuse\. Count repeated preparation, measurement and failed attempts in the task cost\.

</Step>

</Steps>

This design case connects resource scheduling to what the collective would need to recover\. The [product explanation](https://causality.in/q-hena/quantum-collective-computation/#recovery-is-collective-too) follows the same restart\.

## Bind recovery to the actual units {#bind-recovery-to-the-actual-units}

Q\-MUON’s planned compatibility profile would identify the provider, backend, topology, calibration or noise model, circuit, measurement configuration and checkpoint representation\. It names `exact`, `semantic` and `restart_only` restore modes\. Their semantics and schema are unpublished, so those names are not a working serialization API\.

Q\-HENA would bind each unit reservation and swarm checkpoint to the exact profile and restore mode\. Provider or calibration changes, partial recovery and mixed modes need their own supported outcomes\. Exact recovery would require evidence for every unit and the composed swarm\.

For the present one\-person programme, a plain [stage note](/q-hena/source/docs/adaptation-receipt-md) identifies the candidate and resolved dependencies, profiles, comparisons, resource limits, results, uncertainty and missing coverage\. The programme owner separately decides whether that evidence closes the stage\. The linked specification keeps the later signed\-record design, deferred until a second person or a release needs it\.

## Inspect the current source {#inspect-the-current-source}

The [build guide](/q-hena/building#check-the-kernel-crate) owns the compiler prerequisites and online or offline kernel check\. A passing check confirms that the kernel crate's dependency re\-exports type\-check\.

| Source | What a developer can inspect |
| --- | --- |
| [Kernel manifest](/q-hena/source/kernel/Cargo-toml) and [crate root](/q-hena/source/kernel/src/lib-rs) | Dependencies on MONAD, Q\-MUON and Reckon, and their re\-exports\. HENA is the fork’s classical upstream, not a current Cargo dependency\. |
| [Paper](/q-hena/source/paper/Q-HENA-tex) and [swarm notation](/q-hena/source/paper/shared/q-hena/definitions-tex) | The proposed architecture’s notation and abstract\. |
| [Message](/q-hena/source/protos/hena/message-proto), [routing](/q-hena/source/protos/hena/routing-proto) and [memory](/q-hena/source/protos/hena/memory-proto) headers | Inherited protocol paths with no swarm messages implemented\. The memory header reserves Q\-HENA’s own store contract\. |

For paper prerequisites and commands, use [the build guide](/q-hena/building)\. The [licensing section](/q-hena/repository#licence) explains the selected policy and pending inherited\-rights review; the rights review \(`docs/rights-review.md` in the repository checkout, for maintainers\) specifies the inventory and acceptance records required before release\. Return to the [classical\-versus\-quantum comparison](https://causality.in/q-hena/quantum-collective-computation/#two-contributions-to-examine) when assessing what a future implementation would establish\.
