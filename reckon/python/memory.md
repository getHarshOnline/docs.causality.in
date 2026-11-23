---
title: "Reckon memory module"
---

<a id="reckon&#45;memory&#45;module"></a>

The memory manager retains full native records through PyO3, keeping the caller’s Python payload and mutable containers by reference\. A separate admission projection keeps accounting independent of later edits to those values\. It filters reads, tracks retrievals for later credit and chooses existing records to remove when a write overflows its byte, slot or token allowance\. Tasks supply observations and outcomes; stored scores do not establish a causal effect\.

Start with [one cue from retrieval to an outcome](/reckon/python/overview#connect-retrieval-to-an-outcome)\. The authored [memory\-manager guide](/reckon/memory-manager#integrate-the-python-manager) explains the write/read path, policy choices, intervention boundary, thread safety, numeric updates and diagnostic subscriptions\. Its [budget section](/reckon/memory-manager#budget-enforcement) preserves trial\-copy semantics, admission validation, caller\-supplied tokens and operation windows, including the token\-budget example\.

The [generated memory API](https://docs.causality.in/reckon/python-api/memory) reads signatures, fields and method documentation from the code\. [Build the reference](/reckon/contributing/python-reference) alongside the authored manual; generated files are not source files\.

`MemoryRecord.provenance` holds a `RecordOrigin`: the creating operation, input record IDs and content hash\. The manager accepts `source_op` and `input_ids` when writing; the record computes a missing hash\. The stored JSON keeps its `provenance` object and snake\-case keys\. These origin fields describe how the record was made; the debugger draws their input links as proxies unless separate pair\-bound probe evidence is supplied\.

Change `manager.py` for write, retrieval and scoring, `record.py` for Python record behavior and `kernel/src/record.rs` for their field model and `policies.py` for the Python selection of native retention rules\. The [kernel](/reckon/kernel) owns native rankings, store accounting and logging\. Python and Rust wrappers may choose different rankings with similar names; compare their actual rules and tie behavior before interpreting results\.

[Protocol sources](/reckon/python/wire-messages) define separate wire models: `record.proto` covers record values, `budget.proto` retention\-policy declarations and `events.proto` event types\. The [Rust model](/reckon/assets/kernel/src/record.rs) owns live laboratory records and their generated TypeScript/JSON Schema; explicit adapters preserve the historical wire meanings\. For event capture, use the [server's process\-local subscription](/reckon/debugger/server#with-causalmemorymanager-process-local-registration) or export/import path; a bounded event ring can lose history, and a saved operation does not contain the whole record\.

Part of the [Python library](/reckon/python/overview)\.
