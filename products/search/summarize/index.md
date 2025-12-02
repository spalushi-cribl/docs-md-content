# summarize



# ![](page/agg-icon.svg) summarize

The `summarize` operator produces a table that aggregates the input data.

`summarize` is similar to [eventstats](eventstats), but it replaces the input events instead of enriching them.

## Syntax

`Scope | summarize [[AggregatedField =] AggregationFunction [, ...]] [by [GroupField =] GroupingExpression [, ...]]`

### Arguments

* **Scope**: The events to search.
* **AggregatedField**: Optional name for a field that contains an aggregation result. Defaults to a name derived from the corresponding **AggregationFunction**.
* **GroupField**: Optional name for a group field. Defaults to a name derived from the **GroupingExpression**.
* **AggregationFunction**: [Cribl](cribl-functions) and [statistical](statistical-functions) functions, with field names as arguments. Wildcards are not supported for field names in aggregation functions.
* **GroupingExpression**: An expression that can reference the input data. The output will have as many events as there are distinct values of all the grouping expressions.

When the input table is empty, the output depends on whether **GroupingExpression** is used:

* If **GroupingExpression** is not provided, the output will be a single (empty) event.
* If **GroupingExpression** is provided, the output will have no events.

## Results

The input events are arranged into groups having the same values of the `by` expressions. Then the specified aggregation functions are computed over each group, producing an event for each group. The result contains the `by` fields and also at least one field for each computed aggregate. (Some aggregation functions return multiple fields.)

The result has as many events as there are distinct combinations of `by` values (which may be zero). If there are no group keys provided, the result has a single record.

## Examples

### Minimum and maximum timestamp

Finds the minimum and maximum timestamp of all events in the Dataset. There is no `by` clause, so there is just one event in the output:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| summarize Min = min(_time), Max = max(_time)
```

### Minimum and maximum across two fields

Finds the minimum `start` time and maximum `end` times across events in the Dataset. With no `by` clause, there is just one event in the output:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| summarize earliest = min(start), latest = max(end)
```

### Distinct count

Create an event for each source address, showing a count of distinct ports. Because there are few values for `srcaddr`, no grouping function is needed in the `by` clause:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| summarize sources=dcount(srcport) by srcaddr
```

Create an event for each continent, showing a count of the cities in which activities occur. Because there are few values for `continent`, no grouping function is needed in the `by` clause:

```kusto
dataset=myDataset 
| summarize cities=dcount(city) by continent
```

Summarize a count of distinct destination ports, by both destination address and day start.

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| summarize destinations=dcount(dstport) by dstaddr, startofday(start)
```

### Count and total by category and month

Create a table with sell transactions and the total amount per fruit and sell month. The output fields show the count of transactions, transaction worth, fruit, and the [`datetime`](datetime) of the beginning of the month in which the transaction was recorded.

```kusto
dataset=myDataset 
| summarize NumTransactions=count(), Total=sum(UnitPrice * NumUnits) by Fruit, StartOfMonth=startofmonth(SellDateTime)
```

### Count items in intervals

* Create a table that shows how many items have prices in each interval: [0,10.0], [10.0,20.0], and so on. This example has a field for the count and a field for the price range.

  ```kusto
  dataset=myDataset 
  | summarize count() by price_range=bin(price, 10.0)
  ```

* Count events by 60-second intervals. formatting the results as human-readable times.

  ```kusto {runSearch=true}
  dataset="cribl_search_sample" 
  | summarize count() by minute=bin(start, 60), strftime(minute,"%X")
  ```

* Count by parity.

  ```kusto {runSearch=true}
  dataset=$vt_dummy event<10 
  | extend answer=42, parity=iif(event%2==0, 'even', 'odd') 
  | summarize count() by parity
  ```

* Generate a table of users and the credits they have consumed. [`$vt_jobs`](vt_jobs) requires **Admin** [Permissions](cloud-managing#search-member-roles).

  ```kusto {runSearch=true}
  dataset=$vt_jobs 
  | summarize totalSeconds=sum(cpuMetrics.totalExecCPUSeconds) by user 
  | extend CreditsUsed=totalSeconds/3600 
  | project CreditsUsed, user 
  | render table
  ```

