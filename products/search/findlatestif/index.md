# findlatestif



# ![](page/agg-icon.svg) findlatestif

The `findlatestif` aggregation function returns the latest value (based on `_time`) of **Expression** across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `findlatestif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

## Example

This example returns the latest date for all products that have more than 4 letters.

```kusto
dataset=myDataset
| summarize findlatestif(date, strlen(product) > 4)
```

