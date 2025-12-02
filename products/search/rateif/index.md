# rateif



# ![](page/agg-icon.svg) rateif

The `rateif` aggregation function returns the rate (based on `_time`) observed value of **Expression** across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `rateif( Expression, Time, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Time**: Unit of time, specified with a positive integer and time period. Supports weeks ( `w` ), days ( `d` ), hours ( `h` ), minutes ( `m` ), and seconds ( `s` ). For example, one day would be `1d`.
* **Predicate**: Expression that will be used to filter rows.

## Example

```kusto
dataset=myDataset
| summarize rateif(goats, "5s", host=="Cribl.local")
```

