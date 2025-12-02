# join


The `join` operator merges events from two different data scopes.

You can merge data coming from different [Datasets](basic-concepts#datasets) and [Dataset Providers](basic-concepts#dataset-providers), and then
process the output for further analysis.

## Syntax

Using a [`let`](let) statement:

```kusto
let RightScopeName = RightScope;

LeftScope
  | join [ JoinOptions ] RightScopeName on JoinConditions
```

Using an [inline subquery](common-examples#inline-subqueries):

```kusto
LeftScope
  | join [ JoinOptions ] (
    RightScope
    )
    on JoinConditions
```

### Arguments

- **RightScopeName**: The name for the **RightScope**. Spaces (` `) are not allowed.
- **RightScope**: The data on the right-hand side of the join operation. Needs to be assigned a name, using the
  [let](let) statement. Can be a single value, an expression, or an entire query, for example:
  `dataset="cribl_internal_logs" | limit 1000`.
- **LeftScope**: The data on the left-hand side of the join operation. Can be a single value, an expression, or an
  entire query.
- **JoinOptions**: The join operation's optional settings. Syntax: `option1=value1[, option2=value2, ...]`. The
  following options are available:
  - `kind=String`: The [type](#types-of-joins) of join to perform. Can be `inner`, `innerunique`, `leftanti`, or `leftouter`. Default:
    `inner`.
  - `overwrite=Bool`: Applies to fields with the same name on both **LeftScope** and **RightScope**. If set to `true`, this operator uses the values of the **RightScope** fields. If `false`, it uses the values of the  **LeftScope** fields. Default: `false`.
  - `caseInsensitive=Bool`: If `true`, the join operation is case-insensitive. Default: `false`.
- **JoinConditions**: The conditions on which the events are merged. Separate multiple conditions with a comma (`,`).
  Use the following syntax:
  - `FieldName`, if the fields you want to match have the same name in both Datasets.
  - `$left.LeftFieldName == $right.RightFieldName`, if you want to match fields that have different names.

### Rules

The **RightScope** must:

- Either be assigned a name, using a [let](let) statement.
- Or be an [inline subquery](common-examples#inline-subqueries).

The join operation includes the first 50,000 events of the **RightScope**. The remaining events are ignored.

## Types of Joins

By default, Cribl Search performs an [**inner**](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/join-inner) join, which returns the
following:

| Scope          | What's included in the output                                      |
| -------------- | ------------------------------------------------------------------ |
| **LeftScope**  | Events that match the join conditions, including the matching keys |
| **RightScope** | Events that match the join conditions, including the matching keys |

To perform an [**innerunique**](https://learn.microsoft.com/en-us/kusto/query/join-innerunique) join, use the `kind=innerunique` setting, which returns the following:

| Scope          | What's included in the output                                             |
| -------------- | ------------------------------------------------------------------------- |
| **LeftScope**  | Unique events that match the join conditions, including the matching keys |
| **RightScope** | Unique events that match the join conditions, including the matching keys |

To perform a [**leftanti**](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/join-leftanti)
join, use the `kind=leftanti` setting, which returns the following:

| Scope          | What's included in the output               |
| -------------- | ------------------------------------------- |
| **LeftScope**  | Events that don't match the join conditions |
| **RightScope** | No events       

To perform a [**leftouter**](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/join-leftouter) join, use the
`kind=leftouter` setting, which returns the following:

| Scope          | What's included in the output                                      |
| -------------- | ------------------------------------------------------------------ |
| **LeftScope**  | All events                                                         |
| **RightScope** | Events that match the join conditions, including the matching keys |                            |

## Examples

Using an [inline subquery](common-examples#inline-subqueries), join internal Cribl logs related to HTTP requests with a
Dataset containing HTTP status codes.

```kusto {runSearch=true}
// LeftScope
dataset="cribl_internal_logs" method status
 | where isnotnull(status)
 | join kind=leftouter (
    // RightScope (inline subquery)
    cribl dataset=$vt_dummy event < 4
     | extend x =
            case(event == 0, dynamic({"status": 200, "text": 'OK'}),
                 event == 1, dynamic({"status": 308, "text": 'Redirect'}),
                 event == 2, dynamic({"status": 404, "text": 'Not found'}),
                             dynamic({"status": 500, "text": 'Internal Error'}))
     | project statusNum=x.status, statusText=x.text
 ) on $left.status == $right.statusNum
```

Join two scopes of data, based on a single namesake field (`status`). Returns only those `LeftScope` events that have
`status`. Those events are also enriched with the `status` fields from the `RightScope`.

```kusto {runSearch=true}
// RightScope
let codeMap = dataset=$vt_dummy event < 4
 | extend x =
   case(event == 0, dynamic({"status": 200, "text": 'OK'}),
        event == 1, dynamic({"status": 308, "text": 'Redirect'}),
        event == 2, dynamic({"status": 404, "text": 'Not found'}),
                    dynamic({"status": 500, "text": 'Internal Error'}))
 | project status=x.status, statusText=x.text;

dataset="cribl_internal_logs" method status
 | where isnotnull(status) // LeftScope
 | join kind=leftouter codeMap on status
```

Show blocklisted IPs in VPC flow logs and sensitive ports their network requests are accessing.

```kusto {runSearch=true}
// first RightScope (dummy list of blocklisted IPs)
let pretendBlocklistedIps = dataset="cribl_search_sample" dataSource="vpcflowlogs"
 | limit 10
 | distinct srcaddr
 | project blocklistedIp=srcaddr;

// second RightScope
let servicePortsToWatch = dataset="cribl_lookups" lookup_table="service_names_port_numbers" and transport='tcp' and port_number in (20, 21, 22, 443)
 | project port_number, service_name;

// LeftScope
dataset="cribl_search_sample" dataSource="vpcflowlogs"
 | limit 1000
 | join pretendBlocklistedIps on $left.srcaddr == $right.blocklistedIp
 | join servicePortsToWatch on $left.dstport == $right.port_number;
```

Perform a join with the `overwrite` option set to `true`. Returns only those `LeftScope` events that have the `name`
field, but the `RightScope` `name` values overwrite the `LeftScope` `name` values.

```kusto {runSearch=true}
let RightScope = dataset=$vt_dummy event < 10
 | extend name=strcat('NameFromRightScope', event);

dataset=$vt_dummy event < 10
 | extend name=strcat('NameFromLeftScope', event) // LeftScope
 | join overwrite=true RightScope on event;
```

Create a code map for different event statuses and then join it with another Dataset to produce a distinct list of methods along with their corresponding status codes and descriptive texts.

```kusto {runSearch=true}
let codeMap = dataset=$vt_dummy event < 4
 | extend x =
   case(event == 0, dynamic({"status": 200, "text": 'OK'}),
        event == 1, dynamic({"status": 308, "text": 'Redirect'}),
        event == 2, dynamic({"status": 404, "text": 'Not found'}),
                    dynamic({"status": 500, "text": 'Internal Error'}))
 | project status=x.status, statusText=x.text;

dataset="cribl_int*" method status
 | where isnotnull(status)
 | join codeMap on status
 | distinct method, status, statusText;
```

