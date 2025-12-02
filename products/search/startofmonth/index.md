# startofmonth


The `startofmonth` function returns the start of the month containing the date, shifted by an offset, if provided.

## Syntax

    `startofmonth( Date, Offset )`

### Arguments

* **Date**: The input date.
* **Offset**: An optional number of offset months from the input date (integer, default - 0).

## Returns

A [`datetime`](datetime) representing the start of the month for the given **Date** value, with the offset, if specified.

## Example

This example returns `1664582400`:

```kusto {runSearch=true}
print monthStart = startofmonth( datetime(2022-09-01 10:10:17), 1 )
```
