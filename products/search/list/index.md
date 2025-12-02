# list


# ![](page/agg-icon.svg) list

The `list` aggregation function returns the list of values of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `list( Expression [, Max ] )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Max**: An integer that limits the number of values returned. The default is `100`. If set to `0`, all values are returned.

{#example}
## Examples {#examples} 

This example lists methods (HTTP verbs) on API requests, up to a limit:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource=access*
| limit 1000 
| summarize list(request_method)
```


