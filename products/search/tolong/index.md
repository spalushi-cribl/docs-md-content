# tolong


The `tolong` function converts the input to a value of type [`long`](long) ([`int`](int)).

Aliases: [`long`](long), [`int`](int), [`toint`](toint).

## Syntax

`tolong( Expression )`

### Arguments

- **Expression**: Expression to convert to a value of type [`long`](long).

## Returns

If the type conversion is successful, returns a [`long`](long) number. Decimal places are truncated.

If the type conversion fails, returns `null`.

## Example

- `tolong(123)` returns `123`
- `tolong("123.456")` returns `123`
- `tolong("abc")` returns `null`
