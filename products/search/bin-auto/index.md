# bin_auto



The `bin_auto` function rounds values down to a fixed-size "bin", with control over the bin size and starting point provided by an option property.



You can use this function with the [`summarize`](summarize) and [`eventstats`](eventstats) operators.

## Syntax

    `bin_auto( Expression )`

with option:

    `set OptionName [ = OptionValue ] ; ... | ... bin_auto( Expression )`

### Arguments

* **Expression**: A scalar expression of a numeric type indicating the value to round.
* **OptionName**:

    * `query_bin_auto_size`: Time period. Supports these relative times – `s[econds]`, `m[inutes]`, `h[ours]`, and `d[ays]`.
    * `query_bin_auto_buckets`: Numeric literal, desired number of buckets to be created by `bin_auto`. Buckets are split as close as possible based on the search's time range.
    
#### Rules

* The total number of buckets will vary based on the time range.
* Values without units get interpreted as seconds. (For example, `-1` = `-1s`.)

## Example

```kusto
set query_bin_auto_size=1h;
dataset=myDataset
| summarize count() by bin_auto(timestamp)
```

