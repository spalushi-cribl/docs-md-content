# stdevif


# ![](page/agg-icon.svg) stdevif

The `stdevif` aggregation function calculates the stdev of **Expression** across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `stdevif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: When `true`, the **Expression** calculated value will be added to the standard deviation.

## Results

Returns the standard deviation of **Expression** across the group for which **Predicate** evaluates to `true`.

## Example

This example summarizes standard deviations in byte count, by source address, for events whose destination port is higher than `1024`:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" 
| summarize stdevif(bytes, dstport>1024) by srcaddr
```


