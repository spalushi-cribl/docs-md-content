# todouble


The `todouble` function converts the input to a value of type [`double`](double) ([`real`](real), [`decimal`](decimal)).

Aliases: [`double`](double), [`decimal`](decimal), [`todecimal`](decimal), [`real`](real), [`toreal`](toreal).

## Syntax

`todouble( Expression )`

### Arguments

- **Expression**: Expression whose value will be converted to a value of type `double`.

## Returns

If the type conversion is successful, the result is a value of type [`double`](double) ([`real`](real), [`decimal`](decimal)).

If the type conversion fails, returns [`null`](null).

## Example

- `todouble("123.4")` returns `123.4`
- `todouble("1e3")` returns `1000`
- `todouble("hello")` returns `null`
