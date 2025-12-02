# varianceif



# ![](page/agg-icon.svg) varianceif

The `varianceif` aggregation function calculates the [variance](variance) of **Expression** for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `varianceif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Predicate that if `true`, the **Expression** calculated value will be added to the variance.

## Results

Returns the variance value of **Expression** across the group for which **Predicate** evaluates to `true`.

## Example

This example summarizes the variance in byte count, by source address, for events whose destinatino port is higher than `1024`:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" 
| summarize varianceif(bytes, dstport>1024) by srcaddr
```


