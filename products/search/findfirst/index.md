# findfirst


# ![](page/agg-icon.svg) findfirst

The `findfirst` aggregation function returns the first observed non-`null` value of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `findfirst( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Usage {#usage}

To find an earliest event with respect to the `_time` field, instead use [`findearliest`](findearliest). 

You can use `findfirst` after a [`sort`](sort) or [`order`](order) operator when sorting by a non-time field. Once events are sorted, this function acts much like [`min`](min).

## Examples {#example}

Here is a basic example:

```kusto
dataset=myDataset
| summarize findfirst(channel)
```

This example effectively finds `min(status)` and `max(status)`:

```kusto {runSearch=true}
dataset="cribl_internal_logs" status=/[0-9]+/ 
| order by status asc 
| summarize x = findfirst(status), y = findlast(status)
```
