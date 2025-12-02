# !in~


The `!in~` operator applies a case-insensitive string or regex literal filter, and excludes events that match the specified criteria.



The following table provides a comparison of the `in` operators:

| Operator           | Description                    | Case-Sensitive | Example (yields true)              |
| ------------------ | ------------------------------ | -------------- | ---------------------------------- |
| [`in`](in-cs)      | Equal to one of the events     | Yes            | `"abc" in ("123", "345", "abc")`   |
| [`!in`](not-in-cs) | Not equal to any of the events | Yes            | `"bca" !in ("123", "345", "abc")`  |
| [`in~`](in)        | Equal to any of the events     | No             | `"Abc" in~ ("123", "345", "abc")`  |
| [`!in~`](not-in)   | Not equal to any of the events | No             | `"bCa" !in~ ("123", "345", "ABC")` |


## Syntax

`Scope | where Field !in~ (Expression, ... )`

Or:

`Scope !in~ ListOfValues`

### Arguments

- **Scope**: The input tabular result set to filter.
- **Field**: The field to filter.
- **Expression**: An expression that specifies the values to search. An expression can be a [type](types) value, an expression that produces a set of values, or a regex literal expression that returns a [`bool`](bool) value.
  > To pass regex literals as **Expression** arguments, see syntax details at [Regex Examples](regex-matching#regex-examples), [Regex Flags](regex-matching#flags), and [Disambiguate Regex Characters](regex-matching#regex-distinguish).
  >
  {.box .success}
- **ListOfValues** Comma-separated list of one or more `StringExpression`s. For example,
  `(StringExpression, StringExpression, ...)`.

## Example

The following query shows how to use `!in~` with a list of values.

```kusto
dataset=myDataset
  | where State !in~ ("FLORIDA", "GEORGIA", "NEW YORK")
  | count
```

Filter the `logs` Dataset for `host` fields that don't have a case-insensitive match in the `foo` query.

```kusto
let foo = dataset=api
  | summarize count=count() by hostname

dataset=logs
  | where host !in~ foo.hostname
```

