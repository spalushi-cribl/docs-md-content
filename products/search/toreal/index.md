# toreal


The `toreal` function converts the input to a value of type [`real`](real) ([`double`](double), [`decimal`](decimal)).

Aliases: [`real`](real), [`decimal`](decimal), [`todecimal`](todecimal), [`double`](double), [`todouble`](todouble).

## Syntax

`toreal( Expression )`

### Arguments

- **Expression**: Expression whose value will be converted to a value of type [`real`](real).

## Returns

If the type conversion is successful, returns a value of type [`real`](real) ([`double`](double), [`decimal`](decimal)).

If the type conversion fails, returns [`null`](null).

## Example

- `toreal("123.4")` returns `123.4`
- `toreal("1e3")` returns `1000`
- `toreal("hello")` returns `null`
