# startofweek


The `startofweek` function returns the start of the week containing the date, shifted by an offset, if provided.

Start of the week is considered to be a Sunday.

## Syntax

    `startofweek( Date, Offset )`

### Arguments

* **Date**: The input date.
* **Offset**: An optional number of offset weeks from the input date (integer, default - 0).

## Returns

A [`datetime`](datetime) representing the start of the week for the given **Date** value, with the offset, if specified.

## Example

This example returns `1,662,249,600`:

```kusto {runSearch=true}
print weekStart = startofweek( datetime(2022-09-01 10:10:17), 1 )
```
