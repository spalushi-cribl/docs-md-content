# row_rank_min



The `row_rank_min` function assigns a minimal numerical position (rank) to each row within the results, considering rows with equal values to have the same rank.

The rank is the minimal row number that the current row's **Term** appears in.

## Syntax

    `row_rank_min( Term [, Restart ] )`

### Arguments

* **Term**: An expression that indicates the value to consider for the rank. The rank is the minimal row number for **Term**.
* **Restart**: An expression that returns a [`bool`](bool) value to indicate when the ranking operation should restart to the **Term** value. The default is `false`.

## Scope

Cribl Search supports this function in the [`extend`](extend) operator, but not in the [`project`](project) or [`where`](where) operator.

## Examples

This example uses [`row_rank_min`] to return a ranking of hosts, and also uses [`row_rank_dense`](row_rank_dense) to count distinct hosts.

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access*"
 | limit 100
 | sort by host asc
 | extend row_rank_dense=row_rank_dense(host) // incremented from 1
 | extend row_rank_min=row_rank_min(host) // actual row number (starting from row 1)
 | project _time, host, row_rank_dense, row_rank_min
```

This snippet partitions data by `appname`:

```kusto 
| extend Rank=row_rank_min(appname, prev(appname) != appname)
```
