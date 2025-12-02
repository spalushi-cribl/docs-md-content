# findearliest


# ![](page/agg-icon.svg) findearliest

The `findearliest` aggregation function returns the earliest value (based on `_time`) of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `findearliest( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

{#example}
## Examples

Return the earliest event by `_time`:

```kusto {runSearch=true}
dataset="cribl_search_sample"
| summarize findearliest(_time)
```

Return the earliest event by a custom field's value:

```kusto
dataset=myDataset
| summarize findearliest(receiptTime)
```
