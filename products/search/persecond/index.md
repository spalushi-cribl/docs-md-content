# persecond



# ![](page/agg-icon.svg) persecond

The `persecond` aggregation function returns the per second rate (based on `_time`) of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `persecond( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Example

```kusto
dataset=myDataset
| summarize persecond(transactions)
```

