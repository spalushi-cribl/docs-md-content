# !endswith_cs



The `!endswith_cs` operator applies a case-sensitive ending string filter and excludes events that match.



The following table compares the `endswith` operators using these abbreviations:

- RHS = right-hand side of the expression
- LHS = left-hand side of the expression

| Operator                          | Description                            | Case-Sensitive | Example (yields true)            |
| --------------------------------- | -------------------------------------- | -------------- | -------------------------------- |
| [`endswith`](endswith)            | RHS is a closing subsequence of LHS    | No             | `"Fabrikam" endswith "Kam"`      |
| [`!endswith`](not-endswith)       | RHS isn't a closing subsequence of LHS | No             | `"Fabrikam" !endswith "brik"`    |
| [`endswith_cs`](endswith-cs)      | RHS is a closing subsequence of LHS    | Yes            | `"Fabrikam" endswith_cs "kam"`   |
| [`!endswith_cs`](not-endswith-cs) | RHS isn't a closing subsequence of LHS | Yes            | `"Fabrikam" !endswith_cs "brik"` |


## Syntax

    `Scope | where Field !endswith_cs String`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **String**: The string used to filter.

## Example

```kusto
dataset=myDataset 
| summarize Events=count() by State
| where State !endswith_cs "is"
```

