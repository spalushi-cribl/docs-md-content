# let


The `let` statement assigns a name to a value, expression, or an entire query.

You can then [reference](common-examples#let-subqueries) that name in the context of the same search. This
allows you to write more complex queries that consist of several stages and can reuse their results.

`let` statements are often used together with the [`join`](join) and [`union`](union) operators.


> You can also perform unions and joins by using [inline subqueries](common-examples#inline-subqueries).
{.box .info}

## Syntax

`let Name = Expression;`

Each `let` statement needs to end with a semicolon (`;`).

### Arguments

- **Name**: String. The name to assign to the **Expression**. Spaces (` `), and the keyword `root` are not allowed.
- **Expression**: The value to assign to the **Name**. Can be a single value, an expression, or an entire query.

## Examples

Return no more than 1000 events.

```kusto {runSearch=true}
let x = 1000;

dataset="cribl_search_sample"
 | limit x
```

Find all VPC flow logs in the `cribl_search_sample` Dataset that are DNS-related requests.

```kusto {runSearch=true}
// returns 1 record (a scalar)
let dns = dataset="cribl_lookups" lookup_table="service_names_port_numbers" service_name="domain" transport="tcp";

dataset="cribl_search_sample" dataSource='vpcflowlogs'
 | where dstport == dns.port_number
```

Join internal Cribl logs related to HTTP requests with a Dataset containing HTTP status codes.

```kusto {runSearch=true}
let codeMap = dataset=$vt_dummy event < 4
 | extend x =
   case(event == 0, dynamic({"status": 200, "text": 'OK'}),
        event == 1, dynamic({"status": 308, "text": 'Redirect'}),
        event == 2, dynamic({"status": 404, "text": 'Not found'}),
                    dynamic({"status": 500, "text": 'Internal Error'}))
 | project status=x.status, statusText=x.text;

dataset="cribl_internal_logs" method status
 | where isnotnull(status)
 | join kind=leftouter codeMap on status
```

Calculate the 95th percentile of requests in the Cribl internal logs.

```kusto {runSearch=true}
let num_percentile = 95;

let response_time_percentile = dataset="cribl_internal_logs" method status response_time
 | summarize total=count(), response_time=percentile(response_time, num_percentile);

let long_responses = dataset="cribl_internal_logs" method status response_time
 | where response_time > response_time_percentile.response_time
 | summarize count=count(), maxResponseTime=max(response_time);

print strcat('There are ', long_responses.count, ' requests in the ', num_percentile, ' percentile. Total requests were: ', response_time_percentile.total, '. Highest response time was: ', long_responses.maxResponseTime, 'ms');
```

