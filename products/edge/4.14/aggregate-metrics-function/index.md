# Aggregate Metrics


The Aggregate Metrics Function computes aggregate statistics for metrics and metric events. It can perform common aggregations such as count, sum, average, min, max, median, rate, and more. This Function helps you pre-aggregate your metrics data before sending it to downstream storage or analysis tools to reduce storage needs and costs. It also simplifies large datasets by reducing complexity and improving query efficiency.

Use this Function to:

- **Reduce high-cardinality data:** Consolidate datasets with a large number of unique or uncommon values to improve query performance and save money on storage or processing costs.
- **Create time-based rollups:** Aggregate high-frequency metrics into larger time intervals (such as hourly or daily) to simplify analysis and improve long-term data retention.
- **Normalize metrics from multiple sources:** Aggregate metrics collected from different systems into a single, unified view.
- **Combine related or duplicate metrics:** Merge related metrics together to create a unified metric.
- **Downsample metrics:** Retain long-term trends while reducing the volume of detailed data.

## How the Aggregate Metrics Function Works

The Aggregate Metrics Function processes data in the following order:

1. **Filter:** Checks incoming data for metrics or metric events that match the specified filter values.

1. **Aggregate:** Combines the applicable metrics or metric events based on the configuration settings.

1. **Output:** Extracts, formats, and outputs the aggregated values.

When using this Function, keep these considerations in mind:

