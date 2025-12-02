# sumif



# ![](page/agg-icon.svg) sumif

The `sumif` aggregation function calculates the sum of **Expression** for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `sumif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

## Results

Returns the sum of **Expression** across the group for which **Predicate** evaluates to `true`.

## Examples

This example sums up the number of packets sent to destination address `10.8.20.30` through each destination port:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| summarize packetsToBob=sumif(packets, dstaddr=="10.8.20.30") by dstport 
```


