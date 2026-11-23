---
title: "HENA"
---

<a id="hena"></a>

HENA asks whether useful collective reasoning and hierarchy can emerge from local rules: many units learning sparse communication and using bounded, differentiable memory\. This manual explains the proposed swarm coordination and how to compare it with simpler systems\. The first stock\-model swarm would provide a working baseline; it would not establish the learned routing and memory sought within a model\.

The Rust crate re\-exports MONAD, MUON and Reckon\. Its build checks their dependency wiring; the research comparisons below would test the collective behaviour\.

<CardGroup>

<Card href="&#47;hena&#47;building&#35;check&#45;the&#45;kernel" title="Build the current crate" id="gazette-layout-0">

Check the dependency wiring with MONAD, MUON and Reckon\.

</Card>

<Card href="https&#58;&#47;&#47;causality&#46;in&#47;hena&#47;how&#45;units&#45;form&#45;a&#45;collective&#47;" title="Understand the research" id="gazette-layout-1">

Follow one piece of information through the proposed collective\.

</Card>

</CardGroup>

## What the current crate exposes {#what-the-current-crate-exposes}

`hena-kernel` re\-exports `monad_kernel`, `muon_kernel` and `reckon_kernel`\. Each is a direct Cargo git dependency pinned to one commit of its repository\. HENA can therefore use MONAD's types directly as well as through the other kernels\.

There is no HENA scheduler, routing policy or store API\. Its Protocol Buffers files declare packages and imports\. They contain no messages\. A successful kernel check confirms the dependencies compile together\.

`simulator/` is the planned home for the first single\-machine experiment: run many stock units through MUON's slot, route messages sparsely and keep memory within a fixed capacity\. Sequence modelling, key\-value retrieval and simple reasoning would test the collective against a transformer at matched compute\. The simulator has no implementation yet\. Scaling the number of units and moving to distributed hardware follow a working local comparison\.

## Compose a unit without depending on its internals {#compose-a-unit-without-depending-on-its-internals}

MUON defines the intended interface for checking whether a unit can perform a task, invoking it and receiving a result\. HENA would call that interface without inspecting a model's state\. Adapters around existing open\-weight models would provide the first units\. A unit built for MUON could take one model's place through the same interface\.

This keeps changes inside a unit separate from changes to the swarm\. A stronger model might improve the answer even if the routing remains poor\. An experiment needs to distinguish that gain from better coordination\.

## Routing decides which information meets {#routing-decides-which-information-meets}

Each unit would receive part of the input, messages from a few neighbours and reads from memory\. Over several rounds, details such as the connector warning could reach the units or memory that supply the answer\.

Broadcasting every view spends communication on every recipient\. Routing everything through a central unit moves the bottleneck\. HENA instead aims to learn sparse routes within compute, communication and memory limits, so queries reach selected units and memory rather than attending to every key\. Learned routing is intended to replace full attention; stock\-model orchestration would provide the baseline for testing that aim\.

