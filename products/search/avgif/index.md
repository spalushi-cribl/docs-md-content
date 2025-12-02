# avgif



# ![](page/agg-icon.svg) avgif

The `avgif` aggregation function calculates the average of **Expression** across the group where **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `avgif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Records with `null` values are ignored and not included in the calculation. Wildcards are not supported for field names.
* **Predicate**: Predicate that if `true`, the **Expression** calculated value will be added to the average.

## Results

Returns the average value of **Expression** across the group where Predicate evaluates to `true`.

{#example}
## Examples

This example returns average byte count from source address `10.0.0.164`:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| limit 1000 
| summarize AverageSessionBytesForSrcAddr=avgif(bytes, srcaddr=="10.0.0.164")
```

This example returns average byte count by private source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| limit 1000 
| summarize AverageSessionBytesForPrivateAddr=avgif(bytes, ipv4_is_private(srcaddr))
```


