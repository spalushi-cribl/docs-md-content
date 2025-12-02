# count


# ![](page/agg-icon.svg) count

The `count` aggregation function counts the number of non-null events per summarization group, or the total number of non-null events if summarization is done without grouping.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

> If you need to count boolean `true` values, use the [`countif`](countif) aggregation function.
>
{.box .info}

## Syntax

    `count( [Expression] )`

### Arguments

* **Expression**: Optional. Expression used for aggregation calculation. Wildcards are not supported for field names.

## Results

Returns a count of the events per summarization group (or in total, if summarization is done without grouping).

## Example

This example summarizes the number of non-null events by source address:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| limit 1000 
| summarize NumberOfEvents=count() by srcaddr
```


