# findlatest



# ![](page/agg-icon.svg) findlatest

The `findlatest` aggregation function returns the latest value (based on `_time`) of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `findlatest( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Example

```kusto
dataset=myDataset
| summarize findlatest(receiptTime)
```

