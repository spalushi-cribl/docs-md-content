# row_number


The `row_number` function assigns a unique row number to each row within the results. The row index starts by default at 1 for the first row, and is incremented by 1 for each additional row. Optionally, the row index can start at a different value than 1.

## Syntax

    `row_number(StartingIndex [, Restart ] )`

### Arguments

* **StartingIndex**: The value of the row index to start at or restart to. The default value is `1`. Supports `long`.
* **Restart**: An expression that returns a [`bool`](bool) value to indicate when the numbering operation should restart to the **StartingIndex** value. The default is `false`.

## Scope

Cribl Search supports this function in the [`extend`](extend) operator, but not in the [`project`](project) or [`where`](where) operator.

## Example

This example restarts the cumulative counter each time it encounters identical `clientip` values on adjacent rows.

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access*" host="web01.cribl.io"
 | limit 100
 | sort by _time asc 
 | extend rownum=row_number(0, clientip!=prev(clientip))
 | project _time, clientip, bytes, rownum
``` 
