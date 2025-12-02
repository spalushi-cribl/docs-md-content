# row_cumsum



The `row_cumsum` function calculates the cumulative sum for a specified field across all previous rows.

## Syntax

    `row_cumsum(Term [, Restart ] )`

### Arguments

* **Term**: An expression that indicates the value to be summed. Supports `int`, `long`, or `real`.
* **Restart**: An expression that returns a [`bool`](bool) value to indicate when the accumulation operation should restart or be set back to 0. It can be used to indicate partitions in the data.

## Scope

Cribl Search supports this function in the [`extend`](extend) operator, but not in the [`project`](project) or [`where`](where) operator.

## Examples

This first example returns total bytes per a specified host.

```kusto {runSearch=true}
ddataset="cribl_search_sample" dataSource="access*" host="web01.cribl.io"
 | limit 100
 | sort by _time asc 
 | extend total_bytes=row_cumsum(bytes)
 | project _time, bytes, total_bytes
``` 

This second example returns total bytes per a specified host, restarting the count when the `clientip` is identical in adjacent rows.

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access*" host="web01.cribl.io"
 | limit 100
 | sort by _time asc 
 | extend total_bytes_from_client=row_cumsum(bytes, clientip!=prev(clientip))
 | project _time, clientip, bytes, total_bytes_from_client
``` 
