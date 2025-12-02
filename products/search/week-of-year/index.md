# week_of_year


The `week_of_year` function returns an integer representing the week number. The week number is calculated from the first week of a year, which is the one that includes the first Thursday, according to [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Week_dates).

## Syntax

    `week_of_year( Datetime )`

### Arguments

* **Datetime**: A [`datetime`](datetime).

## Returns

The week number of the given year.

## Examples

This example returns `53`:

```kusto {runSearch=true}
print week_of_year( datetime(2020-12-31) )
```

This example returns `1`:

```kusto {runSearch=true}
print week_of_year( datetime(1970-01-01) )
```
