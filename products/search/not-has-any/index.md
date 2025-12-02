# !has_any


The `!has_any` operator applies any case-insensitive string filters and excludes events that do not match.

## Syntax

    `Scope | where Field !has_any (Expression, ... )`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **Expression**: An expression that specifies the values to search. An expression can be a [type](types) value or expression that produces a set of values.

## Example

```kusto
dataset=myDataset 
| where State !has_any ("CAROLINA", "DAKOTA", "NEW") 
| summarize count() by State
```
