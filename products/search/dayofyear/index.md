# dayofyear


The `dayofyear` function returns the integer number representing the day number of the given year.

## Syntax

    `dayofyear( Date )`

### Arguments

* **Date**: A [`datetime`](datetime).

## Returns

The day number of the given year.

## Example

This example returns `348`:

```kusto {runSearch=true}
print dayofyear( datetime(2025-12-14) )
```
