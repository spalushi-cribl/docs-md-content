# percentile


# ![](page/agg-icon.svg) percentile

The `percentile` aggregation function returns an estimate for the specified [nearest-rank percentile](#nearest) of the population defined by **Expression**. The accuracy depends on the density of population in the region of the percentile. The percentiles aggregate provides an approximate value using [T-Digest](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf).



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `percentile( Expression, Percentile )`

### Arguments

* **Expression**: Expression that will be used for aggregation calculation. Does not support wildcards for field names.
* **Percentile**: A double constant that specifies the percentile.

## Results

Returns an estimate for **Expression** of the specified percentiles in the group.

{#example}
## Examples

This example summarizes 95th-percentile traffic, in bytes, by source address, excluding low values:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" 
| summarize perc95=percentile(bytes, 95) by srcaddr 
| where perc95 > 100
```

This example summarizes 95th-percentile traffic, in bytes, by destination address, excluding `null` values:

```kusto {runSearch=true}
dataset="cribl_search_sample" datatype="aws_vpcflow" 
| where isnotnull(bytes) and isnotnull(dstaddr) 
| summarize p95=percentile(bytes, 95) by dstaddr
```


