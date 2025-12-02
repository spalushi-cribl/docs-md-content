# toint


The `toint` function converts the input to a value of type [`int`](int) ([`long`](long)).

Aliases: [`int`](int), [`long`](long), [`tolong`](tolong).

## Syntax

`toint( Expression )`

### Arguments

- **Expression**: Expression to convert to a value of type [`int`](int).

## Returns

If the type conversion is successful, returns an [`int`](int) value. Decimal places are truncated.

If the type conversion fails, returns `null`.

## Examples

- `toint(123)` returns `123`
- `toint("123.456")` returns `123`
- `toint("abc")` returns `null`
