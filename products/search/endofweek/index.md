# endofweek


The `endofweek` function returns the end of the week containing the date, shifted by an offset, if provided.

Last day of the week is considered to be a Saturday.

## Syntax

    `endofweek( Date [, Offset] )`

### Arguments

* **Date**: The input date.
* **Offset**: An optional number of offset weeks from the input date (integer, default - 0).

## Returns

A [`datetime`](datetime) representing the end of the week for the given **Date** value, with the offset, if specified.

## Example

This example returns `1,767,484,799.999`, representing `Sat Jan 31 2026 23:59:59 GMT+0000`:

```kusto {runSearch=true}
print weekEnd = endofweek( datetime(2025-12-21 10:10:17), 1)
```
