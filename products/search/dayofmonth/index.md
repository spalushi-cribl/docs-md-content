# dayofmonth


The `dayofmonth` function returns the integer number representing the day number of the given month.

## Syntax

    `dayofmonth( Date )`

### Arguments

* **Date**: A [`datetime`](datetime).

## Returns

The day number of the given month.

## Example

This example returns `14`:

```kusto {runSearch=true}
print dayofmonth(datetime(2015-12-14))
```
