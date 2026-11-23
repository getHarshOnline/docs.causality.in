---
title: "HENA: Hierarchically Evolving Neural Automata"
---

<a id="hena&#45;hierarchically&#45;evolving&#45;neural&#45;automata"></a>

One large model attends to everything at once\. HENA investigates whether collective reasoning can come instead from many local units, sparse learned communication, bounded differentiable memory and emergent hierarchy: an evolving swarm of MUONs, which is why its name is plural\. It is the swarm layer of [Causality](https://causality.in), the research programme of RawThoughts Enterprises Private Limited \(RTEPL\), authored by Harsh Joshi\. Its intended first swarm would orchestrate stock open\-weight models through [MUON](https://causality.in/muon/)'s unit interface; later units could enter through that same interface\.

Start with [How units form a collective](https://causality.in/hena/how-units-form-a-collective/) for the research idea and worked example\. The [developer manual](/hena/overview) explains the current crate and the swarm coordination decisions an implementation would need to make\.

## The swarm is a research proposal {#the-swarm-is-a-research-proposal}

The Rust crate re\-exports the MONAD, MUON and Reckon kernels\. Its protocol declarations expose no swarm API\. Today the repository holds that crate and the paper; routing, shared memory and scheduling are the work in front of it, and emergent hierarchy is the hypothesis they test\. The [paper’s problem statement](/hena/source/paper/sections/00_problem_statement-tex) sets out the proposed differentiable collective, its three budgets, quantitative targets, matched comparisons, failure conditions and open questions\. It reports no swarm implementation or experimental result\.

The first experiment would use `simulator/` for a single\-machine swarm of stock units, sparse routing and bounded memory, tested on sequence modelling, key\-value retrieval and simple reasoning\. Distributed execution follows a working local comparison\.

## The kernel builds on its three dependencies {#the-kernel-builds-on-its-three-dependencies}

The kernel depends on [MONAD](https://causality.in/monad/), MUON and [Reckon](https://causality.in/reckon/) at pinned commits of their repositories\. Follow [Building](/hena/building) for the Rust and Protocol Buffers prerequisites, kernel check and separate paper build\.

## HENA composes MONAD, MUON and Reckon {#hena-composes-monad-muon-and-reckon}

| Repository | Relationship |
| --- | --- |
| [MONAD](https://causality.in/monad/) | The calculus; a direct dependency |
| [MUON](https://causality.in/muon/) | The single unit and its slot contract; a direct dependency |
| [Reckon](https://causality.in/reckon/) | Reckon's kernel is a direct dependency; HENA's future store would use its proposed record\-store interface |
| [Q\-HENA](https://causality.in/q-hena/) | The quantum variant, which keeps HENA's contracts and is kept in step with HENA by merge at each stage |

## HENA is licensed AGPL\-3\.0\-or\-later {#hena-is-licensed-agpl-3-0-or-later}

HENA's kernel, protocol headers and paper use [AGPL\-3\.0\-or\-later](/hena/source/LICENSE)\. Third\-party material keeps its own terms and required notices\.

Commercial use is permitted under the applicable licence\. Alternative terms require a separate written agreement with RawThoughts Enterprises Private Limited, which can grant them only for material it is entitled to relicense; this repository grants none\. See [NOTICE](/hena/source/NOTICE) for ownership, [CITATION\.cff](/hena/source/CITATION-cff) for citation and [CONTRIBUTING\.md](/hena/source/CONTRIBUTING-md) for contribution requirements\. Outside contributions remain closed pending review of the [draft agreement](/hena/source/CLA-md) and a way to record acceptance\.
