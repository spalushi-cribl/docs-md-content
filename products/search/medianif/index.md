# medianif



# ![](page/agg-icon.svg) medianif

The `medianif` aggregation function returns the middle value of **Expression** across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `medianif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

## Example

```kusto
dataset=myDataset
| summarize medianif(data, host=="Cribl.local")
```

