# countif


# ![](page/agg-icon.svg) countif

The `countif` aggregation function counts events based on a predicate.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

> If you need to count non-null values regardless of truthiness, use the [`count`](count) aggregation function.
>
{.box .info}

## Syntax

    `countif( Predicate )`

### Arguments

* **Predicate**: An expression used for aggregation calculation. Use any scalar expression that returns a [`bool`](bool) value. Wildcards are not supported for field names.

## Results

Returns a count of rows for which **Predicate** evaluates to `true`.

## Examples

This example summarizes byte counts (with a minimum value of 11), by source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" 
| summarize gtthan10Count=countif(bytes > 10) by srcaddr
```


