# row_rank_dense



The `row_rank_dense` function assigns a unique numerical position (rank) to each row within the results, preserving the order even for rows with equal values. Unlike regular ranking, there are no gaps in the assigned ranks.

The row rank starts by default at 1 for the first row, and is incremented by 1 whenever the provided **Term** is different than the previous row's **Term**.

## Syntax

    `row_rank_dense( Term [, Restart ] )`

### Arguments

* **Term**: An expression that indicates the value to consider for the rank. The rank is increased whenever the **Term** changes.
* **Restart**: An expression that returns a [`bool`](bool) value to indicate when the ranking operation should restart to the **Term** value. The default is `false`.

## Scope

Cribl Search supports this function in the [`extend`](extend) operator, but not in the [`project`](project) or [`where`](where) operator.

## Examples

This example uses `row_rank_dense` to return a count of distinct hosts, and also uses [`row_rank_min`](row_rank_min) to rank the hosts.

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
| extend Rank=row_rank_dense(appname, prev(appname) != appname)
```