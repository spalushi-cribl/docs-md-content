# sumsqif



# ![](page/agg-icon.svg) sumsqif

The `sumsqif` aggregation function returns the sum of squares of **Expression** across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `sumsqif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

## Example

```kusto
dataset=myDataset
| summarize sumsqif(goats, host=="Cribl.local")
```

