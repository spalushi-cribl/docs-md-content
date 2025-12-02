# median



# ![](page/agg-icon.svg) median

The `median` aggregation function returns the middle value of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `median( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Example

```kusto
dataset=myDataset
| summarize median(age)
```

