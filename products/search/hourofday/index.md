# hourofday


The `hourofday` function returns the integer representing the hour number of the given date.

## Syntax

    `hourofday( Datetime )`

### Arguments

* **Datetime**: A [`datetime`](datetime).

## Returns

The hour number of the day (0–23).

## Example

This example returns `18`:

```kusto {runSearch=true}
print hourofday( datetime(2025-11-30 18:54) )
```
