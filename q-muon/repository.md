---
title: "Q-MUON: Quantum MUON"
---

<a id="q&#45;muon&#45;quantum&#45;muon"></a>

A quantum unit raises two separate questions: whether it learns anything useful, and whether it does so better than an adequate classical unit at the same total cost\. Neither is shown for Q\-MUON, and each capability has to be shown on its own before it is claimed\. Q\-MUON is the quantum variant of [MUON](https://causality.in/muon/), the single unit of [Causality](https://causality.in), a research programme of RawThoughts Enterprises Private Limited with Harsh Joshi as its author\. It would put quantum gates, entanglement and quantum communication inside a unit while keeping MUON's slot contract unchanged, and it is the unit that [Q\-HENA](https://causality.in/q-hena/) would compose into a quantum swarm\. This repository is a fork of MUON: the planned workflow mirrors an exact frozen classical candidate before reviewing its quantum adaptation\. The [manual](/q-muon/overview) introduces Q\-MUON\.

Today the repository holds the fork of MUON, a kernel that re\-exports MONAD, and a paper with an abstract; mirroring a fixed classical MUON candidate is the first problem in front of it\. Small\-scale quantum learning provides examples for investigating this proposed unit\.

Planned quantum work includes gate kernels in `kernel/qpu/` for Qiskit and Cirq, Q\-MUON’s own communication protocols in `protos/q-muon/`, and Python wrappers for Qiskit, Cirq and PennyLane\.

The proposed stack has parallel classical and quantum paths, sharing MONAD’s formal core:

```text
Classical: MONAD → MUON  → HENA
Quantum:   MONAD → Q-MUON → Q-HENA
```

[Reckon](https://causality.in/reckon/) supplies the record protocol and measurement tools for comparisons along both paths\. The diagram describes the design; Q\-MUON’s current kernel still depends only on MONAD\.

## Building {#building}

The [build guide](/q-muon/building) gives the prerequisites, kernel check and paper validation commands, including offline checks and how to interpret warnings and skipped checks\. These checks establish source integration and typesetting; they do not test quantum behaviour or advantage\.

## Licence {#licence}

RawThoughts Enterprises Private Limited applies [AGPL\-3\.0\-or\-later](/q-muon/source/LICENSE) to the work it controls here\. The [fork additions and inherited files](/q-muon/source/LICENSE-ADDITIONS) identify the paper and scaffold and their sources\. Inherited files and dependencies keep their applicable terms and notices\. The release review establishes the grants and notices for the exact tree\.

Commercial use is permitted under the applicable licence\. Alternative terms require a separate written agreement with RawThoughts Enterprises Private Limited, which can grant them only for material it is entitled to relicense; this repository grants none\. See [NOTICE](/q-muon/source/NOTICE) for ownership, [CITATION\.cff](/q-muon/source/CITATION-cff) for citation and [CONTRIBUTING\.md](/q-muon/source/CONTRIBUTING-md) for contribution requirements\. Outside contributions remain closed pending review of the [draft agreement](/q-muon/source/CLA-md) and a way to record acceptance\.

## Related repositories {#related-repositories}

| Repository | Relationship |
| --- | --- |
| [MUON](https://causality.in/muon/) | The classical unit; this fork's upstream |
| [MONAD](https://causality.in/monad/) | The formal core; the kernel's only dependency |
| [Q\-HENA](https://causality.in/q-hena/) | The quantum swarm, which would compose Q\-MUON units |
