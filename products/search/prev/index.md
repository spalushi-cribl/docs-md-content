# prev



The `prev` function returns the value of a specific field in a previous row. The previous row is located at a specified offset relative to the current row within the results.

## Syntax

    `prev(Field [, Offset ] [, DefaultValue ] )`

### Arguments

* **Field**: The field from which to get the values.
* **Offset**: The offset to go back in rows. The default is `1`.
* **DefaultValue**: The default value to be used when there are no previous rows from which to take the value. The default is `null`.

## Scope

Cribl Search supports this function in the [`extend`](extend) operator, but not in the [`project`](project) or [`where`](where) operator.

{#example}
## Examples

This example returns the time difference between adjacent events.

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access*" host="web01.cribl.io"
 | limit 100
 | sort by _time asc
 | extend time_prev_delta = (_time) - prev(_time)
 ```

This example filters for events with a time difference greater than 250 ms:

```kusto
dataset=myDataset
| where SensorName == 'sensor-9'
| sort by Timestamp asc
| extend timeDiffInMilliseconds = datetime_diff('millisecond', Timestamp, prev(Timestamp, 1))
| where timeDiffInMilliseconds > 250
