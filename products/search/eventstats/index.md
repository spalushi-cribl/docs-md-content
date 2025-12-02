# eventstats


# ![](page/agg-icon.svg) eventstats

The `eventstats` operator aggregates events and adds the results as new fields to the source events.

`eventstats` is similar to [`summarize`](summarize), but it enriches the input events instead of replacing them.

By default, `eventstats` can aggregate up to 50,000 events at a time. You can change this limit with the **MaxNoOfAggregatedEvents** parameter.

## Syntax

`Scope | eventstats [max_events=MaxNoOfAggregatedEvents] [[AggregatedField =] AggregationFunction [, ...]] [by [GroupField =] GroupingExpression [, ...]]`

### Arguments

* **Scope**: The events to aggregate and enrich.
* **MaxNoOfAggregatedEvents**: The maximum number of events to aggregate. After reaching this limit, aggregation stops, and all of the input events are enriched with the same, most recent aggregation results. Default: `50000`.
* **AggregatedField**: Optional name for a field that contains an aggregation result. Defaults to a name derived from the corresponding **AggregationFunction**.
* **GroupField**: Optional name for a group field. Defaults to a name derived from the corresponding **GroupingExpression**.
* **AggregationFunction**: A [Cribl](cribl-functions) or [statistical](statistical-functions) function, with field names as arguments. You can add multiple functions, separated by a comma. Wildcards are not supported for field names in aggregation functions.
* **GroupingExpression**: The expression by which `eventstats` groups the input events before aggregating them. You can add multiple expressions, separated by a comma.

## Results

First, the input events are arranged into groups where the corresponding **GroupingExpression**s evaluate to the same values.

Then, the specified **AggregationFunction**s process each group. The results are added to the input events as new fields.

## Examples

Calculate the average response time for all events, and add a new field that contains the result.

```kusto {runSearch=true}
dataset="cribl_internal_logs"
| limit 100
| eventstats avgResponseTime = avg(response_time)
```

Calculate the average response time separately for each distinct value of the `src` field. Name the result field `srcAvgResponseTime`.

```kusto {runSearch=true}
dataset="cribl_internal_logs"
| limit 100
| eventstats srcAvgResponseTime = avg(response_time) by src
```

Show only those events that have a response time greater than the average.

```kusto {runSearch=true}
dataset="cribl_internal_logs"
| limit 100
| eventstats avgResponseTime = avg(response_time)
| where response_time > avgResponseTime
```

Calculate the ratio of events for each HTTP method.

```kusto {runSearch=true}
dataset="cribl_internal_logs" method
| limit 1000
| summarize cnt=count() by method
| eventstats total=sum(cnt)
| project method, ratio = (cnt * 100 / total)
```

```kusto {runSearch=true}
dataset=$vt_dummy event<1000
| extend randomNumber=rand(10)
| eventstats avg(randomNumber)
```
