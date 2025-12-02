# tobool


The `tobool` function converts the input to a value of type [`bool`](bool).

Aliases: [`bool`](bool).

## Syntax

`tobool( Expression )`

### Arguments

- **Expression**: Expression to convert to a [`bool`](bool) value.

## Returns

If the type conversion is successful, returns a [`bool`](bool) value.

If the type conversion fails, returns [`null`](null).

## Examples

| Query             | Result  |
| ----------------- | ------- |
| `tobool("true")`  | `true`  |
| `tobool("TRUE")`  | `true`  |
| `tobool("yes")`   | `true`  |
| `tobool("false")` | `false` |
| `tobool("f")`     | `false` |
| `tobool(1)`       | `true`  |
| `tobool("1")`     | `true`  |
| `tobool(123)`     | `true`  |
| `tobool(0)`       | `false` |
| `tobool(0.0)`     | `false` |
| `tobool(“abc”)`   | `null`  |
| `tobool(null)`    | `null`  |
