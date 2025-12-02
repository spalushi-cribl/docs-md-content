# findearliestif


# ![](page/agg-icon.svg) findearliestif

The `findearliestif` aggregation function returns the earliest value (based on `_time`) of **Expression** across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `findearliestif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

{#example}
## Examples

This example returns the 10 earliest events with an `ACCEPT` action:

```kusto {runSearch=true}
dataset="cribl_search_sample" | limit 10
| eventstats startTime=findearliestif(_time, action=="ACCEPT")
| extend timeDelta=_time-startTime
```

This example returns the earliest date for all products that have more than 4 letters.

```kusto
dataset=myDataset
| summarize findearliestif(date, strlen(product) > 4)
```
