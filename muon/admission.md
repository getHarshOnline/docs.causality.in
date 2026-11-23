---
title: "Resource admission and slot conformance"
---

<a id="resource&#45;admission&#45;and&#45;slot&#45;conformance"></a>

These are the proposed records and tests for MUON’s version\-0 unit interface\. No unit coordinator or adapter implements them\. The first stock implementation can use today's kernel types; MONAD's mathematical research remains paused\. The [developer overview](/muon/overview) explains the five surfaces and capability report, with worked examples of quantity and error comparisons\.

The caller, estimator, unit and allocator have different responsibilities\. Admission checks whether a request is eligible; a reservation allocates capacity to a particular attempt\. Keep both decisions explicit before inference\. Define the caller\-requirement, estimated\-demand, fixed\-capacity and admission\-decision schemas with the first typed unit interface\. Define availability and reservation records with the first shared pool, whether HENA or a standalone unit coordinator owns it, as described in [HENA's design](https://github.com/getHarshOnline/HENA/blob/main/docs/index.md#shared-resources-need-a-swarm-level-decision)\.

## Admission needs five records written by different parties {#admission-needs-five-records-written-by-different-parties}

A single bare resource vector, or one request written by whoever estimates the cost, cannot keep callers, estimators and allocators honest\. Admission uses five records instead:

1. `CallerRequirementsV1`, signed by the caller or by the swarm's coordination: the input and parameter digests, the maximum tolerated error, the required fidelity and determinism, a deadline or latency class, and the caller's resource and cost ceilings\.
2. `EstimatedResourceDemandV1`, from `estimate_demand(input, params, capability_profile_ref)`: the estimator version, the unit and configuration identity, projected slots, bytes, tokens and energy, the estimate's confidence or upper\-bound policy, and how long it is valid\. It has no error\-tolerance field, so an estimate can never write, relax or replace the caller's tolerance; it may report expected error only as labelled diagnostic evidence\.
3. `ResourceCapacityProfileV1`, for one exact unit and configuration: its fixed capacity ceilings and the maximum error, fidelity, determinism and latency it guarantees\. It never carries what is free right now\.
4. `ResourceAvailabilitySnapshotV1`, issued by HENA's or the standalone unit coordinator's allocator: the current unreserved capacity and its generation and validity\.
5. `ResourceReservationV1`, issued atomically by that allocator: capacity reserved for this holder and attempt against one live generation\. [The allocator design](https://github.com/getHarshOnline/HENA/blob/main/docs/index.md#availability-and-reservations-belong-to-the-allocator) owns both records' complete fields and transitions\.

Quantities are admitted the ordinary way: demand must be within the unreserved quantity, the capacity ceiling and the caller's ceiling, for slots, bytes, tokens and every admitted energy or cost dimension\. Accuracy is a guarantee, not an amount, so its rule runs the other way: the unit's maximum error must be within the caller's tolerance, alongside the required fidelity, determinism and deadline\. Anything missing, negative, non\-finite, expired, mismatched or unversioned fails\.

`admit(caller_requirements, estimated_demand, capacity_profile, availability_snapshot)` returns a `ResourceAdmissionDecisionV1` that records all four references, the component\-by\-component result and the first constraint that failed\. The decision establishes eligibility; the caller schedules only after the allocator issues a matching live reservation\. The allocator design specifies atomic reservation, replay, expiry and recovery; the unit checks the resulting reservation before accepting the attempt\.

<Info>

**A unit refuses work without both a positive decision and a live matching reservation**, with a typed error, never by silently degrading or truncating\.

</Info>

MONAD's current `ResourceVector` is only a compatibility carrier until MONAD settles what its `epsilon` means\. An adapter may map it into these records only through an explicit, versioned mapping with tests, and no caller may treat its componentwise comparison as admission\.

## Conformance is a test suite {#conformance-is-a-test-suite}

Conformance is a suite run per implementation and per stage, not a review judgment\. It checks:

- **Completeness:** all five surfaces callable, and the report accurate against observed behaviour \(declared determinism reproduces, declared fidelity is delivered\)\.
- **Identity:** every report field is bound, and changing any of them changes the digest\.
- **Resource algebra and authorship:** equality and every single\-component failure across the four admission inputs; quantity and error orders tested separately; the estimator unable to set or relax tolerance; bad identity, expiry, malformed and non\-finite inputs failing closed; admission deterministic across language bindings\.
- **Reservation safety:** check stale release, renewal and consumption against the exact allocator generation, holder and attempt\. Exact replay is idempotent and changed reuse conflicts\. Concurrent reservations cannot oversubscribe\. Crash, restart and double consumption must leave one allocator head and consistent accounting\.
- **The compatibility vector:** any mapping from MONAD's resource vector names its reading of `epsilon` and its version, proved by golden tests, and its comparison is never used as admission\.
- **Budget honesty:** a typed refusal without a decision and a live reservation; never silent truncation\.
- **Observer floor:** at least one binding emits well\-ordered records that a Reckon\-protocol store accepts in a harness run\.
- **Checkpoint round\-trip:** checkpoint and restore give an identical report and conformant behaviour\.
- **Opacity:** one consumer drives two distinct implementations \(two adapter backends at the stock stack; an adapter and the first micro\-transformer unit at the first swap\) through the contract alone, with no backend branches\. Different capabilities and measured outputs are allowed; task adequacy and substitution need their own preregistered tests\.
- **Version:** the version is declared, and additive\-only change is verified by diffing against the stage's baseline schema\.

The stock stack exits for this contract when the suite exists, at least two adapters pass it, and the contract is stamped version 1\. Until then every interface above is a design and may change without ceremony\.

The [training and evaluation procedure](/muon/training) governs comparisons of task adequacy and replacement candidates\. A passing interface test does not establish either claim\.

Return to the [developer overview](/muon/overview)\.
