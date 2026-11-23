---
title: "Plan a training or evaluation run"
---

<a id="plan&#45;a&#45;training&#45;or&#45;evaluation&#45;run"></a>

This is the required procedure for MUON's future training and evaluation harness\. No training or evaluation run has been performed, and the harness is not implemented\. The first stock implementation can start on today's kernel types independently of MONAD's paused mathematical research\. Later training follows its stage's agreed scope and the requirements below\.

The stock stage evaluates existing models without finetuning\. Parameter\-efficient finetuning begins in the hardened stage at the earliest, with each finetuned checkpoint labelled as a variant in the unit comparison\. Later micro\-transformer training uses the same data, comparison and release requirements\.

Offline finetuning adjusts the weights of models served through adapters\. It does not implement online surprise\-driven learning inside a unit\. The [unit interface](/muon/overview) keeps unit architecture separate from coordination across units and from a record store's retention policy\. Its [admission and conformance specification](/muon/admission) defines the interface checks that each candidate must satisfy\.

## Identify the data and permitted uses {#identify-the-data-and-permitted-uses}

Represent every training and evaluation dataset by a versioned manifest\. Name its source, preprocessing hash, splits and separate grants to run, retain as context, evaluate, train, derive, generalise and publish\. A repository licence does not establish these grants\. Normalise data into one internal schema before producing backend\-specific exports; each export refers back to the manifest\.

Do not accept anonymous data folders\. Include dataset manifest versions in the finetuned model's identity, so every checkpoint has a reproducible account of the data that produced it\.

## Fix the comparison before running it {#fix-the-comparison-before-running-it}

Freeze a versioned evaluation manifest before training or benchmarking\. Name the decision and its owner, the candidate, the retained baseline and the change being tested\. Pair before\-and\-after finetune runs under matched conditions\. Record:

- Task definitions, permitted dataset and fixture hashes, splits and eligible cases\.
- Exact model, quantisation, backend, software and hardware versions\.
- Seeds, warm\-up, repetitions and resource budget\.
- Primary measures, uncertainty method, tolerances and pass, fail and stop thresholds\.

Keep results separate by model, quantisation and backend\. Changing several components together cannot attribute an improvement to any one of them\. Where that attribution matters, compare each change separately and together on matched cases\.

Audit actual exposure to the held\-out evidence before using it as an acceptance gate\. Record which models, data, prompts and reviewers generation, reward scoring and evaluation share\. A candidate or reward generator cannot be its own only promoting evaluator: require a separately tested check on protected evidence\. Calling the same scorer again on its tuned cases supplies no independent check\.

No optimised artifact deploys without passing its held\-out gate\. This applies to weights, configurations, skills and agent write\-back\. Missing mandatory evidence remains unresolved; an aggregate score cannot override a failed blocking requirement\.

## Separate conditions from measurements {#separate-conditions-from-measurements}

For robustness comparisons, fix source cases, references, processing, measurement method and permitted operations\. State where each condition enters, how its actual level is measured and which resource budget applies\. Record the inputs actually consumed\. A perturbation of an input and a change to the measurement are different interventions\.

Compare conditions on paired cases while holding the route, reference and measurement fixed\. Compare measurement methods while holding the input and condition fixed\. If both change and their interaction matters, compare them separately and together\. Version preprocessing, thresholds and record channels when they change what can be observed\.

Report quality and resource coverage by tested condition and relevant subgroup, with the aggregation unit and denominator\. Include unavailable and partial results\. Exercise an unchanged baseline, a condition whose measured level contradicts its label, and substitution of an input or measurement method\. A result supports robustness under the tested conditions; it does not establish a claim about a different observer, population or causal mechanism\.

## Keep checkpoints with their run records {#keep-checkpoints-with-their-run-records}

Name checkpoints `{base-model}+{method}-{dataset-manifest}-{run}`\. Each records its parent, dataset manifests and evaluation deltas against the base model\. Store checkpoints with their training run records, outside this repository\.

A candidate's record binds:

- Base checkpoint, tokenizer, chat template and quantisation\.
- Datasets and every permitted\-use grant, preprocessing and seeds\.
- Training code and toolchain, backend and hardware, and the finetuning artifact's identity\.
- Evaluation manifest and evidence on retained baselines, negative transfer, interference, privacy and memorisation\.
- Routing or merge behaviour, resource envelope and rollback target\.

Keep generation scores, independent observations, unresolved criteria and the owner's decision distinct\.

## Release and activate in separate steps {#release-and-activate-in-separate-steps}

Releasing a checkpoint, admitting it to a serving adapter, another consumer's admission, Harsh Joshi's activation, a canary and a rollback each need their own recorded decision or result\. A rollback is never an unrecorded configuration edit\. Retain the admitted baseline and identify the exact rollback target\.

A MUON training result neither creates a consumer contract with Pale nor selects an occupant of its judgment seat\. Each consumer must separately accept the exact candidate and its permitted use\.

Return to the [developer overview](/muon/overview)\.
