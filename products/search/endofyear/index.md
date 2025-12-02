# endofyear


The `endofyear` function returns the end of the year containing the date, shifted by an offset, if provided.

## Syntax

    `endofyear( Date [, Offset] )`

### Arguments

* **Date**: The input date.
* **Offset**: An optional number of offset years from the input date (integer, default - 0).

## Returns

A [`datetime`](datetime) representing the end of the year for the given **Date** value, with the offset, if specified.

## Example

This example returns `1,798,761,599.999`, representing `Wed Dec 31 2026 23:59:59 GMT+0000`:

```kusto {runSearch=true}
print yearEnd = endofyear( datetime(2025-10-31 10:10:17), 1)
```
