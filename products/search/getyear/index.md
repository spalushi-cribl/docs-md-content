# getyear


The `getyear` function returns the year part of a [`datetime`](datetime).

Alias: `yearofyear`

## Syntax

    `getyear( Datetime )`

### Arguments

* **Datetime**: A [`datetime`](datetime).

## Example

This example returns `2025`:

```kusto {runSearch=true}
print year = getyear( datetime(2025-07-14) )
```
