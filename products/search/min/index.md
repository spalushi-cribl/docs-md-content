# min



# ![](page/agg-icon.svg) min

The `min` aggregation function finds the minimum value across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `min( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Results

Returns the minimum value of **Expression** across the group.

Numeric values are considered before alpha or alphanumeric values.

## Example

This example summarizes the minimum traffic, in bytes, for each source port:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| limit 1000 
| summarize min(bytes) by srcport
```


