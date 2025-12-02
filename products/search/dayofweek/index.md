# dayofweek


The `dayofweek` function returns an integer between `0` and `6` representing the day of the week, beginning on Sunday. 

> This differs from the standard [Kusto](https://learn.microsoft.com/en-us/kusto/query/day-of-week-function) `dayofweek` function, which returns a timespan.
{.box .info}

## Syntax

    `dayofweek( Date )`

### Arguments

* **Date**: A [`datetime`](datetime).

## Returns

The `timespan` since midnight at the beginning of the preceding Sunday, rounded down to an integer number of days.

## Example

This example returns `2  for Tuesday:

```kusto {runSearch=true}
print dayofweek( datetime(2025-12-16) )
```
