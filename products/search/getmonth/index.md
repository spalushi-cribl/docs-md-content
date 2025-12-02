# getmonth


The `getmonth` function gets the month number (1–12) from a [`datetime`](datetime).

Alias: [`monthofyear`](monthofyear)

## Syntax

    `getmonth( Datetime )`

### Arguments

* **Datetime**: A [`datetime`](datetime).

## Example

This example returns `10`:

```kusto {runSearch=true}
print month = getmonth( datetime(2025-10-14) )
```