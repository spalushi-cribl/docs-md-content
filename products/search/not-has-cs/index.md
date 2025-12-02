# !has_cs



The `!has_cs` operator applies a case-sensitive string filter and excludes events that match.



The following table compares the `has` operators using these abbreviations:

- RHS = right-hand side of the expression
- LHS = left-hand side of the expression

| Operator                | Description                                                   | Case-Sensitive | Example (yields true)              |
| ----------------------- | ------------------------------------------------------------- | -------------- | ---------------------------------- |
| [`has`](has)            | Right-hand-side (RHS) is a whole term in left-hand-side (LHS) | No             | `"North America" has "america"`    |
| [`!has`](not-has)       | RHS isn't a full term in LHS                                  | No             | `"North America" !has "amer"`      |
| [`has_cs`](has-cs)      | RHS is a whole term in LHS                                    | Yes            | `"North America" has_cs "America"` |
| [`!has_cs`](not-has-cs) | RHS isn't a full term in LHS                                  | Yes            | `"North America" !has_cs "amer"`   |


## Syntax

    `Scope | where Field !has_cs String`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **String**: The string used to filter.

## Example

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where State !has_cs "New"
```

