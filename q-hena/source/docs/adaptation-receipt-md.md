---
title: "docs/adaptation-receipt.md"
noindex: true
---

````md
# Adaptation receipt contract

This planned contract identifies the exact quantum-swarm candidate and the classical and quantum evidence used to assess it. No receipt schema, signer implementation or generated bindings exist yet. The [developer guide](index.md) explains the repository's contents and the proposed recovery model.

For the present one-person research programme, use a plain stage note identifying the candidate, what it was tested against, the results and uncertainty, and failures or missing coverage. The programme owner makes the separate decision to close the stage. The signed design below is deferred until a second person or a release needs it; it is not a prerequisite for current stage notes.

## Receipt contents

The proposed `QHenaAdaptationReceiptV1` binds:

- The stage candidate and exact source commits of the frozen HENA candidate, selected Q-MUON adaptation, Reckon compatibility revision, MONAD and resulting Q-HENA.
- The accepted MONAD profile by reference and hash, and build evidence establishing that these dependencies were actually used.
- Required compatibility profiles and checkpoints by reference and hash.
- Model, circuit, configuration, provider, backend version and simulator or hardware identity.
- Dataset and fixture hashes, splits, seeds, shots, repetitions and resource budgets.
- Manifests of the inherited classical suite and quantum suite.
- Every command, exit status and digest of its raw results.
- Metrics, statistical method, uncertainty, tolerances, and pass, fail and stop thresholds fixed in advance.
- The evidence manifest by reference and hash, including required permissions, evaluator roles, containment and rollback evidence.
- Any predecessor receipt and the result disposition.
- Its canonical digest, evidence-authority reference and hash, and signature.

## Signing authority

The proposed `QHenaAdaptationEvidenceAuthorityV1` is a separate record. It identifies the owner, role and scope; manifests and schemas it may sign; key identity, epoch and hash; issue and expiry times; predecessor authority; rotation and recovery policy; and a monotonically advancing record of revocations and supersessions.

Canonicalise the receipt with JSON Canonicalization Scheme (RFC 8785), omitting only its own digest and signature. Sign this domain-separated input:

```text
"Q_HENA_ADAPTATION_RECEIPT_V1\0" || canonical_adaptation_receipt_sha256
```

The signing contract must fix the encoding of the digest and the signature algorithm before interoperable implementations are possible. The expression above is the planned domain separation, not a complete working signature API.

## Required rejection cases

Refuse changed bytes under the same identity; the wrong stage, commit, profile or evidence root; an aggregate pass marker without the evidence; stale predecessor receipts; unknown, retired or rolled-back signing authority; and incomplete coverage from either test suite. Before counting a receipt for a candidate, pin its exact contract manifest by reference and hash.

Keep Q-HENA's source commit fixed and substitute a different resolved Q-MUON source. Repeat with a different MONAD profile under the same MONAD source commit. Both must fail for the selected candidate. Dependency resolution and profile conformance need separate checks.

Also refuse a classical `muon/unit.proto` substituted for Q-MUON's file, or a HENA header selected by shadowing Q-HENA's include root, even when the descriptor digest matches. Compile-time schema wiring must establish which source was actually resolved.

## Proposed implementation locations

| Location | Purpose |
| --- | --- |
| `contracts/adaptation-receipt/schemas/q-hena.adaptation-receipt.v1.schema.json` | Receipt schema |
| `contracts/adaptation-receipt/schemas/q-hena.adaptation-evidence-authority.v1.schema.json` | Signer authority schema |
| `contracts/adaptation-receipt/manifests/q-hena-adaptation-receipt.v1.json` | Versioned contract manifest |
| `contracts/adaptation-receipt/fixtures/` | Canonicalisation, positive and negative cases |
| `contracts/adaptation-receipt/generated/README.md` | Binding-generation instructions |

Publish these definitions and fixtures together. Generate bindings from that source; do not treat the proposed names above as installed types.
````
