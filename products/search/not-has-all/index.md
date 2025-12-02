# !has_all


The `!has_all` operator applies one or more case-insensitive string filters and excludes events that do not match.

## Syntax

    `Scope | where Field !has_all (Expression, ... )`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **Expression**: An expression that specifies the values to search. An expression can be a [type](types) value or expression that produces a set of values.

## Example

```kusto
dataset=myDataset 
| where EpisodeNarrative !has_all ("cold", "strong", "afternoon", "hail")
| summarize Count=count() by EventType
```
