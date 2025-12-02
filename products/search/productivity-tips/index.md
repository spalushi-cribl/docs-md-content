# Productivity Tips


Efficiently view your data and format search queries.

---

These tips help you make the most of Cribl Search.

## Preview Search Modifications {#preview}

Use the Operator [Preview](build-a-search#preview) feature to test adding new operators to a previously run query, Preview works with a subset of your retrieved data, so you can quickly model the changes without incurring the time or cost of rerunning a search until you’re ready.

1. Add an operator to your search (for example, `| extend` or `| extract`).

2. Hover over the operator keyword and select the **Preview** button.

3. Update your search in the resulting **Preview** modal, then select its own **Preview** button to preview the results on up to 100 rows of of your data.

4. When you have validated your new query, select **Apply**. This sends the modified search back to your query box.

## Quickly Format Queries {#format}

To make your search expression more readable in the query box, press the `Ctrl` + `|` (Linux, Windows) or `Command` + `|` (MacOS) key combination. This wraps the query at each pipe symbol.

Instead of this: 

```kusto
dataset="<my-datasource>" dataSource=”VPC Flow Logs” | summarize count() by dst_port | lookup service_names on dst_port
```

See this: 

```kusto
dataset="<my-datasource>" dataSource=”VPC Flow Logs”
| summarize count() by dst_port
| lookup service_names on dst_port 
```

## Simplify Queries with Reusable Macros {#macros}

Cribl Search [Macros](macros) enable you to store snippets of strings – and optionally, variables – that you can reuse in multiple queries.

Instead of this: 

```kusto
dataset="aws_ftc_audit_accesslogs" 
| extend _raw=replace_regex(_raw, @'""', @'"') 
| extract type=json
```

Insert this: 

```kusto
${ftc_audit_accesslogs}
```

Instead of this: 

```kusto
dataset="aws_ftc_audit_accesslogs" _raw!="\"_time\",source,host,sourcetype,\"_raw\",\"_meta\"" 
| extend _raw=replace_regex(_raw, @'""', @'"') 
| extract type=json
```

Insert this: 

```kusto
${ftc_audit_accesslogs} ${exclude_header}
```

## Manage Lookups {#manage-lookups}

To list lookup files' names and contents, use this simple query:

```kusto
dataset="cribl_lookups"
```

## Filter Data with `.show` {#filter-show}

To see file names in a Dataset:

```kusto {runSearch=true}
.show objects("cribl_search_sample")
```

To see file names that do not end with `.gz`

```kusto {runSearch=true}
.show objects("cribl_search_sample")
| where name !endswith('.gz')
```
