---
title: "Q-HENA: Quantum HENA"
---

<a id="q&#45;hena&#45;quantum&#45;hena"></a>

Two questions are open before a quantum swarm can claim anything: nothing yet shows a quantum advantage for a swarm, and HENA's classical swarm has still to show that it differs from neighbouring multi\-agent systems\. Q\-HENA claims neither\. Q\-HENA is the quantum variant of HENA, developed within the [Causality](https://causality.in) research programme by Harsh Joshi for RawThoughts Enterprises Private Limited\. It would compose Q\-MUON units through quantum routing, communication and entanglement\-mediated memory\. Its own swarm store would use the record protocol of the Reckon laboratory\.

The [developer guide](/q-hena/overview) explains the proposed architecture and current source\. For the research question and comparisons, read [Quantum collective computation](https://causality.in/q-hena/quantum-collective-computation/)\.

The repository contains a Rust crate that re\-exports MONAD, Q\-MUON and Reckon, and a paper with an abstract\. The proposed quantum swarm must demonstrate its behaviour and any advantage against an adequate classical comparison\.

Q\-HENA takes HENA's classical work in by merge at each stage and reviews its quantum adaptation of that exact candidate; the classical kernel changes only where an integration point is needed\.

Planned quantum work includes swarm orchestration and gate kernels in `kernel/qpu/` for Qiskit and Cirq, Q\-HENA’s own routing and communication protocols in `protos/q-hena/`, and Python wrappers for Qiskit, Cirq and PennyLane\.

## Building {#building}

The [build guide](/q-hena/building) gives the prerequisites, kernel check and paper validation commands, including offline checks and how to interpret warnings and skipped checks\. These checks establish source integration and typesetting; they do not test quantum behaviour or advantage\.

## Licence {#licence}

RawThoughts Enterprises Private Limited applies [AGPL\-3\.0\-or\-later](/q-hena/source/LICENSE) to the work it controls here\. The [fork additions and inherited files](/q-hena/source/LICENSE-ADDITIONS) identify the paper and scaffold and their sources\. Inherited files and dependencies keep their applicable terms and notices\. The release review establishes the grants and notices for the exact tree\.

Commercial use is permitted under the applicable licence\. Alternative terms require a separate written agreement with RawThoughts Enterprises Private Limited, which can grant them only for material it is entitled to relicense; this repository grants none\. See [NOTICE](/q-hena/source/NOTICE) for ownership, [CITATION\.cff](/q-hena/source/CITATION-cff) for citation and [CONTRIBUTING\.md](/q-hena/source/CONTRIBUTING-md) for contribution requirements\. Outside contributions remain closed pending review of the [draft agreement](/q-hena/source/CLA-md) and a way to record acceptance\.

## Related repositories {#related-repositories}

| Repository | Relationship |
| --- | --- |
| [HENA](https://causality.in/hena/) | The classical swarm; this fork's upstream |
| [Q\-MUON](https://causality.in/q-muon/) | The quantum unit Q\-HENA would compose; a kernel dependency |
| [MONAD](https://causality.in/monad/) | The formal core; a kernel dependency |
| [Reckon](https://causality.in/reckon/) | The Reckon laboratory, whose record protocol Q\-HENA's store would speak; a kernel dependency |
