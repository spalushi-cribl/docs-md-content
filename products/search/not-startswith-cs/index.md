# !startswith_cs



The `!startswith_cs` operator applies a case-sensitive string starting sequence filter and excludes events that match.



The following table compares the startswith operators using these abbreviations:

- RHS = right-hand side of the expression
- LHS = left-hand side of the expression

| Operator                              | Description                             | Case-Sensitive | Example (yields true)             |
| ------------------------------------- | --------------------------------------- | -------------- | --------------------------------- |
| [`startswith`](startswith)            | RHS is an initial subsequence of LHS    | No             | `"Fabrikam" startswith "fab"`     |
| [`!startswith`](not-startswith)       | RHS isn't an initial subsequence of LHS | No             | `"Fabrikam" !startswith "kam"`    |
| [`startswith_cs`](startswith-cs)      | RHS is an initial subsequence of LHS    | Yes            | `"Fabrikam" startswith_cs "Fab"`  |
| [`!startswith_cs`](not-startswith-cs) | RHS isn't an initial subsequence of LHS | Yes            | `"Fabrikam" !startswith_cs "fab"` |


## Syntax

    `Scope | where Field !startswith_cs String`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **String**: The string used to filter.

## Example

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where State !startswith_cs "Lo"
```

