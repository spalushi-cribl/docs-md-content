# Common Query Examples


Learn common ways of searching data.

---

## Show `n` Rows

Let's see some data. What's in a random sample of five rows?

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | take 5
```

## Select a Subset of Fields

Use [`project`](project) to pick out only the fields you want.

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | project method, source, status, url
```

Returns:

| method | source                          | status | url            |
| ------ | ------------------------------- | ------ | -------------- |
| GET    | file://opt/cribl/log/access.log | 200    | /api/v1/health |
| ...    | ...                             | ...    | ...            |

## Filter by Expression

We'll use an expression to find specific field and value matches. Let's see only `GET` events in the `access.log`:

```kusto {runSearch=true}
dataset="cribl_internal_logs" source="*/access.log" method="GET"
 | project method, source, status, url
```

Returns:

| method | source                          | status | url            |
| ------ | ------------------------------- | ------ | -------------- |
| GET    | file://opt/cribl/log/access.log | 200    | /api/v1/health |
| GET    | file://opt/cribl/log/access.log | ...    | ...            |

## Compute Derived Fields

Create a new field by computing a value in every row:

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 5
 | extend query=query()
```

This query uses the [context](context-functions) function `query()`.

It's possible to reuse a column name and assign a calculation result to the same column.

Example:

```kusto
 | extend x = x + 1, y = x
 | extend x = x + 1
```

## Aggregate Groups of Rows

To count the number of events by `src` we'll use the [count](count) function.

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 1000
 | summarize cnt=count() by srcaddr
```

Returns:

| src          | cnt  |
| ------------ | ---- |
| 10.255.0.248 | 2978 |
| ...          | ...  |

The [`summarize`](summarize) operator groups together rows that have the same values in the `by` clause, and then uses
an aggregation function (for example, `count`) to combine each group in a single row. In this case, there's a row for
each `src` and a field for the count of rows in that `src`.

You can use several aggregation functions in one [`summarize`](summarize) operator to produce several computed columns.
For example, we could get the count of events per `src`, and the sum of unique types of events per `src`.

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access*"
 | limit 1000
 | summarize eventCount=count(), useragentCount=dcount(useragent) by clientip
```

Returns:

| src          | eventCount | message |
| ------------ | ---------- | ------- |
| 10.255.0.248 | 5610       | 2       |
| ...          | ...        | ...     |

In the results of a [`summarize`](summarize) operator:

- Each field is named in the `by` clause.
- Each computed expression has a field.
- Each combination of `by` values has a row.

## Display Multiple Series

Use multiple values in a [`summarize by`](summarize) clause to create a separate row for each combination of values:

```kusto {runSearch=true}
dataset="cribl_search_sample" dataSource="access_combined"
 | limit 1000
 | summarize count() by host, clientip
```

## Display a Time Series

The [`timestats`](timestats) operator aggregates events by time periods or bins, which is excellent for displaying a
time series. Here we slice the results into one-minute sections, or bins:

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 1000
 | timestats span=1m
```

## Display Results as Events or Tables

Use the [`render`](render) operator to display results as a list of events or a table.

