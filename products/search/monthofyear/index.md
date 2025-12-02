# monthofyear


The `monthofyear` function returns the integer number representing the month number of the given year.

## Syntax

    `monthofyear( Datetime )`

### Arguments

* **Datetime**: A [`datetime`](datetime).

## Returns

The month number of the given year.

## Example

This example returns `11`:

```kusto {runSearch=true}
print monthofyear( datetime("2025-11-30") )
```
