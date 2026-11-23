---
title: "MUON"
---

<a id="muon"></a>

A caller that swaps one model for another needs the same calls from both\. MUON \(Micro\-Unit Operating Network\) specifies those calls: what a model adapter has to report, what it can expose about its state, and when it refuses a request\. [HENA](https://github.com/getHarshOnline/HENA/blob/main/docs/index.md) studies how several units would work together through those calls\.

<Info>

**The interface is a design\.** The `muon-kernel` crate re\-exports MONAD’s kernel\. The draft v0 contract below has no unit coordinator or adapter implementation\. The first stock implementation can use today's kernel types without waiting for MONAD's paused mathematical research\.

</Info>

<CardGroup>

<Card href="&#47;muon&#47;building" title="Build the kernel crate and the paper" id="gazette-layout-0">

Compile the type re\-export and, optionally, the paper stating the proposed research\.

</Card>

<Card href="https&#58;&#47;&#47;causality&#46;in&#47;muon&#47;computation&#45;state&#45;and&#45;learning&#47;" title="Understand the reasoning problem" id="gazette-layout-1">

Follow a road notice into the next route answer and see how different units would be compared\.

</Card>

</CardGroup>

## A unit is anything that fills the slot {#a-unit-is-anything-that-fills-the-slot}

The slot is the common interface a caller would use\. An adapter would translate these five sets of calls to a model backend\.

Each unit would have its own state space and an observer interface pairing MONAD coarse\-grainings with record channels\. A coarse\-graining specifies the view of state an observer can distinguish; the channel carries the resulting records to a store maintained by its owner\. A declared view need not expose the model's complete internal state\.

- **Inference\.** Answer a request in one shot or as a stream, with explicit sampling parameters and seed behavior\.
- **State\.** Describe the state space and expose a snapshot at a declared fidelity: an opaque checkpoint, a summary such as cache occupancy, or typed internal state\. A backend that can expose only a summary must say so\.
- **Observers\.** Emit records ordered by episode, step and sequence, with input and output digests and resource use\. A store can retain those records independently; the unit does not decide its retention policy\.
- **Budgets\.** Estimate demand and refuse work without an admissible request and a live matching reservation\. Silent truncation would change the work the caller asked for\.
- **Lifecycle\.** Initialize, participate in a caller\-driven loop, checkpoint, restore and shut down\. Checkpoint fidelity is explicit, so a caller knows what can be recovered\.

## Contract and capability {#contract-and-capability}

A capability report identifies the model revision, tokenizer, quantization, backend, runtime and device, along with supported inputs, streaming, state fidelity and checkpoint behavior\. Record whether the configuration requests deterministic behavior and whether repeated runs have shown it\. A change to any field that defines the unit changes its identity\.

The report describes fixed capacity and supported behavior\. Free capacity changes as work reserves or releases it, so it belongs to the allocator used by the standalone unit coordinator or HENA\.

When choosing an adapter, check the operations and fidelity in its capability report\. If it exposes only cache occupancy, a caller that needs typed state must choose a different one\. Two units that meet the interface can still return different answers or have different costs\. Use a task comparison before replacing one\.

## Admission and reservation answer different questions {#admission-and-reservation-answer-different-questions}

Before inference, the caller states the required fidelity, tolerated error, deadline and resource ceilings\. The estimator calculates demand for the given input, unit and configuration\. The unit's capacity profile states what it can guarantee\. The allocator reports what is still free and when that observation expires\. The admission decision checks those four inputs together\.

Consider an illustrative token budget, with all other constraints already satisfied:

| Input | Value |
| --- | --- |
| Caller ceiling | 500 tokens |
| Estimated demand | 150 tokens |
| Unit capacity | 1,000 tokens |
| Unreserved quantity in the current snapshot | 200 tokens |

The request fits all three ceilings\. Suppose two requests both pass against the same snapshot\. The allocator reserves 150 tokens for the first request in one atomic update\. Only 50 tokens remain, so the second cannot reserve 150 from the same pool\. Start work only after both a positive admission decision and a matching live reservation\.

Accuracy uses a different comparison\. If the caller tolerates error up to 0\.05, a guaranteed maximum error of 0\.02 fits; a guarantee of 0\.08 does not\. Error tolerance is not a quantity to reserve, and the estimator cannot relax it to make a request fit\.

A unit must refuse an expired or mismatched reservation rather than continuing under an old allocation\. [HENA's allocator design](https://github.com/getHarshOnline/HENA/blob/main/docs/index.md#availability-and-reservations-belong-to-the-allocator) keeps the holder, work and generation identities, lease transitions and recovery cases together\. Those records are defined with the first shared pool; they prevent concurrent callers from spending the same capacity and identify the allocation an interrupted request held\.

Before mapping MONAD's compatibility `ResourceVector` into these records, define the meaning and ordering of the legacy `epsilon` coordinate and version that mapping\. Do not interpret `epsilon` as a maximum\-error guarantee without that definition\. Comparing two vectors cannot authorise a run\.

## Stock models fill the slot first {#stock-models-fill-the-slot-first}

The plan starts with adapters for stock open\-weight models\. The candidates are MLX on Apple Silicon, GGUF with llama\.cpp and Ollama, and Hugging Face Transformers\. The long\-term design is a micro\-transformer unit\. It must meet the same interface and pass a comparison fixed in advance, at matched cost, between many identical units, one stronger unit, a mixture of units and no swarm\. Coordination costs count, and every arm must meet the minimum performance for the task\. If the small units lose, the strongest single or mixed implementation can keep using the same interface\.

[How experience can change a unit](https://causality.in/muon/how-experience-changes-a-unit/) follows a notice through the current answer, the record store, learning across tasks and a possible change to the unit’s structure\. The current re\-export crate implements none of those mechanisms\.

The [training and evaluation procedure](/muon/training) specifies the future harness's data manifests, matched comparisons, checkpoint records and separate release and activation steps\. It opens no research stage\.

The [repository overview](/muon/repository) links the related projects and licence\.
