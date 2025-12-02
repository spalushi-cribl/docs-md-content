# Drop Dimensions


The Drop Dimensions Function reduces the number of unique dimension combinations in outgoing metrics data to help address high cardinality. Use it to improve query performance and reduce storage costs by removing unnecessary dimensions.

This Function creates new aggregated events for the dropped dimensions. By dropping selected dimensions, you can reduce the number of unique time series, allowing you to group together incoming metrics and and roll them up into a single event. It merges frequently generated metrics into more manageable time windows, which creates smaller, coarser-grained datasets for downstream systems.

## How the Drop Dimensions Function Works

The Drop Dimensions Function processes data in the following order:

1. **Filter:** Checks incoming data for events that match the specified filter values. You can filter by metric dimensions or any field on the incoming event.

1. **Drop:** Removes the specified dimensions from those events.

1. **Aggregate:** Combines all events with the same remaining fields down into a new aggregated event.

1. **Output:** Extracts, formats, and outputs the aggregated result as a new event.

### Considerations

When using this Function, keep these considerations in mind:

- This Function does not perform any conversions from one metric type to another.
- Cribl Edge does not support aggregation of histogram, summary, or distribution metric types. The Function handles these metrics in special ways:
  - **Only unsupported metrics:** If an event only contains one of these unsupported metric types (histogram, distribution, or summary), these metrics pass through the Function unchanged.
  - **Mixed metrics:** If an event includes both supported and unsupported metric types (including histograms, distributions, or summaries), Cribl Edge splits the event into multiple events, each containing a single metric. These events are then processed by the Function normally.
- Cribl Edge creates new aggregated events for each supported metric. When you drop dimensions, it reduces the number of unique time series, enabling more events to be grouped and rolled up together. The result is a set of new, aggregated events that merge frequent metrics into more manageable time windows. Each new event includes only the fields specified in `__criblMetrics`:
  - A field defined by `nameExpr`, where the value of the referenced field becomes the new field name. For example, if `nameExpr: ['_metric']` and the event has `_metric: 'cpu.usage'`, the output event will contain `cpu.usage: 1`.
  - A value field as defined in the values array, where the referenced field provides the value of the new metric field. For example, if `values: ['_value']` and the event has `_value: 1`, then `1` will be the value for `cpu.usage`.
  - Any matching dimension fields retained from the original event.

> While `nameExpr` usually references a field (such as `_metric`), it can also be an expression. In such cases, Cribl Edge uses the evaluated result of the expression as the output field name.
>
{.box .info}

Any fields that are on the event but not specified here will not be present on the outputted event.

### Memory Usage and Cardinality

The Drop Dimensions Function processes metrics by grouping and aggregating data based on dimension combinations. This can lead to increased memory usage during processing if you are processing high-cardinality datasets where many unique combinations of metric names and dimensions exist.

To help identify and troubleshoot potential memory issues, the Function logs the following diagnostic values:

| Diagnostic value | Description |
| ---------------- | ----------- |
| `numUniqueMetricAndDims` | The number of unique combinations of metric names and dimensions currently being tracked. |
| `unFlushedEventCount` | The number of events still waiting to be aggregated based on their new (post-drop) dimension list. |
| `totalBytesTotal` | The total memory (in bytes) currently used by the Function to hold and process data. |

If you observe unusually high values for `numUniqueMetricAndDims` or `totalBytesTotal`, this may be a sign of excessive cardinality. In extreme cases, high cardinality may cause the Drop Dimensions Function to exceed the memory limits of the Worker Processes. In these cases, consider dropping additional dimensions to reduce the number of unique time series generated.

## Configure the Drop Dimensions Function



To configure this Function manually:

1. Add a Source and Destination to get data flowing. See [Integrations](integrations) for more information.

1. Add a new Pipeline or open an existing Pipeline. Add sample data or capture sample data to test your Pipeline as you build it. See [Pipelines](pipelines) for more information.

1. At the top of the Pipeline, select **Add Function** and search for `Drop Dimensions`, then select it.

