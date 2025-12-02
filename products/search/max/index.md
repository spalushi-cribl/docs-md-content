# max


# ![](page/agg-icon.svg) max

The `max` aggregation function finds the maximum value across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `max( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Results

Returns the maximum value of **Expression** across the group.

Numeric values are considered before alpha or alphanumeric values.

## Example

This example summarizes the maximum traffic, in bytes, for each source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" 
| summarize maxBytes=max(bytes) by srcaddr
```


