# !hassuffix_cs



The `!hassuffix_cs` operator applies a case-sensitive ending string filter and excludes events that match. Any of the words in the string can have the suffix.



The following table compares the `hassuffix` operators using these abbreviations:

- RHS = right-hand side of the expression
- LHS = left-hand side of the expression

| Operator                            | Description                    | Case-Sensitive | Example (yields true)                 |
| ----------------------------------- | ------------------------------ | -------------- | ------------------------------------- |
| [`hassuffix`](hassuffix)            | RHS is a term suffix in LHS    | No             | `"North America" hassuffix "ica"`     |
| [`!hassuffix`](not-hassuffix)       | RHS isn't a term suffix in LHS | No             | `"North America" !hassuffix "americ"` |
| [`hassuffix_cs`](hassuffix-cs)      | RHS is a term suffix in LHS    | Yes            | `"North America" hassuffix_cs "ica"`  |
| [`!hassuffix_cs`](not-hassuffix-cs) | RHS isn't a term suffix in LHS | Yes            | `"North America" !hassuffix_cs "icA"` |


## Syntax

    `Scope | where Field !hassuffix_cs Expression`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **Expression**: The scalar or literal expression to search.

## Example

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where State !hassuffix_cs "AS"
```

