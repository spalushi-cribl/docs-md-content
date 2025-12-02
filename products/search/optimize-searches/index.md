# Optimize Searches


Get strategies and tips for optimizing your searches.

---

The practices on this page help you make searches more efficient and faster.

## Constrain Initial Query with Limit, Count, or Time Window {#limit}

Until you’re certain how large your Dataset is, you should add a [`limit`](limit), [`count()`](count), or time limit to your initial query.

Instead of this: 

```kusto
dataset="cribl_search_sample"
```

Do this: 

```kusto
dataset="cribl_search_sample" | limit 1000
```

Or this: 

```kusto
dataset="cribl_search_sample" dataSource="access_combined"
 | limit 1000
 | summarize count() by host, clientip
```

Or this, to constrain results to a 2-month window: 

```kusto
dataset="cribl_search_sample" earliest=-2mon
```

## Unstructured Searches Are Fastest {#unstructured}

Unstructured term searches will return faster than structured searches. Consider using a broad search (with a limit) in initial investigations.

Instead of this: 

```kusto
dataset="my_dataset" email="jim@company.com" | limit 1000
```

Do this: 

```kusto
dataset="my_dataset" "jim@company.com" | limit 1000
```

## Place Filters Early in Queries {#filters-first}

Try to push filtering expressions to the left-most side of your query wherever possible.

Instead of this: 

```kusto
dataset="<my-datasource>" 
| where tenantId == "foo-bar-12345" 
| where proc == "bash" 
| where data_source == "stdout"
```

Do this:

```kusto
dataset="<my-datasource>" tenantId="foo-bar-12345" proc="bash" data_source="stdout"
```

## Place Intensive Operations Last {#coord-last}

To minimize costs, try to move functions like `lookup` and `sort` to the right-most part of a query. First, filter or aggregate your Dataset to reduce the data volume sent to these functions.

Instead of this: 

```kusto
dataset="<my-datasource>" dataSource=”VPC Flow Logs” | lookup service_names on dst_port | summarize count() by service_name 
```

Do this: 

```kusto
dataset="<my-datasource>" dataSource=”VPC Flow Logs” | summarize count() by dst_port | lookup service_names on dst_port 
```

## Summarize to Search Faster {#summarize}

Any search that returns a large number of raw events back to the UI will be slower than a summary result set.

Instead of this: 

```kusto
dataset="my_data"
```
Do this: 

```kusto
dataset="my_data" | summarize count() by cid
```

## Summarize Before Join {#summarize-join}

When using joins with aggregation operators, it’s more efficient to summarize-then-join rather than join-then-summarize.

Instead of this: 

```kusto
let URLMethods=dataset="access_common_data"; 

dataset="my-datasource" | join URLMethods on URL | summarize count() by method
```

Do this: 

```kusto
let URLMethods=dataset="access_common_data" | summarize count() by URL, method;

dataset="my-datasource" | summarize count() by URL, method | join URLMethods on URL 
```

## Search Faster with Comma Syntax {#commas}

Many operators can perform multiple functions simultaneously if you link your query together using commas instead of pipes.

Instead of this: 

```kusto
... | extend field1=”foo” | extend field2=”bar” | extend field3=”pike”
```

Do this: 

```kusto
... | extend field1=”foo”, field2=”bar”, field3=”pike”
```

## Move Partition Tokens to Queries {#tokens}

Token-based partitions in your Dataset can drastically increase your search time if the directory paths are very broad. If you instead specify the tokens as part of your search, this will reduce the search span to only those sub-trees. Ideally, keep time to the left-most portion of your path, and keep tokens to the right wherever possible, as shown here.

If this is your Dataset definition:

```
data/${dataSource}/${_time:%Y}/${_time:%m}/${_time:%d}/${_time:%H}
```

Then instead of this:

```kusto
dataset="myDataset" | summarize count() by destination | where dataSource=”cisco” 
```

Do this: 

```kusto
dataset="myDataset" dataSource=”cisco” | summarize count() by destination
``` 

## Send Exclusively to Cribl Stream to Speed Up Large Result Sets {#send}

When search results expand beyond several thousand events, sending the results to Cribl Stream via the [`send`](send) operator is faster than returning events to the Cribl Search UI.

Instead of this: 

```kusto
dataset="my_data" | send tee=true
```

Do this: 

```kusto
dataset="my_data" | send
```

## Lakehouse: Use `where` for Case-Sensitive Searches {#where-case}

When performing a case-sensitive search against a [Lakehouse](set-up-cribl-lake#search-lakehouse), using the case-sensitive [`where ==`](where) syntax can provide faster performance than using the implicit [`cribl`](cribl) operator.

Instead of this (implicitly invoking `cribl`, which is case-insensitive): 

```kusto
dataset=my_dataset field1=foo field2=bar
```

Do this: 

```kusto
dataset=my_dataset | where field1=="foo" and field2=="bar"
```

> With the `where` operator, you must place quotes around string literals that you use in comparisons, as shown just above. With the `cribl` operator, the quotes are optional.
{.box .info}

## Optimize Parquet with `project` {#parquet-project}

When searching Parquet files, use [`project`](project) in the second query clause, to narrow the subsequent expressions to only your fields of interest.

Instead of this:

```kusto
dataset="a-parquet-datasource" | summarize sum(bytes) by customer, account 
```

Do this:

```kusto
dataset="a-parquet-datasource" | project bytes, customer, account | summarize sum(bytes) by customer, account 
```
