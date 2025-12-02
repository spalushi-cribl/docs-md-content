# stdev



# ![](page/agg-icon.svg) stdev

The `stdev` aggregation function calculates the standard deviation of **Expression** across the group, using [Bessel's correction](https://en.wikipedia.org/wiki/Bessel%27s_correction) for a small data set that is considered a [sample](https://en.wikipedia.org/wiki/Sample_%28statistics%29). 

For a large data set that is representative of the population, use the [stdevp](stdevp) aggregation function.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

Used formula:

![stdev formula](stdev-formula.2a50022344.png)
{scale="50%"}

## Syntax

    `stdev( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Results

The standard deviation value of **Expression** across the group.

## Example

This example summarizes average traffic in bytes, and two measures of standard deviation (sample- and population-based), by source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" 
| summarize avg(bytes), stdev(bytes), stdevp(bytes) by srcaddr
```


