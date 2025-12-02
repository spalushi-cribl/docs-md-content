# minif


# ![](page/agg-icon.svg) minif

The `minif` aggregation function calculates the minimum value across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `minif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

## Results

Returns the minimum value of **Expression** across the group for which **Predicate** evaluates to `true`.

Numeric values are considered before alpha or alphanumeric values.

{#example}
## Examples

This example returns the minimum byte count for source address `10.0.0.33`:

```kusto {runSearch=true}
dataset="cribl_search_sample" | limit 1000 | summarize minif(bytes,srcaddr=="10.0.0.33")
```

This example summarizes minimum byte counts by private source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| limit 1000 
| summarize minif(bytes, ipv4_is_private(srcaddr)) by srcaddr
```


