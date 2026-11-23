---
title: "Reckon Examples"
---

<a id="reckon&#45;examples"></a>

Start with [Connect retrieval to an outcome](/reckon/python/overview#connect-retrieval-to-an-outcome) to store one cue, retrieve it and update its utility from a supplied outcome\. It includes complete code and expected output\. For a task that actually trains a learner and saves its measurements, use [Compare two replay budgets](https://causality.in/reckon/compare-replay-budgets/)\.

## Available Examples {#available-examples}

| Example | Description |
| --- | --- |
| `basic_memory_manager.py` | Write and query records under a five\-slot budget, inspect pruning events, then try utility\-based retention |

## Prerequisites {#prerequisites}

Use the project environment from [installation](/reckon/installation#set-up-the-environment)\. It includes the Python package and required Rust extension; `cargo build` alone does not install that extension into Python\.

## Running Examples {#running-examples}

From the repository root:

```bash
python src/examples/basic_memory_manager.py
```

Look for seven writes, five records still in memory and two prune events\. The script prints the records with the `important` tag\. After `Remaining records with utility policy:`, it prints the payloads and scores from the second manager\. Record IDs vary between runs\.

The event loop reads dictionaries returned by `get_event_log()`\. Their `op_type` identifies the operation, `timestamp` is the logical step supplied to `set_step()`, and `details` is a JSON string that the example decodes to read the pruning reason\. The call empties the buffer\. Save the returned events if another tool needs them\.

The first manager selects `FIFOPolicy`; the kernel uses that selected ranking to free bytes, slots and tokens together\. Read [Budget enforcement](/reckon/memory-manager#budget-enforcement) before interpreting those removals as a FIFO comparison\. Use the task comparison linked above to measure outcomes under different memory allowances\.

## Integration Notes {#integration-notes}

Examples import from the `reckon` package:

- `from reckon import CausalMemoryManager`
- `from reckon.core import MemoryBudget`
- `from reckon.memory import PayloadType`

## Further Reading {#further-reading}

- [Main Module](/reckon/python/overview)
- [Memory Module](/reckon/python/memory)
- [Core Module](/reckon/python/core)