1. In the **Drop Dimensions** modal, configure the following general settings:

   - **Filter:** A JavaScript filter expression that selects which metrics to process through the Function. Defaults to `true`, meaning it evaluates all events.  If you don't want to use the default, you can use a JavaScript expression to apply this Function to specific metric events. See [Usage Examples: Filter Expressions](#filter-expressions) for examples.
   - **Description:** A brief description of how this Function modifies your metrics to help other users understand its purpose later. Defaults to empty. 
   - **Final:** When enabled, stops data from continuing to downstream Functions for additional processing. Default: Off. Toggle **Final** on if Drop Dimensions is the last Function in your Pipeline.
   - **Aggregation time window:** The duration of the tumbling window for aggregating events. Must be a valid time string (such as `10s`) and match the pattern `\d+[sm]$`.
   - **Dimensions to drop:** Add or edit the list of dimension names to drop. This field is not a JavaScript expression. Instead it uses a lightweight syntax that accepts simple strings and operators, including wildcard characters. For most dimension names, you can use the syntax `'dimensionName'` to access dimension names. See [Usage Examples: Drop Dimensions](#drop-dimensions) for examples. Accepted formats:

   | Syntax | Description |
   | -------| ----------- |
   | `!string` | Exclude the specified dimension or field from the grouping. Example: `!proc`. |
   | `string` | Include the specified dimension or field in the grouping. Example: `proc`. |
   | `!*` | Exclude all dimensions or fields from the grouping. |
   | `*` | Include all dimensions or fields in the grouping.<br><br>**NOTE:** Use with caution. The wildcard character cannot search or filter for any dimensions or fields that use dot notation, such as `dimension.name` or `field.name`. StatsD and Open Telemetry use dot notation formats. |
   | `string*` or `"string*"` | Include all dimensions or fields that begin with this string pattern. |

   - **Flush on stream close:** When enabled (default), flushes aggregations when an input stream closes. When disabled, the **Aggregation time window** controls flush behavior. Disabling this setting may be preferable in cases such as:
     - Your input data consists of many small files.
     - You are sending data to Prometheus. Enabling Flush on stream close can cause Prometheus to receive multiple aggregations from the same Worker Process for the same time period. Prometheus cannot distinguish between them and will ingest only the first one.

1. Test that you configured the Function correctly by comparing sample incoming data with outgoing data in the Data Preview pane and Pipeline Diagnostics tool. See [Data Preview](data-preview) and [Metrics View](data-preview#metrics-view) for more information.

## Usage Examples

This section provides typical JavaScript expressions that you might commonly use with the Drop Dimensions Function.

### Filter Expressions {#filter-expressions}

The **Filter** setting is a JavaScript expression that selects which data to process through the Drop Dimensions Function. The following is an example of a JavaScript expression that filters for a metric named `proc.cpu_perc` that contains the dimensions `proc`, `pie`, and `unit` and other variants that may include these dimension names:

```
(_metric == 'proc.cpu_perc' || __criblMetrics[0].nameExpr.includes("'proc.cpu_perc'")) && (__criblMetrics[0].dims.includes("proc")) ||
(_metric == 'proc.cpu_perc' || __criblMetrics[0].nameExpr.includes("'proc.cpu_perc'")) && (__criblMetrics[0].dims.includes("pie")) ||
(_metric == 'proc.cpu_perc' || __criblMetrics[0].nameExpr.includes("'proc.cpu_perc'")) && (__criblMetrics[0].dims.includes("unit"))
```

### Drop Dimensions {#drop-dimensions}

You can use the **Drop dimension** setting to add or edit the list of dimension names to drop. This field is not a JavaScript expression. Instead it uses a lightweight syntax that accepts simple strings and operators, including wildcard characters.

For most dimension names, you can use the syntax `'dimensionName'` to access dimension names. For example:

```
'proc'
'pie'
'unit'
'!proc'
'proc*'
'unit*'
'!*'
'*'
```
