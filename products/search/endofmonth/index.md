# endofmonth


The `endofmonth` function returns the end of the month containing the date, shifted by an offset, if provided.

## Syntax

    `endofmonth( Date [, Offset] )`

### Arguments

* **Date**: The input date.
* **Offset**: An optional number of offset months from the input date (integer, default - 0).

## Returns

A [`datetime`](datetime) representing the end of the month for the given **Date** value, with the offset, if specified.

## Example

This example returns `1,769,903,999.999`, representing `Sat Jan 31 2026 23:59:59 GMT+0000`:

```kusto {runSearch=true}
print monthEnd = endofmonth( datetime(2025-12-21 10:10:17), 1 )
```
