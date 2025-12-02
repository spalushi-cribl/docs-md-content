# endofday


The `endofday` function returns the end of the day containing the date, shifted by an offset, if provided.

## Syntax

    `endofday( Date [, Offset] )`

### Arguments

* **Date**: The input date.
* **Offset**: An optional number of offset days from the input date (integer, default - 0).

## Returns

A [`datetime`](datetime) representing the end of the day for the given **Date** value, with the offset, if specified.

## Example

This example returns `1,766,707,199.999`, representing `Thu Dec 25 2025 23:59:59 GMT+0000`:

```kusto {runSearch=true}
project dayEnd = endofday( datetime(2025-12-24), 1 )
```