- The default metric type is `Automatic`, which allows the aggregation function to determine the appropriate output type.
- Not all methods for aggregating metrics (count, sum, average, and so forth) can convert aggregate data into a certain metrics types. For example, histograms can only convert to other histograms. See [Metric Types](#metric-types) for available types, recommendations, and supported aggregation functions.

> The Aggregate Metrics Function handles metrics data only. In this context, this refers to events with the internal field `__criblMetrics`. If you need to aggregate other types of data, such as logs and traces, use the [Aggregations](aggregations-function) Function instead. If the Aggregate Metrics Function receives non-metric data that do not contain the `__criblMetrics` internal field, it will pass through unchanged without any effect.
>
{.box .info}

## Configure the Aggregate Metrics Function

To simplify configuration, the Pipeline Data Preview pane can automate most of the configuration process for the Aggregate Metrics Function. When you build or edit a Pipeline with metrics data, the Data Preview pane automatically detects metrics in your sample data and switches to the Metrics View, which organizes metrics by name. From this view, you can select the metrics you want to aggregate, and Cribl Edge will automatically add an Aggregate Metrics Function to your Pipeline with an initial configuration based on your selection. See [Manage Metrics and High Cardinality](manage-metrics) for more information.

To configure this Function manually:

1. Add a Source and Destination to begin generating metrics. See [Integrations](integrations) for more information.

1. Add a new Pipeline or open an existing Pipeline. Add or capture sample data test your Pipeline as you build it. See [Pipelines](pipelines) for more information.

1. At the top of the Pipeline, select **Add Function** and search for `Aggregate Metrics`, then select it.

1. In the **Aggregate Metrics** modal, configure the following general settings:

   - **Filter:** A JavaScript filter expression that selects which metrics to process through the Function. Defaults to `true`, meaning it evaluates all events.
     - If you don't want to use the default setting `true`, you can use a JavaScript expression to apply this Function to specific metric events. See [Usage Examples: Filter Expressions](#filter-expressions) for examples.
     - If you use the Metrics View interface in the Pipeline Data Preview to add this Function to your Pipeline, Cribl Edge automatically builds a filter expression based on the metrics you selected. See [Manage Metrics and High Cardinality](manage-metrics) for more information.
   - **Description:** A brief description of how this Function modifies your metrics to help other users understand its purpose later. Defaults to empty.
     - If you use the Metrics View interface in the Pipeline Data Preview to add this Function to your Pipeline, Cribl Edge automatically generates a description based on the selected metrics, including usage tips.  Review the generated description and adjust it to better reflect your use case. See [Manage Metrics and High Cardinality](manage-metrics) for more information.
   - **Final:** When enabled, stops data from continuing to downstream Functions for additional processing. Default: Off. Toggle **Final** on if Aggregate Metrics is the last Function in your Pipeline.
   - **Time window:** The duration of the tumbling window for aggregating events. Must be a valid time string (such as `10s`) and match the pattern `\d+[sm]$`. See [Time Window Settings](#time-window-settings) for additional settings.
   - **Aggregates**: A table that defines how to aggregate specific metric events. If you use the Metrics View interface in Pipeline Data Preview to add this Function to your Pipeline, Cribl Edge automatically populates this field. See [Manage Metrics and High Cardinality](manage-metrics) for more information. The table includes two inputs:
     - **Metric type**: Specifies the type of metric output to apply. Defaults to `Automatic`. Be aware not all aggregation functions can convert aggregate data into a certain metrics types. For example, histograms can only convert to other histograms. See [Metric Types](#metric-types) for available types, recommendations, and supported functions.
     - **Aggregration:** Enter a JavaScript expression that performs the desired aggregation function (such as count, average, sum, and more). See [Usage Examples: Aggregation Function Expressions](#aggregation-function-expressions) for a full list of available aggregation functions and usage examples.
   - **Group by dimensions**: Defines one or more dimensions to group aggregates by. List excluded dimensions first, followed by the dimensions to group by. This field is not a JavaScript expression. Instead it uses a lightweight syntax that accepts simple strings and operators, including wildcard characters. Accepted formats:

   | Syntax | Description |
   | -------| ----------- |
   | `!string` | Exclude the specified dimension or field from the grouping. Example: `!proc`. |
   | `string` | Include the specified dimension or field in the grouping. Example: `proc`. |
   | `!*` | Exclude all dimensions or fields from the grouping. Wildcard characters are not supported for dimensions or fields with dotted field names, such as StatsD and Open Telemetry formats.  |
   | `*` | Include all dimensions or fields in the grouping.<br><br>**NOTE:** Use with caution. The wildcard character cannot search or filter for any dimensions or fields that use dot notation, such as `dimension.name` or `field.name`. StatsD and Open Telemetry use dot notation formats. |
   | `string*` or `"string*"` | Include all dimensions or fields that begin with this string pattern.<br><br>**NOTE:** To search or filter for dimensions or fields that use dot notation, enclose the string in quotation marks. Example: `"os.name"` or `"os.version"`. StatsD and Open Telemetry use dot notation formats. |

   > Using the wildcard character `*` includes all dimensions in the aggregation, which can increase cardinality and lead to higher memory usage. To avoid this impact, first exclude dimensions that may cause high cardinality, then introduce wildcards, if needed. For example: `!_time, !_numericValue, *`
   >
   {.box .warning}

   - **Evaluate fields**: A set of key-value pairs to evaluate and add or update. The system adds fields in the context of an aggregated event before sending them out. This setting does not apply to passthrough events.

1. Configure additional settings as needed. For more information, see:

   - [Time Window Settings](#time-window-settings)
   - [Output Settings](#output-settings)
   - [Advanced Settings](#advanced-settings)

1. Test that you configured the Function correctly by comparing sample incoming data with outgoing data in the Data Preview pane and Pipeline Diagnostics tool. See [Data Preview](data-preview) and [Metrics View](data-preview#metrics-view) for more information.

### Time Window Settings {#time-window-settings}

**Cumulative aggregations**: When enabled, retains cumulative aggregation values when flushing an aggregation table event. When disabled (default), resets aggregation values to `0` on flush.

**Lag tolerance**: Specifies the tolerance for late events in a tumbling window. Must be a valid time string (such as `10s`) and match the pattern `\d+[sm]$`.

**Idle bucket time limit**: The amount of time to wait before flushing a bucket that has not received events. Must be a valid time string (such as `10s`) and match the pattern `\d+[sm]$`.

### Output Settings {#output-settings}

**Passthrough mode**: When enabled, passes the original events along with the aggregation events. Default: Off.

**Sufficient stats mode**: Controls whether to output *only* the statistics required for the supplied aggregations. When disabled (default), outputs richer statistics.

**Preserve group by fields**: Controls whether to retain the original aggregation event’s group by field structure.

**Output prefix**: Adds an optional prefix to all fields output by the Aggregate Metrics Function. Use a prefix to modify the metric name, making it easier to identify and search for aggregated metrics. A prefix helps distinguish aggregated metrics from non-aggregated ones, allowing you to route or search for them more easily without increasing cardinality.

### Advanced Settings {#advanced-settings}

**Aggregation event limit**: The maximum number of events to include in a single aggregation event. Defaults to unlimited. Minimum value: `1`.

**Aggregation memory limit**: The maximum memory that aggregations can use. Defaults to unlimited (the amount of memory available in the system). Accepts numerals with units such as KB, MB, and GB. Example: `4GB`.

**Treat dots as literals:** Toggle on (default) if your fields or dimensions use dot notation, which are used in StatsD and Open Telemetry metrics formats. Examples: `os.name` or `os.version`. If toggled off, metrics or fields using dot notation are enclosed in quotation marks and treated as strings. Example outputs when toggled off: `"os.name"` or `"os.version"`. Use the [Data Preview](data-preview) to confirm that the data output matches your expectations.

**Flush on stream close**: When enabled (default), flushes aggregations when an input stream closes. When disabled, the [Time Window Settings](#time-window-settings) control flush behavior. Disabling this setting may be preferable in cases such as:

- Your input data consists of many small files.
- You are sending data to Prometheus. Enabling Flush on stream close can cause Prometheus to receive multiple aggregations from the same Worker Process for the same time period. Prometheus cannot distinguish between them and will ingest only the first one.

## Metric Types {#metric-types}

This table explains the differences between the metric types and which aggregration functions they support:

| Type | Description |
| ---- | ----------- |
| Automatic | Default. Select this type to allow the aggregation function determine the appropriate metric output type. |
| Counter | A counter is a cumulative metric that increases over time. It represents a value that only increments or resets to zero. Counters never decrease unless the system restarts or the counter is manually reset. Use this metric type to monitor changes over time where you need to compute the rate of increase. If you need to capture the current value at the time of measurement rather than a cumulative value, use the gauge metric type instead.<br><br>Supports aggregation for these functions: <ul><li>count (recommended for the counter metric type)</li><li>avg</li><li>distinct_count</li><li>dc</li><li>earliest</li><li>latest</li><li>max</li><li>max</li><li>min</li><li>median</li><li>mode</li><li>perc</li><li>per_second</li><li>rate</li><li>stdev</li><li>stdev</li><li>stdevp</li><li>sum</li><li>sumsq</li><li>summary</li><li>variance</li><li>variancep</li></ul> |
| Distribution | **NOTE:** Available for Datadog only.<br><br>A distribution is a collection of related measurements that capture the spread or variation of data points over time. Instead of tracking a single value, it records a set of values, allowing you to calculate statistics like mean, median, percentiles, and standard deviation. Use this metric type to identify patterns and outliers in performance.<br><br>Supports aggregation for these functions: <ul><li>avg</li><li>count</li><li>distinct_count</li><li>dc</li><li>earliest</li><li>latest</li><li>first</li><li>histogram</li><li>last</li><li>max</li><li>min</li><li>median</li><li>mode</li><li>perc</li><li>per_second</li><li>rate</li><li>stdev</li><li>stdevp</li><li>sum</li><li>sumsq</li><li>summary</li><li>variance</li><li>variancep</li></ul> |
| Gauge | A gauge represents a single value at a specific point in time. Unlike a counter, a gauge can increase or decrease to reflect the current state rather than its cumulative value. Use this metric type to provide a snapshot of a value at a particular moment.<br><br>Supports aggregation for these functions: <ul><li>sum (recommended for the gauge metric type)</li><li>avg</li><li>count</li><li>distinct_count</li><li>dc</li><li>earliest</li><li>latest</li><li>first</li><li>histogram</li><li>last</li><li>max</li><li>median</li><li>min</li><li>mode</li><li>per_second</li><li>perc</li><li>rate</li><li>stdev</li><li>stdevp</li><li>sumsq</li><li>summary</li><li>variance</li><li>variancep</li></ul> |
| Histogram | A histogram measures the distribution of a set of values over time. It tracks both the count of events and how those events are distributed across a range of values. Use this metric type to measure the frequency of values falling within predefined ranges. Histograms can be useful for identifying outliers and performance bottlenecks.<br><br>Supports aggregation for histograms only. |
| Summary | A summary is similar to a histogram but computes specific quantiles over time (such as median or 90th percentile)instead of counting values in predefined ranges. Use this metric type to capture statistical trends in specific statistical measures over time.<br><br>Supports aggregation for summary only. |
| Timer | A timer measures the duration of an event from start to finish. It records how long an operation takes and allows you to calculate statistics like min, max, average, and percentiles over time. Use this metric type when when your focus is specifically on measuring elapsed time.<br><br>Supports aggregation for these functions: <ul><li>avg</li><li>count</li><li>distinct_count</li><li>dc</li><li>earliest</li><li>latest</li><li>first</li><li>histogram</li><li>last</li><li>max</li><li>median</li><li>min</li><li>mode</li><li>perc</li><li>per_second</li><li>rate</li><li>stdev</li><li>stdevp</li><li>sum</li><li>sumsq</li><li>summary</li><li>variance</li><li>variancep</li></ul> |

## Usage Examples

This section provides typical JavaScript expressions that you might commonly use with the Aggregate Metrics Function.

### Filter Expressions {#filter-expressions}

The **Filter** setting is a JavaScript expression that selects which data to process through the Aggregate Metrics Function. The following is an example of a JavaScript expression that filters for three metrics named `proc.cpu_perc`, `proc.mem_perc`, and `proc.bytes_in` and other variants that may include these names:

```
(_metric == 'proc.cpu_perc' || __criblMetrics[0].nameExpr.includes("'proc.cpu_perc'")) ||
(_metric == 'proc.mem_perc' || __criblMetrics[0].nameExpr.includes("'proc.mem_perc'")) ||
(_metric == 'proc.bytes_in' || __criblMetrics[0].nameExpr.includes("'proc.bytes_in'"))
```

### Aggregation Function Expressions {#aggregation-function-expressions}

The **Aggregates** table in the Aggregate Metric Function defines how to aggregate specific metric events. You can use the **Aggregration** column in this table to enter a JavaScript expression that performs the desired aggregation function. The Aggregate Metrics Function shares much of its functionality with the Aggregations Function. For more detailed examples, see the [Aggregations Function](aggregations-function#aggregation-functions) documentation.

This table provides examples of JavaScript expressions for common aggregation functions for the Aggregate Metrics Function:

| Aggregation | Example |
| ----------- | ------- |
| Average | `avg(_value \|\| proc.mem_perc).as(proc.mem_perc_avg)` |
| Count | `count(_value \|\| proc.bytes_in).as(proc.bytes_in_count)` |
| Sum | `sum(_value \|\| proc.mem_perc).as(proc.mem_perc_sum)` |

> Cribl Edge places the output of the aggregate in a field `<fieldName>_<aggFunction>`. If a conflict occurs, the last aggregate takes precedence. To avoid this conflict, use `as(fieldName)` to rename the imported field.
>
{.box .info}
