# findlast


# ![](page/agg-icon.svg) findlast

The `findlast` aggregation function returns the last observed non-`null` value of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `findlast( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Usage {#usage}

To find a latest event with respect to the `_time` field, instead use [`findlatest`](findlatest). 

You can use `findlast` after a [`sort`](sort) or [`order`](order) operator when sorting by a non-time field. Once events are sorted, this function acts much like [`max`](max).

## Examples {#example}

Here is a basic example:

```kusto
dataset=myDataset
| summarize findlast(channel)
```

This example effectively finds `min(status)` and `max(status)`:

```kusto {runSearch=true}
dataset="cribl_internal_logs" status=/[0-9]+/ 
| order by status asc 
| summarize x = findfirst(status), y = findlast(status)
```
