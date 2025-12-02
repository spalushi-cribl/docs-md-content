# next


The `next` function returns the value of a specific field in a subsequent row. The subsequent row is located at a specified offset relative to the current row within the results.

## Syntax

    `next(Field [, Offset ] [, DefaultValue ] )`

### Arguments

* **Field**: The field from which to get the values.
* **Offset**: The number of rows to move from the current row. Default is `1`.
* **DefaultValue**: The default value when there's no value in the later row. The default is `null`.

## Scope

Cribl Search supports this function in the [`extend`](extend) operator, but not in the [`project`](project) or [`where`](where) operator.

{#example}
## Examples

This example returns the time difference between adjacent events.

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access*" host="web01.cribl.io"
 | limit 100
 | sort by _time asc
 | extend time_next_delta = next(_time) - (_time)
```

This example filters for events with a time difference greater than 250 ms:

```kusto
dataset=myDataset
| where SensorName == 'sensor-9'
| sort by Timestamp asc
| extend timeDiffInMilliseconds = datetime_diff('millisecond', next(Timestamp, 1), Timestamp)
| where timeDiffInMilliseconds > 250
```
