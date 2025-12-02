# tostring


The `tostring` function converts the input to a value of type [`string`](string).

## Syntax

`tostring( Expression )`

### Arguments

- **Expression**: Expression to convert to a string.

## Returns

If the **Expression** value is non-null, returns a string representation of **Expression**.

If the **Expression** value is [`null`](null), returns [`null`](null) as well.


> This is different from standard Kusto Query Language (KQL), in which `tostring` returns an empty string for `null` values.
{.box .info}

## Example

- `tostring(123)` returns `"123"`
- `tostring("hello")` returns `"hello"`
- `tostring(null)` returns `null`
