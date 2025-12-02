# !=


The `!=` (not equals) operator applies a case-sensitive string, or regex literal, filter, and excludes events that match.



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

    `Scope | where Field != (Expression, ... )`

### Arguments

* **Scope**: The input tabular result set to filter.
* **Field**: The field to filter.
* **Expression**: An expression used to filter.

  > To pass regex literals as **Expression** arguments, see syntax details at [Regex Examples](regex-matching#regex-examples), [Regex Flags](regex-matching#flags), and [Disambiguate Regex Characters](regex-matching#regex-distinguish).
  >
  {.box .success}

## Example

```kusto
dataset=myDataset 
| where state != "Kansas" 
| project EventId, State
```

