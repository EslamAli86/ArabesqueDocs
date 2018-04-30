# Configuration paramters

This document describes the paramaters used to configure Arabesque and QFrag. It also describes the perameters used to configure the spark cluster


***


###  Cluster:
| Parameter | Describtion | Default value |
| ------------------- | -------------- | :--------------: |
| **`spark_master`** | This property determines the deploy mode of the application. [Allowed values](https://spark.apache.org/docs/latest/submitting-applications.html#master-urls) are the same as in any Spark application. | `local[*]` |
| **`num_workers`** | Number of Spark executors requested by the application. | `1` |
| **`num_compute_threads`** | Number of cores per executor requested by the application | `1` |
| **`num_partitions`** | The number of parallel execution engines used in Arabesque. It should not be less than the number of cores available in the cluster. | [default parallelism](https://spark.apache.org/docs/latest/configuration.html#execution-behavior) in the SparkContext |
| **`driver_memory`** | sets the desired memory for the driver | `1g` |
| **`executor_memory`** | sets the desired memory for each executor | `1g` |
| **`max_result_size`** | determines the maximum size of the serialized results that will be returned to the driver from each task (Note: this value should be less than the driver memory or it will cause OOM errors) | `1g` |


***


###  Common paramters:
| Parameter | Describtion | Default value |
| ------------------- | -------------- | :--------------: |
| **`system_type`** | determines the system: either **`mining`** to run Arabesque or **`search`** to run QFrag | `mining` |
| **`log_level`** | determines the log level of the application, you can choose one of the [log4j logging levels]() | `info` |


***


###  Arabesque:
| Parameter | Describtion | Default value |
| ------------------- | -------------- | :--------------: |
| **`execution_engine`** | The main interface for the spark master engines | no default value and must be provided |
| **`input_graph_path`** | The main interface for the spark master engines | `main.graph` |
| **`input_graph_subgraphs_path`** | The main interface for the spark master engines | `None` |
| **`input_graph_local`** | The main interface for the spark master engines | `false` |
| **`output_path`** | The main interface for the spark master engines | `Output` |
| **`output_active`** | The main interface for the spark master engines | `true` |
| **`computation`** | The main interface for the spark master engines | `io.arabesque.computation.ComputationContainer` |
| **`master_computation`** | The main interface for the spark master engines | `io.arabesque.computation.MasterComputation` |
| **`optimizations`** | This parameter is optional | no default value |
| **`arabesque.clique.maxsize`** | The main interface for the spark master engines | `4` |
| **`arabesque.motif.maxsize`** | The main interface for the spark master engines | `4` |
| **`arabesque.fsm.maxsize`** | The main interface for the spark master engines | `4` |
| **`arabesque.fsm.support`** | The main interface for the spark master engines | `Integer.MAX_VALUE` |
| **`comm_strategy`** | The communication strategy used to re-distribute the embedding on each superstep. The following values are currently supported: <ul><li><code>odag_sp</code>: ODAGs are used to pack embeddings in an space-efficient structure</li><li><code>embedding</code>: the embedding are packed with a common compression algorithm (LZ4).</li></ul> | `odag_sp` |
| **`flush_method`** | This property is required when `comm_strategy` is `odag_sp`. In particular, we aggregate the ODAGs according to one of the following criteria: <ul><li><code>flush_by_pattern</code>: patterns are used as aggregation key. This is a good alternative when the number of instance per pattern is roughly uniform, which is very rare.</li><li><code>flush_by_entry</code>: every entry (pattern,domainId,wordId) in the ODAG is used as a composite key for aggregation. This is efficient when the distribution of instances per pattern is irregular but the number of domains is small.</li><li><code>flush_by_parts</code>: ranges of domains in the ODAG are used as key for aggregation. This is efficient for irregular distributions of instances among patterns.</li></ul> | `flush_by_parts` |
| **`num_odag_parts`** | The number of parts used to split the ODAG for aggregation when the communication strategy is `odag_sp` and the flush method is `flush_by_parts` | `-1` |
| **`arabesque.aggregators.default_splits`** | Split all aggregations in parts for parallel aggregation. *use only with heavy aggregations*. | `1` |
| **`incremental_aggregation`** | Merges or replaces the aggregations for the next superstep. We can have one of the following scenarios:<ul><li> `true`: In any superstep we are interested in all aggregations seen so far. Thus, the aggregations are incrementally composed </li><li> `false`: In any superstep we are interested only in the previous aggregations. Thus, we discard the old aggregations and replace it with the new aggregations for the next superstep </li></ul> | `false` |


***


###  QFrag:
| Parameter | Describtion | Default value |
| ------------------- | -------------- | :--------------: |
| **`search_input_graph_path`** | bla bla bla | `null` |
| **`search_query_graph_path`** | bla bla bla | `null` |
| **`search_output_path`** | bla bla bla | `output_search` |
| **`search_output_active`** | bla bla bla | `true` |
| **`search_injective`** | bla bla bla | `false` |
| **`search_outliers_pct`** | bla bla bla | `0.001` |
| **`search_outliers_min_matches`** | bla bla bla | `100` |
| **`search_fastNeighbors`** | bla bla bla | `true` |
| **`search_write_in_binary`** | bla bla bla | `false` |
| **`search_num_vertices`** | bla bla bla | `-1` |
| **`search_num_edges`** | bla bla bla | `-1` |
| **`search_num_labels`** | bla bla bla | `-1` |
