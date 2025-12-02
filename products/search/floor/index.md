# floor



The `floor` function rounds values down to an integer multiple of a given floor size. If you have a scattered set of values, they'll be grouped into a smaller set of specific values.

Null values, a null floor size, or a negative floor size results in `null`.



You can use this function with the [`summarize`](summarize) and [`eventstats`](eventstats) operators.

## Syntax

    `floor( Value [, RoundTo ] )`

### Arguments

* **Value**: A number, date, or timespan.
* **RoundTo**: The "floor size". A number or timespan that divides value. Defaults to `1`.

## Returns

The nearest multiple of **RoundTo** below value.

## Examples

* `floor(4.5, 1)` results in `4`
* `floor(3.14)` results in `3`.
* `floor(time(16d), 7d)` results in `14d`
* `floor(datetime(1970-05-11 13:45:07), 1d)` results in `datetime(1970-05-11)`

The following expression calculates a histogram of durations, with a bucket size of 1 second:

```kusto
dataset=myDataset
| summarize Hits=count() by floor(Duration, 1s)
```