To display results as a list of events under the [**Events** tab](search-overview#events):

```kusto {runSearch=true}
dataset=$vt_dummy event<100
 | extend parity=iif(event%2==0, 'even', 'odd')
 | project event, parity
 | render table
```

To display results as a [table](results-table-settings) under the [**Chart** tab](search-overview#charts):

```kusto {runSearch=true}
dataset=$vt_dummy event<100
 | extend parity=iif(event%2==0, 'even', 'odd')
 | project event, parity
 | render event
```

## Search Cribl Lake

You can search [Cribl Lake](/lake/) Datasets the same way you use regular Cribl Search Datasets:

```kusto
dataset="my_lake_dataset"
```

See [Searching Cribl Lake](search-cribl-lake) for more examples.

## Search Arrays {#arrays}

In this example, `arr1` is an array of strings, and `arr2` is an array of numbers:

```kusto
dataset="ClickhouseData"
arr1[0]='s1'
arr2[0]=1
| limit 1000
```

As shown here, you can reference specific elements in an array – using zero-based index notation – to retrieve only the
data you specify. Your query must specify the field name or data point at each array index. (Cribl Search does not
support querying on the index to discover array members.)



Where a Dataset Provider returns array columns as strings, Cribl Search parses these columns back to arrays.



## Search Nested JSON Objects {#nested-json}

In this example, `bar` is a field nested within three levels of JSON objects:

```kusto
dataset="my_warehouse" json1.obj2.nestedObj2.nestedObj3='bar'
 | limit 1000
```

## Subqueries

Subqueries allow you to generate different sets of results in a single search. This lets you perform dynamic lookups
with no need to create static, stored [lookup tables](lookups).

You can use subqueries to append one set of results to another, through the [`union`](union) operator. You can also use
them to [merge](join#types-of-joins) data coming from different [Datasets](basic-concepts#datasets) and
[Dataset Providers](basic-concepts#dataset-providers), with the help of the [`join`](join) operator.

There are two types of subqueries:

- [Inline subqueries](#inline-subqueries)
- [`let` subqueries](#let-statement-subqueries)

### Inline Subqueries

An inline subquery is nested inside another query, using round brackets `()`.

You may find inline subqueries easier to write than [`let`](let) subqueries, but:

- You can reference their results only as vectors, using [`union`](union) or [`join`](join).
- If you opt to use the [`cribl`](cribl) operator, you must explicitly include it in the inline subquery.

```kusto {runSearch=true}
// main query
dataset=$vt_dummy event<10
 | extend foo=42
 | union (
    // inline subquery
    cribl dataset=$vt_dummy event<10
     | extend bar=24
 )
```

You could achieve the same result as above by using a [`let` subquery](#let-subqueries):

```kusto {runSearch=true}
let subquery = dataset=$vt_dummy event<10
 | extend bar=24;

dataset=$vt_dummy event<10
 | extend foo=42
 | union subquery
```

The following example uses an inline subquery to join two different scopes of data:

```kusto {runSearch=true}
dataset=$vt_dummy event<10
 | extend foo=42
 | join (
    // inline subquery
    cribl dataset=$vt_dummy event<10
     | extend bar=24
 ) on event;
```

### `let` Subqueries

You define [`let`](let) subqueries at the beginning of a query, using [`let`](let) statements. You can then reference
them by name, like this:

```kusto {runSearch=true}
// each let statement ends with a semicolon
let x = 1000;

dataset="cribl_search_sample"
 | limit x
```

Depending on what a [`let`](let) subquery evaluates to, you can reference it as a
[vector](#reference-a-let-subquery-as-a-vector) or as a [scalar](#reference-a-let-subquery-as-a-scalar).

Similarly to [inline subqueries](#inline-subqueries), you can use [`let`](let) subqueries with the [`join`](join) and
[`union`](union) operators:

```kusto {runSearch=true}
let subquery = dataset="$vt_dummy" event < 10
 | extend bar=24;

dataset="$vt_dummy" event < 10
 | extend foo=42
 | join subquery on event
```

[`let`](let) subqueries can also reference one another, for example:

```kusto {runSearch=true}
let subquery = dataset="$vt_dummy" event < 1
 | extend bar=24;

dataset="$vt_dummy" event < 10
 | extend foo=42, subqueryBar=subquery.bar
```

#### Reference a `let` Subquery as a Vector

When a [`let`](let) subquery evaluates to a vector, you can use its results to filter the results of your main query,
with help of the [`in`](in-cs)/[`!in`](not-in-cs)/[`in~`](in)/[`!in~`](not-in) operators.

Mind that this functionality is not supported by the implicit [`cribl`](cribl) operator, so you need to use
[`search`](search), [`find`](find), or [`where`](where). For example:

```kusto {runSearch=true}
// dummy list of blocklisted IPs
let pretendBlocklistedIps = dataset="cribl_search_sample" dataSource="vpcflowlogs"
 | limit 10
 | distinct srcaddr
 | project blocklistedIp=srcaddr;

dataset="cribl_search_sample" dataSource="vpcflowlogs"
 | where srcaddr in pretendBlocklistedIps.blocklistedIp;
```

#### Reference a `let` Subquery as a Scalar

To reference a [`let`](let) subquery as a scalar, mind the following:

- The subquery needs to produce exactly one event.
- You must specify a field, using dot notation (`subquery1.text`). You can also reference a nested field
  (`subquery1.headers.test`).
- The field you specify must be a scalar value, not an object.
- You can't specify fields that don't exist.

See the following example, which calculates the 95th percentile of requests in the Cribl internal logs.

```kusto {runSearch=true}
let num_percentile = 95;

let response_time_percentile = dataset="cribl_internal_logs" method status response_time
 | summarize total=count(), response_time=percentile(response_time, num_percentile);

let long_responses = dataset="cribl_internal_logs" method status response_time
 | where response_time > response_time_percentile.response_time
 | summarize count=count(), maxResponseTime=max(response_time);

print strcat('There are ', long_responses.count, ' requests in the ', num_percentile, ' percentile. Total requests were: ', response_time_percentile.total, '. Highest response time was: ', long_responses.maxResponseTime, 'ms');
```

### Subqueries and Time Ranges {#time-range}

A subquery can have its own time range. However, the time range of the main query (either set in the query itself, or
[configured through the UI](build-a-search#time-range)) still applies to the final results.

For example, the following search won't get you any `earlysample` results, because they're outside of the time range of
the main query:

```kusto {runSearch=true}
// subquery time range between 50 and 45 minutes ago
let earlysample = dataset="cribl_search_sample" earliest=-50m@m latest=-45m@m
 | extend stage="one"
 | limit 100;

// main query time range between 10 and 5 minutes ago
dataset="cribl_search_sample" earliest=-10m@m latest=-5m@m
 | extend stage="two"
 | limit 100
 | union earlysample
```

To go around this, you can apply that second time range to another subquery, and then let the main query contain both
time ranges:

```kusto {runSearch=true}
let earlysample = dataset="cribl_search_sample" earliest=-50m@m latest=-45m@m
 | extend stage="one"
 | limit 100;

let latesample = dataset="cribl_search_sample" earliest=-10m@m latest=-5m@m
 | extend stage="two"
 | limit 100;

dataset="cribl_search_sample" earliest=-50m@m latest=-5m@m
 | limit 100
 | union earlysample, latesample
```
