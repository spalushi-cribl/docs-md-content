# avg


# ![](page/agg-icon.svg) avg

The `avg` aggregation function calculates the average (arithmetic mean) across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `avg( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Records with `null` values are ignored and not included in the calculation. Wildcards are not supported for field names.

## Results

Returns the average value of **Expression** across the group.

{#example}
## Examples

This example summarizes average byte count across the specified number of events:

```kusto {runSearch=true}
dataset="cribl_search_sample" | limit 1000 | summarize AverageSessionBytes = avg(bytes)
```

This example summarizes average byte count, and corresponding (small-sample) standard deviation, by source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" | summarize avg(bytes), stdev(bytes) by srcaddr
```

This example summarizes average byte count, and two measures of variance (sample- and population-based), by source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" 
| summarize avg(bytes), variance(bytes), variancep(bytes) by srcaddr
```


