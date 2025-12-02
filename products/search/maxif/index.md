# maxif



# ![](page/agg-icon.svg) maxif

The `maxif` aggregation function calculates the maximum value across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `maxif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

## Results

Returns the maximum value of **Expression** across the group for which **Predicate** evaluates to `true`.

Numeric values are considered before alpha or alphanumeric values.

## Example

This example returns the maximum byte count for source address `10.0.0.33`:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| limit 1000 
| summarize maxif(bytes,srcaddr=="10.0.0.33")
```


