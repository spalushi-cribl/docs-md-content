# values


# ![](page/agg-icon.svg) values

The `values` aggregation function returns all of the distinct values of **Expression** across the group. This allows you to quickly identify and understand all the values a field has in your data.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `values( Expression [, Max [, ErrorRate] ] )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Max**: An integer that limits the number of values returned. The default is `0` where all distinct values are returned.
* **ErrorRate**: Controls how accurately the function counts distinct values. Range is `0–1`. The default value is `0.01`. Higher values allow higher error rates (fewer unique values recognized), with the offsetting benefit of less memory usage.

## Results

The response field is an array of unique values (a "set"), up to either a specified limit or the default limit of `100`. There is no guarantee that this array's elements will be returned in any particular order.

{#example}
## Examples {#examples}

This example lists unique methods (HTTP verbs) on API requests, up to the supplied limit:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource=access*
| limit 1000 
| summarize values(request_method)
```