The [communication example](https://causality.in/hena/communication-and-memory/#messages-change-who-can-contribute) follows a connector warning from a maintenance note to a later repair decision\. It asks whether the route delivered information that changed the answer\.

## Shared resources need a swarm\-level decision {#shared-resources-need-a-swarm-level-decision}

Suppose two units each need the one free execution slot\. Both could agree to run when asked separately\. HENA's proposed allocator would reserve shared capacity once, bind the reservation to the scheduler's current generation and release it after the attempt\.

Define the allocator and its records when the first swarm shares a resource pool\. The unit contract distinguishes eligibility from reserved capacity; the allocator protocol is developed with the swarm coordination that uses the pool\. The fields and failure cases below preserve the proposed design for that implementation\.

### Availability and reservations belong to the allocator {#availability-and-reservations-belong-to-the-allocator}

MUON's [admission specification](https://github.com/getHarshOnline/MUON/blob/main/docs/admission.md) owns the caller's requirements, estimated demand, fixed capacity and eligibility decision\. HENA would own the two records that change as work uses the shared pool\. A standalone unit coordinator sharing capacity between callers has the same allocator responsibilities\.

- `ResourceAvailabilitySnapshotV1` identifies the allocator and pool, the fixed capacity profile, generation, currently unreserved quantities, observation and expiry times, and previous snapshot\.
- `ResourceReservationV1` binds one live generation to a holder and work identity, the requirement, demand and admission records, reserved quantities, lease generation, issue and expiry times, and recorded monotone transitions among active, consumed, released and expired states\. These are lifecycle outcomes; a reservation need not pass through all four in sequence\.

A positive admission decision does not reserve capacity\. The allocator would issue a matching reservation atomically, so two decisions against one snapshot cannot spend the same capacity\. Reserve, renew, consume and release use compare\-and\-swap transitions\. Replaying the exact request returns its original result; changing a reused request conflicts\.

Expiry or a new allocator generation releases capacity through a recorded successor, rather than an inferred timeout\. A crashed holder cannot continue past its lease, and a stale reservation cannot consume a newer allocation\. Tests must cover stale release, renewal and consumption for the exact generation, holder and attempt; concurrent reservations; crash and restart; and double consumption\. Every outcome must leave one allocator head and consistent capacity accounting\.

### Recover the swarm as well as its units {#recover-the-swarm-as-well-as-its-units}

Restoring a unit does not revive an expired reservation or rewind the swarm's store\. The proposed `SwarmCheckpointV1` would bind the swarm and run identity, scheduler generation, routing and population revision, live reservations with their hashes and expiries, ordered child checkpoints and each one's restore class, store and replay positions, the disposition of any partial recovery, and the preceding checkpoint\. Its schema and manifest are to be written with the first simulator that needs to recover shared work\.

Resume requires the current scheduler generation, matching store and replay positions, current reservations and an admitted mix of unit restore classes\. Readmission issues successor reservations\. Q\-HENA's quantum compatibility layer would build on these rules; it cannot strengthen a participating unit's recovery guarantee\.

## The swarm keeps its own history {#the-swarm-keeps-its-own-history}

HENA would keep the swarm's operational store, retention decisions and run history\. Reckon's proposed record interface would let an evaluator inspect records for scoring or debugging\. It would not give the evaluator control over pruning the swarm's store\.

Local, swarm\-wide and episode\-specific memory tiers remain a proposal\. Text retrieval could support initial orchestration\. The [trainable memory target](https://causality.in/hena/how-units-form-a-collective/#memory-can-learn-from-the-task) would make memory differentiable, putting reads and writes in the learning process alongside routes and unit updates\.

## Population changes stay inside a run {#population-changes-stay-inside-a-run}

The proposed evolution mechanism varies local rules and compares the collective behaviour they produce\. It keeps useful alternatives in behaviour categories, or niches, and records how each candidate arose\. Fixed mutation and evaluation operators let an experiment change candidates without changing how they are judged\.

Candidate state would be versioned and checkpointed within its run\. Replacing a unit implementation or using a learned organization in another application would be a separate change\. The [evolution explanation](https://causality.in/hena/how-units-form-a-collective/#evolving-local-rules) develops the research question\.

## Judge organization by the complete task {#judge-organization-by-the-complete-task}

The [paper’s problem statement](/hena/source/paper/sections/00_problem_statement-tex) sets the quantitative targets: processing at $O(T\log T)$ or $O(T)$ rather than quadratic attention cost, a fixed\-capacity store of $O(K)$ records rather than storage growing with input length $T$, and sparse communication rather than all\-to\-all $N^2$ interaction among $N$ units\. Here $K\ll T$, and the memory comparison holds record width fixed; unit states, routing buffers and messages still count towards total memory\. These are proposed targets, not measured properties of the crate\. A useful hierarchy must improve the answer or the cost of producing it against simpler alternatives: a stronger single unit, a flat collective, a central coordinator and an imposed hierarchy\. Count compute, messages, memory and interaction steps together\.

Continue with [Building](/hena/building) to work on the source, or return to [How units form a collective](https://causality.in/hena/how-units-form-a-collective/) for the research explanation\.
