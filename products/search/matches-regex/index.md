# matches regex


The `matches regex` operator applies a case-sensitive regex, or a regex literal (with specified case-sensitivity), and returns events that match.

## Syntax

    `Scope | where Field matches regex Expression`

> For detailed syntax, see [Regex Flags](regex-matching#flags) and [Disambiguate Regex Characters](regex-matching#regex-distinguish).
>
{.box .info}

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **Expression**: The string or regex literal to search.

## Examples

Match a string, with wildcard:

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where State matches regex "K.*S"
```

Match a regex literal (all `POST` requests, case-insensitive):

```kusto {runSearch=true}
dataset="cribl_internal_logs" method=* 
| limit 1000
| where method matches regex /^po.*/i
```

> For further examples using the `where` operator, see [Regex Examples](regex-matching#regex-examples).
>
{.box .info}