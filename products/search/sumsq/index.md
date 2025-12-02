# sumsq



# ![](page/agg-icon.svg) sumsq

The `sumsq` aggregation function returns the sum of squares of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `sumsq( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Example

```kusto
dataset=myDataset
| summarize sumsq(goats)
```

