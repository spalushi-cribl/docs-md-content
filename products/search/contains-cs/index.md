# contains_cs



The `contains_cs` operator applies a case-sensitive string filter and returns events that match.



The following table compares the `contains` operators using these abbreviations:

- RHS = right-hand side of the expression
- LHS = left-hand side of the expression

| Operator                          | Description                        | Case-Sensitive | Example (yields true)           |
| --------------------------------- | ---------------------------------- | -------------- | ------------------------------- |
| [`contains`](contains)            | RHS occurs as a subsequence of LHS | No             | `"FabriKam" contains "BRik"`    |
| [`!contains`](not-contains)       | RHS doesn't occur in LHS           | No             | `"Fabrikam" !contains "xyz"`    |
| [`contains_cs`](contains-cs)      | RHS occurs as a subsequence of LHS | Yes            | `"FabriKam" contains_cs "Kam"`  |
| [`!contains_cs`](not-contains-cs) | RHS doesn't occur in LHS           | Yes            | `"Fabrikam" !contains_cs "Kam"` |


## Syntax

    `Scope | where Field contains_cs String`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **String**: The string used to filter.

## Example

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where State contains_cs "AS"
```

