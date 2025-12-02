# variance



# ![](page/agg-icon.svg) variance

The `variance` aggregation function calculates the variance of **Expression** across the group, considering the group as a [sample](https://en.wikipedia.org/wiki/Sample_%28statistics%29).



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

Used formula:

![variance formula](variance-sample.b3a19cceb3.png)
{scale="50%"}

## Syntax

    `variance( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Results

The variance value of **Expression** across the group.

## Example

This example summarizes the average byte count, and corresponding variance, by source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="vpcflowlogs" 
| summarize avg(bytes), variance(bytes) by srcaddr
```


