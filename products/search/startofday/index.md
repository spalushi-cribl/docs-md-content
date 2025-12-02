# startofday


The `startofday` function returns the start of the day containing the date, shifted by an offset, if provided.

## Syntax

    `startofday( Date, Offset )`

### Arguments

* **Date**: The input date.
* **Offset**: An optional number of offset days from the input date (integer, default - 0).

## Returns

A [`datetime`](datetime) representing the start of the day for the given **Date** value, with the offset, if specified.

## Example

This example returns `1641081600`:

```kusto {runSearch=true}
print dayStart = startofday( datetime(2022-01-01 10:10:17), 1 ) 
```
