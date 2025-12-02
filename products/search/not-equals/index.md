# !~



The `!~` (not equals) operator applies a case-insensitive string filter and excludes events that match.



The following table provides a comparison of the `equal` operators:

| Operator              | Description | Case-Sensitive | Example (yields `true`) |
| --------------------- | ----------- | -------------- | ----------------------- |
| [`==`](equals-cs)     | Equal       | Yes            | `"aBc" == "aBc"`        |
| [`!=`](not-equals-cs) | Not equal   | Yes            | `"abc" != "ABC"`        |
| [`=~`](equals)        | Equal       | No             | `"abc" =~ "ABC"`        |
| [`!~`](not-equals)    | Not equal   | No             | `"aBc" !~ "xyz"`        |

When comparing values of different [types](types), Cribl Search performs automatic type conversion wherever possible,
giving priority to number comparisons.

For more details on comparison rules, see:

- [Comparison operators of the `cribl` operator](cribl#comparisonoperator)
- [Comparison operators of the `where` operator](where#comparison-operators)


## Syntax

    `Scope | where Field !~ (Expression, ... )`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **Expression**: An expression used to filter.

## Example

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where (State !~ "texas") and (event_count > 3000)
| project State, event_count
```

