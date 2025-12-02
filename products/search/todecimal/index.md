# todecimal


The `todecimal` function converts the input to a value of type [`decimal`](decimal) ([`double`](double), [`real`](real)).

Aliases: [`decimal`](decimal), [`double`](double), [`todouble`](todouble), [`real`](real), [`toreal`](toreal).

## Syntax

`todecimal( Expression )`

### Arguments

- **Expression**: Expression whose value will be converted to a value of type `decimal`.

## Returns

If the type conversion is successful, the result is a value of type [`decimal`](decimal) ([`double`](double), [`real`](real)).

If the type conversion fails, returns [`null`](null).

## Examples

- `todecimal("123.4")` returns `123.4`
- `todecimal("1e3")` returns `1000`
- `todecimal("hello")` returns `null`
