# rate



# ![](page/agg-icon.svg) rate

The `rate` aggregation function returns the rate (based on `_time`) observed value of **Expression** across the group.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `rate( Expression, Time )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Time**: Unit of time, specified with a positive integer and time period. Supports weeks ( `w` ), days ( `d` ), hours ( `h` ), minutes ( `m` ), and seconds ( `s` ). For example, one day would be `1d`.

## Example

```kusto
dataset=myDataset
| summarize rate(goats, "5m")
```

