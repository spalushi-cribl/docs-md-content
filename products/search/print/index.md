# print


The `print` operator evaluates one or more scalar expressions, and returns the results as a single row.

Use `print` to verify an expression before you include it in a larger query.


> To produce sample events, use the [`$vt_dummy`](vt_dummy) virtual table.
>
{.box .success}

## Syntax

`print [FieldName =] Expression [, ...]`

### Arguments

- **FieldName**: The name to assign to the output field. If omitted, the name is generated from the corresponding
  **Expression**.
- **Expression**: The scalar expression to evaluate. A scalar expression is any combination of values and operators that
  evaluates to a single value. For example, `1 + 2` is a scalar expression that evaluates to `3`.

## Results

Returns a single row with as many columns as there are **Expression** arguments.

## Examples

- Check a calculation result.

  ```kusto
  // returns 6
  print 1 + 2 + 3
  ```

- Check a string concatenation.

  ```kusto
  // returns "Hello, World!"
  print strcat("Hello", ", ", "World!")
  ```

- Check multiple expressions, displaying the results as a single event.

  ```kusto
  // returns 200, "56", and 1692736230.271
  print ["Check result"] = iif(false, 100 , 200), Check = substring("456", 2), ago(1h)
  ```

- Check an expression, displaying the results as multiple events.

  ```kusto
  // returns two events with "Word" and "Index" fields:
  // Word: "foo", Index: 0
  // Word: "bar", Index: 1
  print Word=split("foo bar", " ")
  | mv-expand with_itemindex=Index Word
  ```

- Print a single event with field `foo` set to 42.
  ```kusto {runSearch=true}
  print foo=42
  ```

