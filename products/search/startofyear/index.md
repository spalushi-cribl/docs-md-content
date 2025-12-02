# startofyear


The `startofyear` function returns the start of the year containing the date, shifted by an offset, if provided.

## Syntax

    `startofyear( Date, Offset )`

### Arguments

* **Date**: The input date.
* **Offset**: An optional number of offset years from the input date (integer, default - 0).

## Returns

A [`datetime`](datetime) representing the start of the year for the given **Date** value, with the offset, if specified.

## Example

This example returns `1,767,225,600`:

```kusto {runSearch=true}
print yearStart = startofyear( datetime(2025-12-01 10:10:17), 1 )
```
