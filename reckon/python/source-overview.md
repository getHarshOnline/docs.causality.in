---
title: "Reckon Python Package"
---

<a id="reckon&#45;python&#45;package"></a>

The `reckon` package connects a task's observations and outcomes to a budgeted record store\. Python owns full records, queries and retention decisions; the required Rust extension owns compact storage, indexing and the event buffer\. Start with [the retrieval/outcome example](/reckon/python/overview#connect-retrieval-to-an-outcome) to wire the API into a decision, or jump to the [module overview](#module-overview) to find the code responsible for a behavior\.

## Package Structure {#package-structure}

The command\-line runner \(`reckon/cli.py`\) dispatches tasks\. The extension stubs \(`reckon/_rust.pyi`\) describe the types Python can import\. [Examples](/reckon/contributing/examples) and [tests](/reckon/contributing/tests) show how to run and check an integration\.

## Architecture {#architecture}

```mermaid
flowchart TB
    subgraph Package["reckon Package"]
        direction TB
        Core["core/\n(abstractions)"]
        Memory["memory/\n(CMM)"]
        Tasks["tasks/\n(benchmarks)"]
        Metrics["metrics/\n(evaluation)"]
        Viz["viz/\n(server)"]
        Generated["generated/\n(proto models)"]
    end

    subgraph External["Dependencies"]
        Rust["Rust Extension\n(_rust module)"]
        Protos["Protocol Buffers"]
    end

    Generated --> Protos
    Memory --> Rust
    Memory --> Core
    Tasks --> Memory
    Tasks --> Metrics
    Viz --> Memory
```

## Module Overview {#module-overview}

| Module | Purpose | Key Classes |
| --- | --- | --- |
| [`core/`](/reckon/python/core) | Mathematical abstractions | `ResourceVector`, `MemoryBudget`, `DirectInfluenceRecords` |
| [`memory/`](/reckon/python/memory) | Memory management | `CausalMemoryManager`, `MemoryRecord`, `RetentionPolicy` |
| [`tasks/`](/reckon/python/tasks) | Benchmark tasks | `BaseTask`, `KeyDoorTask`, `BanditTask` |
| [`metrics/`](/reckon/python/metrics) | Evaluation | `PerformanceMetrics`, `MemoryEfficiencyMetrics`, `RecordSummaryMetrics` |
| [`viz/`](/reckon/debugger/server) | Debugger server | FastAPI application, WebSocket routes |
| `generated/` | Proto\-generated wire models | betterproto2 dataclasses from `.proto` files |

## Integration Points {#integration-points}

### Rust Extension {#rust-extension}

The `reckon._rust` module provides high\-performance implementations:

- `RecordStore`: O\(1\) lookup, O\(log n\) pruning
- `EventLogger`: Ring buffer for memory events

Type stubs in `_rust.pyi` enable IDE support\.

### Protocol Buffers {#protocol-buffers}

Generated `betterproto2.Message` dataclasses in `generated/` come from the protocol sources for [records](/reckon/source/protos/reckon/record-proto), [budgets](/reckon/source/protos/reckon/budget-proto), [influence relations](/reckon/source/protos/reckon/causality-proto) and [events](/reckon/source/protos/reckon/events-proto)\. These are wire models\. The public memory and resource API uses handwritten dataclasses in [record\.py](/reckon/source/src/reckon/memory/record-py) and [resources\.py](/reckon/source/src/reckon/core/resources-py)\. Regenerating bindings does not change those public classes or give them automatic Pydantic validation or JSON Schema export; their implemented checks determine validation\.

<a id="maintenance&#45;gotchas"></a>

## Troubleshooting {#troubleshooting}

1. **Rust Extension Required**: The package will not function without the compiled Rust extension\. Rebuild after Rust changes using the [kernel procedure](/reckon/kernel#python-extension)\.

2. **Proto Regeneration**: After modifying `.proto` files, regenerate wire models with `python protos/generate_python.py` using the [optional Python 3\.10\+ protocol tools](/reckon/python/wire-messages)\. Review any corresponding handwritten public types and conversions separately\.

3. **Type Stubs**: Keep `_rust.pyi` synchronized with Rust exports when adding new Python\-visible functions\.

4. **Circular Imports**: The `TYPE_CHECKING` guard is used throughout to avoid circular import issues between modules\.

<a id="further&#45;reading"></a>

## Related guides {#related-guides}

- [Main Module Guide](/reckon/python/overview) \- Detailed module documentation
- [Architecture Overview](/reckon/architecture) \- System design and data flows
- [Examples](/reckon/contributing/examples) \- Usage examples
- [Tests](/reckon/contributing/tests) \- Test organization
