---
title: "Reckon protocol reference"
---

<a id="reckon&#45;protocol&#45;reference"></a>

Compiled from the protocol sources\. Field numbers, explicit optional presence, oneof membership and map types are part of the wire contract\. A declared RPC does not establish its implementation; the daemon guide describes supported calls\.

- [reckon/budget\.proto](#reckon-budget-proto)
- [reckon/causality\.proto](#reckon-causality-proto)
- [reckon/core/v1/types\.proto](#reckon-core-v1-types-proto)
- [reckon/core/v1/service\.proto](#reckon-core-v1-service-proto)
- [reckon/record\.proto](#reckon-record-proto)
- [reckon/events\.proto](#reckon-events-proto)

<a id="reckon&#45;budget&#45;proto"></a>

## reckon/budget\.proto {#reckon-budget-proto-1}

Package: `reckon`\. Syntax: `proto3`\.

Reckon Budget and Resource Schema<br /> Historical wire messages for resource constraints; the laboratory's own model is the Python<br /> resource configuration in src/reckon/core/resources\.py

<a id="reckon&#45;resourcevector"></a>

### reckon\.ResourceVector {#reckon-resourcevector-1}

Multi\-dimensional resource constraint vector<br /> Defines the operational limits for an agent

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `memory_slots` | 1 | int64 | optional | Maximum number of memory slots |
| `memory_bytes` | 2 | int64 | optional | Maximum memory in bytes |
| `memory_tokens` | 3 | int64 | optional | Maximum memory in tokens |
| `bandwidth` | 4 | double | optional | Maximum bandwidth \(ops/step\) |
| `compute` | 5 | double | optional | Maximum compute per step |
| `noise_floor` | 6 | double | implicit | Irreducible noise level \(ε\_noise\) |

<a id="reckon&#45;memorybudget"></a>

### reckon\.MemoryBudget {#reckon-memorybudget-1}

Memory\-specific budget for the Causal Memory Manager

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `max_slots` | 1 | int64 | implicit | Maximum number of records to store |
| `max_bytes` | 2 | int64 | optional | Maximum total bytes |
| `max_tokens` | 3 | int64 | optional | Maximum total tokens |
| `max_write_ops_per_window` | 4 | int32 | optional | Write rate limit \(per window\) |
| `max_read_ops_per_window` | 5 | int32 | optional | Read rate limit \(per window\) |
| `window_size` | 6 | int32 | implicit | Window size for rate limiting \(in steps\) |

<a id="reckon&#45;memorymanagerconfig"></a>

### reckon\.MemoryManagerConfig {#reckon-memorymanagerconfig-1}

Configuration for the Causal Memory Manager

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `budget` | 1 | [reckon\.MemoryBudget](#reckon-memorybudget) | message presence | Memory budget constraints |
| `retention_policy` | 2 | [reckon\.RetentionPolicyType](#reckon-retentionpolicytype) | implicit | Retention policy to use |
| `evidence_weight` | 3 | double | implicit | For evidence\-per\-byte retention: supplied effect or proxy weight vs utility \(0\-1\) |
| `episode_id` | 4 | string | implicit | Episode identifier |
| `enable_dedup` | 5 | bool | implicit | Enable deduplication via content hashing |
| `compression_threshold` | 6 | int64 | optional | Enable compression for payloads above this size \(bytes\) |
| `compression_algorithm` | 7 | string | implicit | Compression algorithm |

<a id="reckon&#45;retentionpolicytype"></a>

### reckon\.RetentionPolicyType {#reckon-retentionpolicytype-1}

Retention policy types

| Value | Number | Description |
| --- | --- | --- |
| `RETENTION_POLICY_TYPE_UNSPECIFIED` | 0 |  |
| `RETENTION_POLICY_TYPE_FIFO` | 1 |  |
| `RETENTION_POLICY_TYPE_LRU` | 2 |  |
| `RETENTION_POLICY_TYPE_UTILITY` | 3 |  |
| `RETENTION_POLICY_TYPE_EVIDENCE_PER_BYTE` | 4 |  |
| `RETENTION_POLICY_TYPE_DIVERSITY` | 5 |  |

<a id="reckon&#45;causality&#45;proto"></a>

## reckon/causality\.proto {#reckon-causality-proto-1}

Package: `reckon`\. Syntax: `proto3`\.

Reckon Causality Schema<br /> Historical wire messages for causal influence records; the laboratory's own model is in<br /> src/reckon/core/causality\.py

<a id="reckon&#45;causalrelation"></a>

### reckon\.CausalRelation {#reckon-causalrelation-1}

A causal relation A ⪯\_R B under resources R<br /> Represents: varying A produces detectable change in stable records of B

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `source` | 1 | string | implicit | The upstream event identifier |
| `target` | 2 | string | implicit | The downstream event identifier |
| `resources` | 3 | [reckon\.ResourceVector](#reckon-resourcevector) | message presence | Resource constraints under which relation holds |
| `distinguishability` | 4 | double | implicit | Measured distinguishability D\(P\(y\\|do\(A\=1\)\), P\(y\\|do\(A\=0\)\)\) |
| `measure` | 5 | [reckon\.DistinguishabilityMeasure](#reckon-distinguishabilitymeasure) | implicit | The divergence measure used |
| `threshold` | 6 | double | implicit | Threshold η for detectability |
| `timestamp` | 7 | int64 | implicit | Timestamp when this relation was established |
| `trial_count` | 8 | int32 | implicit | Number of trials used to establish relation |
| `confidence_lower` | 9 | double | optional | Confidence interval \(if computed\) |
| `confidence_upper` | 10 | double | optional |  |

<a id="reckon&#45;directinfluencerecords"></a>

### reckon\.DirectInfluenceRecords {#reckon-directinfluencerecords-1}

Resource\-indexed collection of direct influence records over events\.<br /> Stored observations do not imply reflexivity or transitivity\.

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `resources` | 1 | [reckon\.ResourceVector](#reckon-resourcevector) | message presence | Resource constraints indexing these records |
| `relations` | 2 | [reckon\.CausalRelation](#reckon-causalrelation) | repeated | Recorded direct influence relations |
| `events` | 3 | string | repeated | Unique event identifiers |

<a id="reckon&#45;causalquery"></a>

### reckon\.CausalQuery {#reckon-causalquery-1}

Query for causal relations

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `source` | 1 | string | optional | Find relations with this source |
| `target` | 2 | string | optional | Find relations with this target |
| `min_distinguishability` | 3 | double | optional | Minimum distinguishability threshold |
| `after_timestamp` | 4 | int64 | optional | Only relations established after this timestamp |

<a id="reckon&#45;causalanalysisresult"></a>

### reckon\.CausalAnalysisResult {#reckon-causalanalysisresult-1}

Result of causal analysis

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `upstream_events` | 1 | string | repeated | Events that are upstream \(cause\) of the target |
| `downstream_events` | 2 | string | repeated | Events that are downstream \(effect\) of the source |
| `matching_relations` | 3 | [reckon\.CausalRelation](#reckon-causalrelation) | repeated | All relations matching the query |
| `mean_distinguishability` | 4 | double | implicit | Summary statistics |
| `total_relations` | 5 | int32 | implicit |  |

<a id="reckon&#45;distinguishabilitymeasure"></a>

### reckon\.DistinguishabilityMeasure {#reckon-distinguishabilitymeasure-1}

Divergence measure for detecting causal influence

| Value | Number | Description |
| --- | --- | --- |
| `DISTINGUISHABILITY_MEASURE_UNSPECIFIED` | 0 |  |
| `DISTINGUISHABILITY_MEASURE_TOTAL_VARIATION` | 1 |  |
| `DISTINGUISHABILITY_MEASURE_KL_DIVERGENCE` | 2 |  |
| `DISTINGUISHABILITY_MEASURE_HELLINGER` | 3 |  |

<a id="reckon&#45;core&#45;v1&#45;types&#45;proto"></a>

## reckon/core/v1/types\.proto {#reckon-core-v1-types-proto-1}

Package: `reckon.core.v1`\. Syntax: `proto3`\.

Reckon Core API v1 Types<br /> Wire messages the daemon's API uses: Record, Query, RunSummary, RunExport, ParetoPoint, Link and TagValue

<a id="reckon&#45;core&#45;v1&#45;tagvalue"></a>

### reckon\.core\.v1\.TagValue {#reckon-core-v1-tagvalue-1}

TAGS \& LINKS<br />Tag value: supports string or list of strings

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `string_value` | 1 | string | oneof `value` |  |
| `list_value` | 2 | [reckon\.core\.v1\.StringList](#reckon-core-v1-stringlist) | oneof `value` |  |

<a id="reckon&#45;core&#45;v1&#45;stringlist"></a>

### reckon\.core\.v1\.StringList {#reckon-core-v1-stringlist-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `values` | 1 | string | repeated |  |

<a id="reckon&#45;core&#45;v1&#45;link"></a>

### reckon\.core\.v1\.Link {#reckon-core-v1-link-1}

Explicit relationship between records

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `rel` | 1 | string | implicit | Relationship type: \"caused\_by\", \"derived\_from\", \"supersedes\" |
| `to_record_id` | 2 | string | implicit | Target record ID |
| `meta_json` | 3 | bytes | implicit | UTF\-8 JSON, schema\-less, optional |

<a id="reckon&#45;core&#45;v1&#45;recordcost"></a>

### reckon\.core\.v1\.RecordCost {#reckon-core-v1-recordcost-1}

RECORD COST \& SCORES<br />Resource cost of storing a record

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `bytes` | 1 | int64 | implicit | Size in bytes |
| `tokens` | 2 | int64 | implicit | Token count \(for LLM contexts\) |
| `slots` | 3 | int32 | implicit | Number of slots occupied |

<a id="reckon&#45;core&#45;v1&#45;utilityestimate"></a>

### reckon\.core\.v1\.UtilityEstimate {#reckon-core-v1-utilityestimate-1}

Utility estimates for retention decisions

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `value` | 1 | double | implicit | Estimated downstream utility |
| `uncertainty` | 2 | double | implicit | Uncertainty in value estimate \(std dev\) |
| `last_used` | 3 | int64 | implicit | Last timestep this record was retrieved |
| `use_count` | 4 | int32 | implicit | Number of times retrieved |

<a id="reckon&#45;core&#45;v1&#45;causalscore"></a>

### reckon\.core\.v1\.CausalScore {#reckon-core-v1-causalscore-1}

Causal influence estimates \(updated post\-hoc via ablation/probes\)

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `ablation_delta` | 1 | double | optional | Performance change when record is ablated |
| `counterfactual_delta` | 2 | double | optional | Performance change under counterfactual replacement |
| `influence_proxy` | 3 | double | optional | Gradient\-based or TD\-error\-based influence |
| `last_evaluated` | 4 | int64 | implicit | Timestep when causal score was last computed |

<a id="reckon&#45;core&#45;v1&#45;record"></a>

### reckon\.core\.v1\.Record {#reckon-core-v1-record-1}

RECORD<br />A causal memory record stored by the Causal Memory Manager

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `id` | 1 | string | implicit | Unique identifier |
| `timestamp` | 2 | int64 | implicit | Step/timestep when record was created |
| `episode_id` | 3 | string | implicit | Episode or run identifier |
| `payload_type` | 4 | [reckon\.core\.v1\.PayloadType](#reckon-core-v1-payloadtype) | implicit | Type of payload |
| `payload` | 5 | bytes | implicit | The actual data \(JSON\-encoded bytes\) |
| `compressed` | 6 | bool | implicit | Whether payload is compressed |
| `encoding` | 7 | string | implicit | Compression encoding \(gzip, zstd, lz4\) |
| `cost` | 8 | [reckon\.core\.v1\.RecordCost](#reckon-core-v1-recordcost) | message presence | Resource cost |
| `tags` | 9 | map\<string, [reckon\.core\.v1\.TagValue](#reckon-core-v1-tagvalue)\> | map | Map with typed tag values \(replaces repeated string tags\) |
| `links` | 10 | [reckon\.core\.v1\.Link](#reckon-core-v1-link) | repeated | Explicit relationships to other records |
| `utility` | 11 | [reckon\.core\.v1\.UtilityEstimate](#reckon-core-v1-utilityestimate) | message presence | Utility estimates |
| `causal_score` | 12 | [reckon\.core\.v1\.CausalScore](#reckon-core-v1-causalscore) | message presence | Causal influence estimates |
| `metadata` | 13 | bytes | implicit | Additional user\-defined metadata \(JSON\-encoded\) |
| `created_at_step` | 14 | int64 | optional | Step at which this record was created \(snapshot, immutable\) |
| `created_at_sequence` | 15 | int64 | optional | Sequence number at creation time \(snapshot, immutable\) |

<a id="reckon&#45;core&#45;v1&#45;historicalrunmetricsv1"></a>

### reckon\.core\.v1\.HistoricalRunMetricsV1 {#reckon-core-v1-historicalrunmetricsv1-1}

RUN METRICS \& PARETO POINT<br />Metrics carried by the historical v1 summary format\. Preserve its field names<br /> and numbers when decoding saved runs\. Names alone do not establish a formula;<br /> in particular fields 4 and 5 need the original producer and source revision\.

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `mean_return` | 1 | double | optional | Task\-facing \(PRODUCT defaults\) |
| `success_rate` | 2 | double | optional |  |
| `reward_per_total_episode` | 3 | double | optional | compute\-normalized |
| `causal_precision` | 4 | double | optional | Historical labels; do not reinterpret as current task or record metrics\. |
| `useful_memory_rate` | 5 | double | optional |  |
| `probe_overhead_ratio` | 6 | double | optional | Cost\-facing |
| `base_episodes` | 7 | int32 | optional |  |
| `probe_episodes` | 8 | int32 | optional |  |
| `probe_wall_time_s` | 9 | double | optional |  |
| `extra` | 10 | map\<string, double\> | map | Extensible extras \(keys must be documented; values numeric only\) |

<a id="reckon&#45;core&#45;v1&#45;paretopoint"></a>

### reckon\.core\.v1\.ParetoPoint {#reckon-core-v1-paretopoint-1}

Coordinates retain the supplied measurement names\.

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `x` | 1 | double | implicit | Cost metric value \(lower is better\) |
| `y` | 2 | double | implicit | Quality metric value \(higher is better\) |
| `x_metric` | 3 | string | implicit | X\-axis metric name \(e\.g\., \"probe\_overhead\_ratio\"\) |
| `y_metric` | 4 | string | implicit | Y\-axis metric name \(e\.g\., \"reward\_per\_total\_episode\"\) |
| `is_pareto` | 5 | bool | implicit | Whether this point is on the Pareto frontier |
| `label` | 6 | string | implicit | Optional display label |

<a id="reckon&#45;core&#45;v1&#45;artifactindex"></a>

### reckon\.core\.v1\.ArtifactIndex {#reckon-core-v1-artifactindex-1}

ARTIFACT INDEX

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `config_path` | 1 | string | implicit | Relative paths within run artifact folder \(empty if absent\)<br />\"config\.json\" |
| `summary_path` | 2 | string | implicit | \"summary\.md\" |
| `events_path` | 3 | string | implicit | \"logs/events\.jsonl\" |
| `metrics_csv_path` | 4 | string | implicit | \"metrics\.csv\" |
| `figure_paths` | 5 | string | repeated | \"figures/\*\.png\" |
| `log_paths` | 6 | string | repeated | other logs |

<a id="reckon&#45;core&#45;v1&#45;runsummary"></a>

### reckon\.core\.v1\.RunSummary {#reckon-core-v1-runsummary-1}

RUN SUMMARY \(UI\-ready, diff\-stable\)

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `run_id` | 1 | string | implicit |  |
| `config_id` | 2 | string | implicit | Identity for \"You Are Here\" marker A<br /> 12\-char hash\_config\(normalized config JSON\) |
| `config_json` | 3 | bytes | implicit | Full normalized config as UTF\-8 JSON \(includes policy knobs etc\.\) |
| `notebook_name` | 4 | string | implicit | Provenance / context |
| `run_mode` | 5 | string | implicit | \"demo\" \\| \"paper\" |
| `git_hash` | 6 | string | implicit | present in config but excluded from hash |
| `started_at_ms` | 7 | int64 | implicit | Timing |
| `finished_at_ms` | 8 | int64 | implicit |  |
| `task_name` | 9 | string | implicit | Core task/budget summary \(duplicated from config for easy filtering\) |
| `num_episodes` | 10 | int32 | implicit |  |
| `budget_slots` | 11 | int32 | implicit |  |
| `budget_bytes` | 12 | int64 | optional |  |
| `budget_tokens` | 13 | int64 | optional |  |
| `seed` | 14 | int32 | implicit |  |
| `seed_count` | 15 | int32 | implicit |  |
| `policy_name` | 16 | string | implicit |  |
| `metrics` | 17 | [reckon\.core\.v1\.HistoricalRunMetricsV1](#reckon-core-v1-historicalrunmetricsv1) | message presence | Historical v1 metrics; current task diagnostics have their own producers\. |
| `pareto_point` | 18 | [reckon\.core\.v1\.ParetoPoint](#reckon-core-v1-paretopoint) | message presence | Pareto location \+ axis semantics |
| `core_api_version` | 19 | string | implicit | Schema/version pointers for consumers<br />\"1\.0\" |
| `events_schema_version` | 20 | string | implicit | \"LogEvent/v1\" |
| `metrics_schema_version` | 21 | string | implicit | \"MetricsCSV/v1\" \(optional\) |
| `artifacts` | 22 | [reckon\.core\.v1\.ArtifactIndex](#reckon-core-v1-artifactindex) | message presence | Artifact inventory \(so clients know what's included\) |

<a id="reckon&#45;core&#45;v1&#45;runexport"></a>

### reckon\.core\.v1\.RunExport {#reckon-core-v1-runexport-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `run_id` | 1 | string | implicit |  |
| `summary` | 2 | [reckon\.core\.v1\.RunSummary](#reckon-core-v1-runsummary) | message presence |  |
| `events_jsonl_bytes` | 3 | bytes | implicit | Raw JSONL from Rust logger; NO type coupling in v1 |
| `metrics_csv_bytes` | 4 | bytes | optional | Optional heavy blobs \(controlled by ExportRunRequest flags\) |
| `records_snapshot_json` | 5 | bytes | optional | Optional: snapshot of records \(scoped, not entire store\)<br />UTF\-8 JSON array |
| `artifacts_tar_gz` | 6 | bytes | optional | Optional: single\-file export |
| `core_api_version` | 7 | string | implicit | Version tag<br />\"1\.0\" |
| `source_revision` | 8 | uint64 | implicit | Revision of the daemon's retained run observations, not the live store\. |

<a id="reckon&#45;core&#45;v1&#45;creditassignment"></a>

### reckon\.core\.v1\.CreditAssignment {#reckon-core-v1-creditassignment-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `record_id` | 1 | string | implicit |  |
| `credit` | 2 | double | implicit |  |
| `policy` | 3 | [reckon\.core\.v1\.CreditPolicy](#reckon-core-v1-creditpolicy) | implicit |  |

<a id="reckon&#45;core&#45;v1&#45;episodecontext"></a>

### reckon\.core\.v1\.EpisodeContext {#reckon-core-v1-episodecontext-1}

Episode context for credit linkage

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `id` | 1 | string | implicit |  |
| `episode_id` | 2 | string | implicit |  |
| `step` | 3 | int64 | implicit |  |
| `retrieved_record_ids` | 4 | string | repeated |  |
| `created_at_ms` | 5 | int64 | implicit |  |
| `ttl_ms` | 6 | int64 | optional | context expiry for daemon mode |

<a id="reckon&#45;core&#45;v1&#45;evidence"></a>

### reckon\.core\.v1\.Evidence {#reckon-core-v1-evidence-1}

EVIDENCE \(from causal probing\)

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `record_id` | 1 | string | implicit |  |
| `test_type` | 2 | string | implicit |  |
| `baseline_mean` | 3 | double | implicit |  |
| `baseline_std` | 4 | double | implicit |  |
| `intervention_mean` | 5 | double | implicit |  |
| `intervention_std` | 6 | double | implicit |  |
| `delta` | 7 | double | implicit |  |
| `effect_size_d` | 8 | double | implicit | Cohen's d |
| `p_value` | 9 | double | implicit |  |
| `p_value_adjusted` | 10 | double | implicit |  |
| `ci_lower` | 11 | double | implicit |  |
| `ci_upper` | 12 | double | implicit |  |
| `n_trials` | 13 | int32 | implicit |  |
| `power` | 14 | double | optional |  |
| `is_significant` | 15 | bool | implicit |  |
| `is_significant_adjusted` | 16 | bool | implicit |  |

<a id="reckon&#45;core&#45;v1&#45;tagquery"></a>

### reckon\.core\.v1\.TagQuery {#reckon-core-v1-tagquery-1}

QUERY TYPES

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `tags` | 1 | map\<string, [reckon\.core\.v1\.TagValue](#reckon-core-v1-tagvalue)\> | map | Match records with these tags \(AND semantics\) |

<a id="reckon&#45;core&#45;v1&#45;idquery"></a>

### reckon\.core\.v1\.IdQuery {#reckon-core-v1-idquery-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `ids` | 1 | string | repeated | Match records with these IDs |

<a id="reckon&#45;core&#45;v1&#45;utilityquery"></a>

### reckon\.core\.v1\.UtilityQuery {#reckon-core-v1-utilityquery-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `min_utility` | 1 | double | implicit | Match records with utility above this threshold |
| `episode_id` | 2 | string | optional | Optional: limit to specific episode |

<a id="reckon&#45;core&#45;v1&#45;episodecontextref"></a>

### reckon\.core\.v1\.EpisodeContextRef {#reckon-core-v1-episodecontextref-1}

Episode context reference for tracking retrieved records

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `context_id` | 1 | string | optional | Existing context ID \(for RPC mode\) |
| `new_episode_id` | 2 | string | optional | Or create new context with this episode ID |

<a id="reckon&#45;core&#45;v1&#45;probeconfig"></a>

### reckon\.core\.v1\.ProbeConfig {#reckon-core-v1-probeconfig-1}

PROBE CONFIG

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `test_type` | 1 | string | implicit | Test type: \"t\_test\", \"permutation\", \"bootstrap\" |
| `n_trials` | 2 | int32 | implicit | Number of trials per probe |
| `n_permutations` | 3 | int32 | implicit | For permutation test |
| `alpha` | 4 | double | implicit | Significance level |
| `delta_threshold` | 5 | double | implicit | Minimum effect size to consider significant |
| `fdr_method` | 6 | string | implicit | FDR method: \"none\", \"bonferroni\", \"bh\" |
| `adaptive_sampling` | 7 | bool | implicit | Enable adaptive sampling |
| `early_stopping` | 8 | bool | implicit | Enable early stopping |
| `max_trials` | 9 | int32 | implicit | Maximum trials for adaptive sampling |

<a id="reckon&#45;core&#45;v1&#45;probecostsummary"></a>

### reckon\.core\.v1\.ProbeCostSummary {#reckon-core-v1-probecostsummary-1}

Probe cost summary

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `probe_episodes` | 1 | int32 | implicit | Total episodes used for probing |
| `probe_wall_time_s` | 2 | double | implicit | Total wall time for probes \(seconds\) |
| `records_probed` | 3 | int32 | implicit | Number of records probed |

<a id="reckon&#45;core&#45;v1&#45;coreapiversion"></a>

### reckon\.core\.v1\.CoreApiVersion {#reckon-core-v1-coreapiversion-1}

API VERSION

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `version` | 1 | string | implicit | \"1\.0\" |

<a id="reckon&#45;core&#45;v1&#45;payloadtype"></a>

### reckon\.core\.v1\.PayloadType {#reckon-core-v1-payloadtype-1}

PAYLOAD TYPE<br />Type of payload stored in a record

| Value | Number | Description |
| --- | --- | --- |
| `PAYLOAD_TYPE_UNSPECIFIED` | 0 |  |
| `PAYLOAD_TYPE_OBSERVATION` | 1 |  |
| `PAYLOAD_TYPE_ACTION` | 2 |  |
| `PAYLOAD_TYPE_REWARD` | 3 |  |
| `PAYLOAD_TYPE_CUE` | 4 |  |
| `PAYLOAD_TYPE_TRACE` | 5 |  |
| `PAYLOAD_TYPE_REPRESENTATION` | 6 |  |
| `PAYLOAD_TYPE_CUSTOM` | 7 |  |

<a id="reckon&#45;core&#45;v1&#45;recordscope"></a>

### reckon\.core\.v1\.RecordScope {#reckon-core-v1-recordscope-1}

RUN EXPORT \(artifact bundle, JSONL\-first for events\)<br />Record snapshot scope for export

| Value | Number | Description |
| --- | --- | --- |
| `RECORD_SCOPE_UNSPECIFIED` | 0 |  |
| `RECORD_SCOPE_NONE` | 1 | no records |
| `RECORD_SCOPE_RETRIEVED` | 2 | records that were retrieved |
| `RECORD_SCOPE_WRITTEN` | 3 | records that were written |
| `RECORD_SCOPE_TOUCHED` | 4 | retrieved OR written |
| `RECORD_SCOPE_ALL` | 5 | entire store \(use with caution\) |

<a id="reckon&#45;core&#45;v1&#45;creditpolicy"></a>

### reckon\.core\.v1\.CreditPolicy {#reckon-core-v1-creditpolicy-1}

CREDIT ASSIGNMENT

| Value | Number | Description |
| --- | --- | --- |
| `CREDIT_POLICY_UNSPECIFIED` | 0 |  |
| `CREDIT_POLICY_NONE` | 1 | Standalone logging \(degenerate\) |
| `CREDIT_POLICY_SIMPLE` | 2 | Equal credit |
| `CREDIT_POLICY_SHAPED` | 3 | Utility\-shaped |
| `CREDIT_POLICY_ABLATION` | 4 | Positive measured ablation deltas |

<a id="reckon&#45;core&#45;v1&#45;service&#45;proto"></a>

## reckon/core/v1/service\.proto {#reckon-core-v1-service-proto-1}

Package: `reckon.core.v1`\. Syntax: `proto3`\.

Reckon Core API v1 Service<br /> The daemon's five\-method RPC API: Write, Retrieve, Observe, Probe and ExportRun

<a id="reckon&#45;core&#45;v1&#45;writerequest"></a>

### reckon\.core\.v1\.WriteRequest {#reckon-core-v1-writerequest-1}

WRITE

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `payload` | 1 | bytes | implicit | The payload data to write |
| `payload_type` | 2 | [reckon\.core\.v1\.PayloadType](#reckon-core-v1-payloadtype) | implicit | Type of payload |
| `tags` | 3 | map\<string, [reckon\.core\.v1\.TagValue](#reckon-core-v1-tagvalue)\> | map | Tags for the record |
| `links` | 4 | [reckon\.core\.v1\.Link](#reckon-core-v1-link) | repeated | Links to other records |
| `initial_utility` | 5 | double | implicit | Initial utility estimate |
| `run_id` | 6 | string | implicit | Optional local run identity\. Empty operations have no export provenance\. |

<a id="reckon&#45;core&#45;v1&#45;writeresponse"></a>

### reckon\.core\.v1\.WriteResponse {#reckon-core-v1-writeresponse-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `record_id` | 1 | string | implicit | Assigned record ID |
| `timestamp` | 2 | int64 | implicit | Timestamp when written |
| `episode_id` | 3 | string | implicit | Episode ID |
| `cost` | 4 | [reckon\.core\.v1\.RecordCost](#reckon-core-v1-recordcost) | message presence | Resource cost |
| `api_version` | 5 | [reckon\.core\.v1\.CoreApiVersion](#reckon-core-v1-coreapiversion) | message presence | API version |

<a id="reckon&#45;core&#45;v1&#45;retrieverequest"></a>

### reckon\.core\.v1\.RetrieveRequest {#reckon-core-v1-retrieverequest-1}

RETRIEVE

**oneof `query`:** Query type

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `by_tags` | 1 | [reckon\.core\.v1\.TagQuery](#reckon-core-v1-tagquery) | oneof `query` |  |
| `by_ids` | 2 | [reckon\.core\.v1\.IdQuery](#reckon-core-v1-idquery) | oneof `query` |  |
| `by_utility` | 3 | [reckon\.core\.v1\.UtilityQuery](#reckon-core-v1-utilityquery) | oneof `query` |  |
| `limit` | 10 | int32 | implicit | Maximum number of records to return |
| `context` | 11 | [reckon\.core\.v1\.EpisodeContextRef](#reckon-core-v1-episodecontextref) | message presence | Episode context for credit tracking |
| `run_id` | 12 | string | implicit | Must match a reused context's run; empty means unattributed operations\. |

<a id="reckon&#45;core&#45;v1&#45;retrieveresponse"></a>

### reckon\.core\.v1\.RetrieveResponse {#reckon-core-v1-retrieveresponse-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `records` | 1 | [reckon\.core\.v1\.Record](#reckon-core-v1-record) | repeated | Retrieved records |
| `episode_context_id` | 2 | string | implicit | Episode context ID \(for use with Observe\) |
| `api_version` | 3 | [reckon\.core\.v1\.CoreApiVersion](#reckon-core-v1-coreapiversion) | message presence | API version |

<a id="reckon&#45;core&#45;v1&#45;observerequest"></a>

### reckon\.core\.v1\.ObserveRequest {#reckon-core-v1-observerequest-1}

OBSERVE

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `episode_context_id` | 1 | string | implicit | Episode context ID from Retrieve |
| `outcome_value` | 2 | double | implicit | Outcome value \(reward, success indicator, etc\.\) |
| `outcome_type` | 3 | string | implicit | Type of outcome \(for logging/analysis\) |
| `credit_policy` | 4 | [reckon\.core\.v1\.CreditPolicy](#reckon-core-v1-creditpolicy) | implicit | Credit assignment policy |

<a id="reckon&#45;core&#45;v1&#45;observeresponse"></a>

### reckon\.core\.v1\.ObserveResponse {#reckon-core-v1-observeresponse-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `credits` | 1 | [reckon\.core\.v1\.CreditAssignment](#reckon-core-v1-creditassignment) | repeated | Credit assignments made |
| `policy_used` | 2 | [reckon\.core\.v1\.CreditPolicy](#reckon-core-v1-creditpolicy) | implicit | Policy used |
| `api_version` | 3 | [reckon\.core\.v1\.CoreApiVersion](#reckon-core-v1-coreapiversion) | message presence | API version |

<a id="reckon&#45;core&#45;v1&#45;proberequest"></a>

### reckon\.core\.v1\.ProbeRequest {#reckon-core-v1-proberequest-1}

PROBE

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `record_ids` | 1 | string | repeated | Record IDs to probe |
| `config` | 2 | [reckon\.core\.v1\.ProbeConfig](#reckon-core-v1-probeconfig) | message presence | Probe configuration |

<a id="reckon&#45;core&#45;v1&#45;proberesponse"></a>

### reckon\.core\.v1\.ProbeResponse {#reckon-core-v1-proberesponse-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `evidence` | 1 | [reckon\.core\.v1\.Evidence](#reckon-core-v1-evidence) | repeated | Evidence for each record |
| `cost` | 2 | [reckon\.core\.v1\.ProbeCostSummary](#reckon-core-v1-probecostsummary) | message presence | Probe cost summary |
| `api_version` | 3 | [reckon\.core\.v1\.CoreApiVersion](#reckon-core-v1-coreapiversion) | message presence | API version |

<a id="reckon&#45;core&#45;v1&#45;exportrunresponse"></a>

### reckon\.core\.v1\.ExportRunResponse {#reckon-core-v1-exportrunresponse-1}

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `export` | 1 | [reckon\.core\.v1\.RunExport](#reckon-core-v1-runexport) | message presence | The exported run data |

<a id="reckon&#45;core&#45;v1&#45;exportrunrequest"></a>

### reckon\.core\.v1\.ExportRunRequest {#reckon-core-v1-exportrunrequest-1}

EXPORT RUN

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `run_id` | 1 | string | implicit | Run ID to export |
| `include_events` | 2 | bool | implicit | Include events JSONL |
| `include_metrics_csv` | 3 | bool | implicit | Include metrics CSV |
| `record_scope` | 4 | [reckon\.core\.v1\.RecordScope](#reckon-core-v1-recordscope) | implicit | Which records to include |
| `max_records` | 5 | int32 | implicit | Maximum records to include \(0 \= no limit\) |
| `include_artifacts_tar` | 6 | bool | implicit | Bundle everything as tar\.gz |
| `expected_revision` | 7 | uint64 | optional | Refuse if this run has changed since the caller's previous export\. |

<a id="reckon&#45;core&#45;v1&#45;causalmemoryservice"></a>

### reckon\.core\.v1\.CausalMemoryService {#reckon-core-v1-causalmemoryservice-1}

5\-verb API \(pruning/forget is internal policy, not a verb\)

| Method | Request | Response | Description |
| --- | --- | --- | --- |
| `Write` | [reckon\.core\.v1\.WriteRequest](#reckon-core-v1-writerequest) | [reckon\.core\.v1\.WriteResponse](#reckon-core-v1-writeresponse) | Write a record to memory |
| `Retrieve` | [reckon\.core\.v1\.RetrieveRequest](#reckon-core-v1-retrieverequest) | [reckon\.core\.v1\.RetrieveResponse](#reckon-core-v1-retrieveresponse) | Retrieve records by query |
| `Observe` | [reckon\.core\.v1\.ObserveRequest](#reckon-core-v1-observerequest) | [reckon\.core\.v1\.ObserveResponse](#reckon-core-v1-observeresponse) | Observe outcome and assign credit |
| `Probe` | [reckon\.core\.v1\.ProbeRequest](#reckon-core-v1-proberequest) | [reckon\.core\.v1\.ProbeResponse](#reckon-core-v1-proberesponse) | Probe records for causal influence |
| `ExportRun` | [reckon\.core\.v1\.ExportRunRequest](#reckon-core-v1-exportrunrequest) | [reckon\.core\.v1\.ExportRunResponse](#reckon-core-v1-exportrunresponse) | Export run data for analysis |

<a id="reckon&#45;record&#45;proto"></a>

## reckon/record\.proto {#reckon-record-proto-1}

Package: `reckon`\. Syntax: `proto3`\.

Reckon Record Schema<br /> Historical wire messages for MemoryRecord and related types; the Rust kernel holds the laboratory's own model

<a id="reckon&#45;recordcost"></a>

### reckon\.RecordCost {#reckon-recordcost-1}

Resource cost of storing a record

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `bytes` | 1 | int64 | implicit | Size in bytes |
| `tokens` | 2 | int64 | implicit | Token count \(for LLM contexts\) |
| `slots` | 3 | int32 | implicit | Number of slots occupied |

<a id="reckon&#45;recordorigin"></a>

### reckon\.RecordOrigin {#reckon-recordorigin-1}

Origin and creation context of a record

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `source_op` | 1 | string | implicit | Operation that created this record |
| `input_ids` | 2 | string | repeated | IDs of records this depends on |
| `hash` | 3 | string | implicit | Content hash for deduplication \(SHA256 truncated\) |

<a id="reckon&#45;utilityestimate"></a>

### reckon\.UtilityEstimate {#reckon-utilityestimate-1}

Utility estimates for retention decisions

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `value` | 1 | double | implicit | Estimated downstream utility |
| `uncertainty` | 2 | double | implicit | Uncertainty in value estimate \(std dev\) |
| `last_used` | 3 | int64 | implicit | Last timestep this record was retrieved |
| `use_count` | 4 | int32 | implicit | Number of times retrieved |

<a id="reckon&#45;causalscore"></a>

### reckon\.CausalScore {#reckon-causalscore-1}

Causal influence estimates \(updated post\-hoc via ablation/probes\)

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `ablation_delta` | 1 | double | optional | Performance change when record is ablated |
| `counterfactual_delta` | 2 | double | optional | Performance change under counterfactual replacement |
| `influence_proxy` | 3 | double | optional | Gradient\-based or TD\-error\-based influence |
| `last_evaluated` | 4 | int64 | implicit | Timestep when causal score was last computed |

<a id="reckon&#45;memoryrecord"></a>

### reckon\.MemoryRecord {#reckon-memoryrecord-1}

A causal memory record stored by the Causal Memory Manager

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `id` | 1 | string | implicit | Unique identifier |
| `timestamp` | 2 | int64 | implicit | Step/timestep when record was created |
| `episode_id` | 3 | string | implicit | Episode or run identifier |
| `payload_type` | 4 | [reckon\.PayloadType](#reckon-payloadtype) | implicit | Type of payload |
| `payload` | 5 | bytes | implicit | The actual data \(JSON\-encoded bytes\) |
| `compressed` | 6 | bool | implicit | Whether payload is compressed |
| `encoding` | 7 | string | implicit | Compression encoding \(gzip, zstd, lz4\) |
| `cost` | 8 | [reckon\.RecordCost](#reckon-recordcost) | message presence | Resource cost |
| `tags` | 9 | string | repeated | Categorical tags |
| `provenance` | 10 | [reckon\.RecordOrigin](#reckon-recordorigin) | message presence | Origin context |
| `utility` | 11 | [reckon\.UtilityEstimate](#reckon-utilityestimate) | message presence | Utility estimates |
| `causal_score` | 12 | [reckon\.CausalScore](#reckon-causalscore) | message presence | Causal influence estimates |
| `metadata` | 13 | bytes | implicit | Additional user\-defined metadata \(JSON\-encoded\) |
| `created_at_step` | 14 | int64 | optional | Step at which this record was created \(snapshot, immutable after creation\)\.<br /> Used for derived timeline ordering only — NOT authoritative record identity\.<br /> For authoritative event ordering, use DebuggerEvent\.step instead\. |
| `created_at_sequence` | 15 | int64 | optional | Sequence number at creation time \(snapshot, immutable after creation\)\.<br /> Used for derived timeline ordering only — NOT authoritative record identity\.<br /> For authoritative event ordering, use DebuggerEvent\.sequence instead\. |

<a id="reckon&#45;payloadtype"></a>

### reckon\.PayloadType {#reckon-payloadtype-1}

Type of payload stored in a record

| Value | Number | Description |
| --- | --- | --- |
| `PAYLOAD_TYPE_UNSPECIFIED` | 0 |  |
| `PAYLOAD_TYPE_OBSERVATION` | 1 |  |
| `PAYLOAD_TYPE_ACTION` | 2 |  |
| `PAYLOAD_TYPE_REWARD` | 3 |  |
| `PAYLOAD_TYPE_CUE` | 4 |  |
| `PAYLOAD_TYPE_TRACE` | 5 |  |
| `PAYLOAD_TYPE_REPRESENTATION` | 6 |  |
| `PAYLOAD_TYPE_CUSTOM` | 7 |  |

<a id="reckon&#45;events&#45;proto"></a>

## reckon/events\.proto {#reckon-events-proto-1}

Package: `reckon`\. Syntax: `proto3`\.

Reckon Events Schema<br /> Historical wire messages for debugger and memory events; the Rust kernel holds the laboratory's own model

<a id="reckon&#45;memoryevent"></a>

### reckon\.MemoryEvent {#reckon-memoryevent-1}

A memory operation event logged by the CMM

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `event_id` | 1 | string | implicit | Unique event identifier |
| `op_type` | 2 | [reckon\.MemoryOpType](#reckon-memoryoptype) | implicit | Type of operation |
| `timestamp` | 3 | int64 | implicit | Timestamp \(step\) when event occurred |
| `episode_id` | 4 | string | implicit | Episode identifier |
| `record_id` | 5 | string | implicit | Record ID involved \(if applicable\) |
| `query_json` | 6 | string | optional | For READ: query that was executed |
| `result_ids` | 7 | string | repeated | For READ: IDs of records returned |
| `prune_reason` | 8 | string | implicit | For PRUNE: reason for pruning |
| `score_type` | 9 | string | implicit | For UPDATE\_SCORE: type of score updated |
| `old_score` | 10 | double | optional | For UPDATE\_SCORE: old value |
| `new_score` | 11 | double | optional | For UPDATE\_SCORE: new value |
| `usage_after` | 12 | [reckon\.ResourceUsage](#reckon-resourceusage) | message presence | Resource usage after this operation |
| `duration_us` | 13 | int64 | implicit | Duration of operation in microseconds |

<a id="reckon&#45;resourceusage"></a>

### reckon\.ResourceUsage {#reckon-resourceusage-1}

Current resource usage snapshot

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `slot_count` | 1 | int64 | implicit | Number of records stored |
| `byte_count` | 2 | int64 | implicit | Total bytes used |
| `token_count` | 3 | int64 | implicit | Total tokens used \(if tracked\) |
| `ops_in_window` | 4 | int32 | implicit | Operations in current window |

<a id="reckon&#45;budgetevent"></a>

### reckon\.BudgetEvent {#reckon-budgetevent-1}

Aggregate event for budget compliance

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `timestamp` | 1 | int64 | implicit | Timestamp when snapshot was taken |
| `episode_id` | 2 | string | implicit | Episode identifier |
| `budget` | 3 | [reckon\.MemoryBudget](#reckon-memorybudget) | message presence | Current budget configuration |
| `usage` | 4 | [reckon\.ResourceUsage](#reckon-resourceusage) | message presence | Current usage |
| `within_budget` | 5 | bool | implicit | Whether currently within budget |
| `slot_utilization` | 6 | double | implicit | Utilization percentage \(0\-1\) for each dimension |
| `byte_utilization` | 7 | double | implicit |  |
| `token_utilization` | 8 | double | implicit |  |

<a id="reckon&#45;debuggerevent"></a>

### reckon\.DebuggerEvent {#reckon-debuggerevent-1}

High\-level debugger event

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `event_id` | 1 | string | implicit | Unique event identifier |
| `event_type` | 2 | [reckon\.DebuggerEventType](#reckon-debuggereventtype) | implicit | Type of debugger event |
| `wall_time_ms` | 3 | int64 | implicit | Wall\-clock timestamp \(Unix millis\) |
| `episode_id` | 4 | string | implicit | Episode identifier |
| `step` | 5 | int64 | implicit | Step number within episode |
| `initial_budget` | 6 | [reckon\.MemoryBudget](#reckon-memorybudget) | optional | For EPISODE\_START: initial budget |
| `retention_policy` | 7 | [reckon\.RetentionPolicyType](#reckon-retentionpolicytype) | optional | For EPISODE\_START: retention policy type |
| `final_usage` | 8 | [reckon\.ResourceUsage](#reckon-resourceusage) | optional | For EPISODE\_END: final usage |
| `total_writes` | 9 | int64 | optional | For EPISODE\_END: total records written |
| `total_prunes` | 10 | int64 | optional | For EPISODE\_END: total records pruned |
| `memory_events` | 11 | [reckon\.MemoryEvent](#reckon-memoryevent) | repeated | For STEP: memory events in this step |
| `exceeded_dimension` | 12 | string | implicit | For BUDGET\_EXCEEDED: which dimension exceeded |
| `new_policy` | 13 | [reckon\.RetentionPolicyType](#reckon-retentionpolicytype) | optional | For POLICY\_CHANGE: new policy |
| `snapshot` | 14 | [reckon\.MemoryRecord](#reckon-memoryrecord) | repeated | For CHECKPOINT: snapshot of all current records |
| `metadata` | 15 | bytes | implicit | Additional metadata \(JSON\-encoded\) |
| `sequence` | 16 | int64 | optional | Sequence number for canonical ordering within \(episode\_id, step\)\.<br /> Monotonically increasing counter\. Part of canonical key: \(episode\_id, step, sequence\)\.<br /> Wall\-clock time \(wall\_time\_ms\) is display\-only; sequence is authoritative for ordering\. |

<a id="reckon&#45;eventstream"></a>

### reckon\.EventStream {#reckon-eventstream-1}

Stream of events for real\-time visualization

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `events` | 1 | [reckon\.DebuggerEvent](#reckon-debuggerevent) | repeated | Events in chronological order |
| `sequence` | 2 | int64 | implicit | Sequence number for this batch |
| `is_final` | 3 | bool | implicit | Whether this is the final batch |

<a id="reckon&#45;eventsubscription"></a>

### reckon\.EventSubscription {#reckon-eventsubscription-1}

Request to subscribe to events

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `episode_id` | 1 | string | implicit | Episode to subscribe to \(empty for all\) |
| `after_step` | 2 | int64 | optional | Only events after this step |
| `event_types` | 3 | [reckon\.DebuggerEventType](#reckon-debuggereventtype) | repeated | Only specific event types |
| `include_memory_events` | 4 | bool | implicit | Include memory events within debugger events |
| `batch_size` | 5 | int32 | implicit | Maximum events per batch |

<a id="reckon&#45;eventquery"></a>

### reckon\.EventQuery {#reckon-eventquery-1}

Query for historical events

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `episode_id` | 1 | string | implicit | Episode identifier |
| `from_step` | 2 | int64 | optional | Start step \(inclusive\) |
| `to_step` | 3 | int64 | optional | End step \(inclusive\) |
| `event_types` | 4 | [reckon\.DebuggerEventType](#reckon-debuggereventtype) | repeated | Only specific event types |
| `op_types` | 5 | [reckon\.MemoryOpType](#reckon-memoryoptype) | repeated | Only specific memory operation types |
| `record_id` | 6 | string | optional | Only events involving this record |
| `limit` | 7 | int32 | implicit | Maximum results |
| `offset` | 8 | int32 | implicit | Offset for pagination |

<a id="reckon&#45;eventqueryresult"></a>

### reckon\.EventQueryResult {#reckon-eventqueryresult-1}

Response to event query

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `events` | 1 | [reckon\.DebuggerEvent](#reckon-debuggerevent) | repeated | Matching events |
| `total_count` | 2 | int64 | implicit | Total count \(before limit/offset\) |
| `has_more` | 3 | bool | implicit | Whether more results exist |

<a id="reckon&#45;probecostevent"></a>

### reckon\.ProbeCostEvent {#reckon-probecostevent-1}

Probe cost tracking for causal evaluation<br /> Records compute spent on probing \(ablation, intervention tests\)

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `event_id` | 1 | string | implicit | Unique event identifier |
| `wall_time_ms` | 2 | int64 | implicit | Wall\-clock timestamp \(Unix millis\) |
| `episode_id` | 3 | string | implicit | Episode being evaluated |
| `record_id` | 4 | string | optional | Record ID being probed \(if single\-record intervention\) |
| `probe_episodes` | 5 | int32 | implicit | Number of episodes run for this probe |
| `probe_calls` | 6 | int32 | implicit | Number of task\.run invocations |
| `probe_wall_time_ms` | 7 | int64 | implicit | Total wall time spent on probe \(milliseconds\) |
| `probe_env_steps` | 8 | int64 | optional | Environment steps consumed \(if applicable\) |
| `probe_records_evaluated` | 9 | int32 | implicit | Number of records evaluated in this probe batch |
| `probe_type` | 10 | string | implicit | Type of probe \(ablation, counterfactual, etc\.\) |
| `seed` | 11 | int64 | optional | Seed used for reproducibility |

<a id="reckon&#45;probecostsummary"></a>

### reckon\.ProbeCostSummary {#reckon-probecostsummary-1}

Aggregated probe cost metrics for a complete evaluation run

| Field | Number | Type | Presence | Description |
| --- | --- | --- | --- | --- |
| `probe_episodes_total` | 1 | int64 | implicit | Total episodes used for probing |
| `probe_wall_time_total_ms` | 2 | int64 | implicit | Total wall time for all probes \(milliseconds\) |
| `probe_calls_total` | 3 | int64 | implicit | Total task\.run calls for probing |
| `probe_env_steps_total` | 4 | int64 | implicit | Total environment steps for probing |
| `probe_records_evaluated_total` | 5 | int32 | implicit | Total records evaluated |
| `probe_episodes_per_record` | 6 | double | implicit | Episodes per record evaluated \(average\) |
| `probe_wall_time_per_record_ms` | 7 | double | implicit | Wall time per record evaluated \(average, ms\) |
| `probe_overhead_ratio` | 8 | double | implicit | Overhead ratio: probe\_time / \(base\_time \+ probe\_time\) |
| `base_episodes` | 9 | int64 | implicit | Base task episodes \(non\-probe\) |
| `base_wall_time_ms` | 10 | int64 | implicit | Base task wall time \(milliseconds\) |

<a id="reckon&#45;memoryoptype"></a>

### reckon\.MemoryOpType {#reckon-memoryoptype-1}

Type of memory operation

| Value | Number | Description |
| --- | --- | --- |
| `MEMORY_OP_TYPE_UNSPECIFIED` | 0 |  |
| `MEMORY_OP_TYPE_WRITE` | 1 |  |
| `MEMORY_OP_TYPE_READ` | 2 |  |
| `MEMORY_OP_TYPE_PRUNE` | 3 |  |
| `MEMORY_OP_TYPE_UPDATE_SCORE` | 4 |  |
| `MEMORY_OP_TYPE_COMPRESS` | 5 |  |
| `MEMORY_OP_TYPE_BUDGET_CHECK` | 6 |  |

<a id="reckon&#45;debuggereventtype"></a>

### reckon\.DebuggerEventType {#reckon-debuggereventtype-1}

Type of debugger event \(higher\-level than memory events\)

| Value | Number | Description |
| --- | --- | --- |
| `DEBUGGER_EVENT_TYPE_UNSPECIFIED` | 0 |  |
| `DEBUGGER_EVENT_TYPE_EPISODE_START` | 1 |  |
| `DEBUGGER_EVENT_TYPE_EPISODE_END` | 2 |  |
| `DEBUGGER_EVENT_TYPE_STEP` | 3 |  |
| `DEBUGGER_EVENT_TYPE_POLICY_CHANGE` | 4 |  |
| `DEBUGGER_EVENT_TYPE_BUDGET_EXCEEDED` | 5 |  |
| `DEBUGGER_EVENT_TYPE_CAUSAL_UPDATE` | 6 |  |
| `DEBUGGER_EVENT_TYPE_CHECKPOINT` | 7 |  |
