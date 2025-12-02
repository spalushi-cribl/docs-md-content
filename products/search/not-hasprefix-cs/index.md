# !hasprefix_cs



The `!hasprefix_cs` operator applies a case-sensitive starting string filter and excludes events that match. Any of the words in the string can have the prefix.



The following table compares the `hasprefix` operators using these abbreviations:

- RHS = right-hand side of the expression
- LHS = left-hand side of the expression

| Operator                            | Description                    | Case-Sensitive | Example (yields true)                |
| ----------------------------------- | ------------------------------ | -------------- | ------------------------------------ |
| [`hasprefix`](hasprefix)            | RHS is a term prefix in LHS    | No             | `"North America" hasprefix "ame"`    |
| [`!hasprefix`](not-hasprefix)       | RHS isn't a term prefix in LHS | No             | `"North America" !hasprefix "mer"`   |
| [`hasprefix_cs`](hasprefix-cs)      | RHS is a term prefix in LHS    | Yes            | `"North America" hasprefix_cs "Ame"` |
| [`!hasprefix_cs`](not-hasprefix-cs) | RHS isn't a term prefix in LHS | Yes            | `"North America" !hasprefix_cs "CA"` |


## Syntax

    `Scope | where Field !hasprefix_cs Expression`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **Expression**: The expression for which to search.

## Example

```kusto
dataset=myDataset 
| summarize Events=count() by State
| where State !hasprefix_cs "P"
```

