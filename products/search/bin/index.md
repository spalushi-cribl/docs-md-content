# bin



The `bin` function rounds values down to an integer multiple of a given bin size. If you have a scattered set of values, they'll be grouped into a smaller set of specific values.

Null values, a null bin size, or a negative bin size results in `null`.



You can use this function with the [`summarize`](summarize) and [`eventstats`](eventstats) operators.

## Syntax

    `bin( Value, RoundTo )`

### Arguments

* **Value**: A number, date, or timespan.
* **RoundTo**: The "bin size". A number or timespan that divides value.

## Returns

The nearest multiple of **RoundTo** below value.

## Examples

* `bin(4.5, 1)` results in `4.0`
* `bin(time(16d), 7d)` results in `14d`
* `bin(datetime(1970-05-11 13:45:07), 1d)` results in `datetime(1970-05-11)`

The following expression calculates a histogram of durations, with a bucket size of 1 second:

```kusto
dataset=myDataset
| summarize Hits=count() by bin(Duration, 1s)
```



