---
title: "Reckon Main Module"
---

<a id="reckon&#45;main&#45;module"></a>

Use `CausalMemoryManager` to write observations, retrieve relevant records and update their utility after an outcome\. Your task supplies the action policy and reward\. The manager decides which records to keep under its memory allowance\. The [example](#connect-retrieval-to-an-outcome) follows one delayed cue\.

The package also supplies benchmark tasks, measurement helpers and a debugger server\. Stored utility, attribution and influence\-proxy fields support experiments; interpreting a score as a causal effect needs the experiment and its assumptions\.

## Connect retrieval to an outcome {#connect-retrieval-to-an-outcome}

After [building the extension](/reckon/installation#set-up-the-environment), save this as `memory_outcome.py` and run it with `python memory_outcome.py` from the repository root\. It stores a delayed cue, retrieves it later and assigns one supplied outcome to the retrieved record\.

```python
from reckon import CausalMemoryManager, MemoryBudget, PayloadType
from reckon.core.episode_context import EpisodeContext

memory = CausalMemoryManager(
    MemoryBudget(max_slots=2, max_bytes=1_000),
    episode_id="key-door",
)
memory.set_step(1)
record_id = memory.write(
    payload={"key_room": "A"},
    payload_type=PayloadType.OBSERVATION,
    tags=["key"],
    source_op="observe_key",
)

memory.set_step(5)
with EpisodeContext(episode_id="key-door", step=5):
    hits = memory.retrieve(query={"tags": ["key"]})
    print(hits[0].payload["key_room"])
    credits = memory.observe(outcome=1.0)

record = memory.get_record(record_id)
assert record is not None
print(f"Credits: {len(credits)}, utility: {record.utility.value:.3f}")
print(f"Retained records: {memory.current_usage_slots}")
```

Expected output:

```text
A
Credits: 1, utility: 0.100
Retained records: 1
```

At step 1, the task saves the key's room\. At step 5, the query finds that record by its `key` tag and prints `A`\. `retrieve()` also adds the returned ID to the current `EpisodeContext`, so the later `observe()` call knows which record to credit\.

There is one retrieved record, so it gets all the credit\. Its utility starts at zero; the default moving average keeps 90% of the old value and adds 10% of the credited outcome, giving `0.100`\. This example supplies the outcome directly\. To find out whether the cue helps an agent open a door, connect the calls to a task's observations, decisions and reward, then compare runs with and without the cue\. Changing the supplied outcome here only changes the score update\.

Keep a context around the retrievals associated with one outcome\. Start a fresh context for the next decision, or clear its tracked IDs, so an earlier retrieval does not receive credit again\. Memory has a separate lifetime: leaving the context keeps the record\. `set_episode()` resets the stores and logger by default; pass `reset_memory=False` when the next episode should inherit records, learned scores and retained events\. Collect needed events before a reset or before the finite buffer overwrites them\.

In your own task, handle an empty retrieval before using `hits[0]`\. Query tags match any requested tag, matches are ordered by utility and the default limit is ten\. When the store fills, the [retention policy](/reckon/memory-manager#choose-what-to-keep) decides which record to remove\. Keeping the retrieval and outcome code fixed while changing that policy lets you compare the consequences of different memory choices\.

<a id="architecture"></a> <a id="submodules"></a> <a id="core&#45;mathematical&#45;abstractions"></a> <a id="memory&#45;causal&#45;memory&#45;manager"></a> <a id="tasks&#45;benchmark&#45;tasks"></a> <a id="metrics&#45;evaluation"></a> <a id="viz&#45;debugger&#45;server"></a>

## Choose a module {#choose-a-module}

| If you need to | Use |
| --- | --- |
| Store, query and retain observations | [Memory](/reckon/python/memory) |
| Define resources, channels or influence diagnostics | [Core](/reckon/python/core) |
| Add a task or learner | [Tasks](/reckon/python/tasks) |
| Compute outcomes, probes and costs | [Metrics](/reckon/python/metrics) |
| Import or stream a saved run | [Debugger server](/reckon/debugger/server) |

The [architecture guide](/reckon/architecture) follows the Python/Rust boundary and the build pipeline\.

## Follow the implementation {#follow-the-implementation}

The [memory\-manager guide](/reckon/memory-manager#integrate-the-python-manager) owns the write/read paths and the Python/Rust boundary: Python filters queries while Rust retains the live record objects, fixed admission costs, priorities, retention selection and the event buffer\. Live cost metadata can change without changing admitted accounting\. The [architecture guide](/reckon/architecture) follows the complete component and build relationships\.

Use the [generated core and memory API](https://docs.causality.in/reckon/python-api) for exact current signatures and docstrings\. The [reference build](/reckon/contributing/python-reference) uses the selected source and its real extension; authored examples and experiment explanations remain in these guides\.

## Integration {#integration}

### With Debugger UI {#with-debugger-ui}

The logger buffers operations\. Export and save the events, then import the run or append events through the visualization service\. The React timeline reads what the service makes available over REST and WebSocket\.

The [visualization module](/reckon/debugger/server) owns the import, live registration and streaming interfaces\. A displayed event is not proof that the complete source run was exported\.

### With CLI {#with-cli}

`reckon run --config` reads a YAML or JSON experiment; explicit task, policy, budget and carry/learning flags override it\. `reckon compare` runs a task/policy/budget/seed matrix and keeps individual outcomes beside their spread, peak storage and elapsed time\. The [reproducibility guide](/reckon/reproducibility#reproducing-results) owns the commands, configuration fields and current comparison limits\.

<a id="maintenance&#45;gotchas"></a>

## Troubleshooting {#troubleshooting}

1. **Dual Storage**: Records are stored in both Python \(`_records` dict\) and Rust \(`RecordStore`\)\. Ensure both are updated on write/prune operations\.

2. **Event Logger Capacity**: The ring buffer has fixed capacity \(default 10,000\)\. Events are overwritten when full\.

3. **Policy Selection**: Built\-in Python policies call the kernel's shared selector\. Python and Rust wrappers can choose different named rankings, including raw utility versus utility per byte\. Name the rule in comparisons; a shared policy name does not imply the same ranking\. Custom Python policies still implement `RetentionPolicy`\.

4. **Episode Boundaries**: Call `cmm.set_episode()` at episode start to reset state and update logger context\.

5. **Step Tracking**: Call `cmm.set_step()` before operations to ensure correct timestamps\.

<a id="further&#45;reading"></a>

## Related guides {#related-guides}

- [Parent: Package Overview](/reckon/python/source-overview)
- [Architecture](/reckon/architecture)
- [Mathematical Framework](/reckon/mathematical-core)
- [CMM Design](/reckon/memory-manager)
- [Glossary](/reckon/glossary)
