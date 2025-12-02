# sum



# ![](page/agg-icon.svg) sum

The `sum` aggregation function calculates the sum of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `sum( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Results

Returns the sum value of **Expression** across the group.

## Example

This example sums up the total number of packets sent to each combination of destination address and destination port:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| summarize totalPackets=sum(packets) by dstaddr,dstport
```


