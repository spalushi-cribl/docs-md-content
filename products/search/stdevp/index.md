# stdevp



# ![](page/agg-icon.svg) stdevp

The `stdevp` aggregation function calculates the standard deviation of **Expression** across the group, considering the group as a [population](https://en.wikipedia.org/wiki/Statistical_population) for a large data set that is representative of the population.

For a small data set that is a sample, use the [stdev](stdev) aggregation function.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

Used formula:

![stdevp formula](stdev-population.d78a968671.png)
{scale="50%"}

## Syntax

    `stdevp( Expression )`

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


