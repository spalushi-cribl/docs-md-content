# gettype


The `gettype` function returns the [type](types) of the input value.

## Syntax

`gettype( Expression )`

### Arguments

- **Expression**: The value whose type you want to check.

## Returns

Depending on the input, returns one of the following strings:

| Input                         | Returned string                                                          | Input examples                |
| ----------------------------- | ------------------------------------------------------------------------ | ----------------------------- |
| A boolean                     | `bool`                                                                   | `true`, `false`, `3 < 5`      |
| A number                      | `int` for values in range `-2^31…2^31-1`.                                | `3`, `1000`                   |
|                               | `long` for values in range `-2^63…2^63-1` but greater than `int`.        | `2 ** 40`                     |
|                               | `decimal` for values greater than `long`.                                | `2 ** 65`                     |
|                               | `real`                                                                   | `3.15`, `3 / 5`               |
|                               | `NaN`                                                                    | `NaN`                         |
| A `bigint`                    | Same as a number: `int`, `long`, or `decimal`, based on the value range. | `BigInt(1234567890123456789)` |
| A string                      | `string`                                                                 | `'Aloa'`                      |
| `[]`                          | `array`                                                                  | `[17, 3]`                     |
| `{}`                          | `dictionary`                                                             | `{ foo: 'bar' }`              |
| `null`                        | `null`                                                                   | `null`                        |
| Undefined                     | `null`                                                                   | Undefined field reference     |
| Anything else, like functions | `unknown`                                                                | [`coalesce(33)`](coalesce)    |

## Examples

| Input             | Output       |
| ----------------- | ------------ |
| `true`            | `bool`       |
| `false`           | `bool`       |
| `0`               | `int`        |
| `42`              | `int`        |
| `2 ** 40`         | `long`       |
| `BigInt(2 ** 60)` | `long`       |
| `3.1415`          | `real`       |
| `2 ** 65`         | `decimal`    |
| `BigInt(2 ** 65)` | `decimal`    |
| `[17, 3]`         | `array`      |
| `{ foo: "bar" }`  | `dictionary` |
| `Number.NaN`      | `NaN`        |
| `null`            | `null`       |
| Undefined         | `null`       |
| `() => 42`        | `unknown`    |

