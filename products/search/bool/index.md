# bool


The `bool` data type can have one of two states: `true` or `false`, as well as the [`null`](null) value.

Alias: `boolean`.

## `bool` Definition {#definition}

A value is considered `true` in any of the following cases:

- If it's the actual `bool` value `true`.
- If it's one of the following case-insensitive strings: `'true'`, `'t'`, `'yes'`, `'y'`.
- If it's a numeric value equal to `1` (including `1.0`).

A value is considered `false` in any of the following cases:

- If it's the actual `bool` value `false`.
- If it's one of the following case-insensitive strings: `'false'`, `'f'`, `'no'`, `'n'`.
- If it's a numeric value equal to `0` (including `0.0`).

Any values that couldn't be converted to `bool` are considered [`null`](null).

## Comparing `bool` to Numbers

When comparing `bool` values to numbers, only `1` is considered `true`. For example:

| Expression    | Result  |
| ------------- | ------- |
| `true == 1`   | `true`  |
| `true == 100` | `false` |

However, when converting a value to a `bool`, all non-zero numbers are converted to `true`:

| Expression       | Result  |
| ---------------- | ------- |
| `tobool(100)`    | `true`  |
| `bool(-1)`       | `true`  |
| `iff(100, 1, 0)` | `1`     |
| `not(100)`       | `false` |

## `bool` as a Filter

To search for any values that are considered `true`, filter on the literal `true` value:

- `dataset="foo" something=true`
- `dataset="foo" | where something==true`

These will match on:

- Booleans with value `true`.
- The following case-insensitive strings: `'true'`, `'t'`, `'yes'`, `'y'`.
- Numeric values equal to `1`.

To search for any values that are considered `false`, filter on the literal `false` value:

- `dataset="foo" something=false`
- `dataset="foo" | where something==false`

These will match on:

- Booleans with value `false`.
- The following case-insensitive strings: `'false'`, `'f'`, `'no'`, `'n'`.
- Numeric values equal to `0`.

Note that searching on `"true"` or `"false”` literals with quotes does a string match instead. This means that:

`dataset="foo" | where something=="true"`

Will match on `"true"` and `true`, but not on `1`, `"yes"` or any other `bool` values.

## `bool` as Function Input

For any functions that take `bool` arguments, the values will be automatically converted to `true`, `false` or
[`null`](null), according to our [`bool` definition](#definition). Mind that **all non-zero numbers are converted to
`true`**.

For conditional functions, [`null`](null) is treated as `false`.

| Expression                      | Result    |
| ------------------------------- | --------- |
| `iif(true, 'true', 'false')`    | `'true'`  |
| `iif('yes', 'true', 'false')`   | `'true'`  |
| `iif(1, 'true', 'false')`       | `'true'`  |
| `iif(123, 'true', 'false')`     | `'true'`  |
| `iif('false', 'true', 'false')` | `'false'` |
| `iif(0, 'true', 'false')`       | `'false'` |
| `iif('abc', 'true', 'false')`   | `'false'` |

## `bool` and the Logical NOT Operator

Both the logical NOT `!` operator and the [`not`](not) function:

- Return `false` for all values considered `true` by the [`bool` definition](#definition). Mind that **all non-zero
  numbers are converted to `true`**.
- Return `true` for all values considered `false` by the [`bool` definition](#definition).
- Return [`null`](null) if passed [`null`](null), or for any values that can't be converted to `true` or `false`.

| Expression     | Result  |
| -------------- | ------- |
| `not(true)`    | `false` |
| `not('true')`  | `false` |
| `not('yes')`   | `false` |
| `not(1)`       | `false` |
| `not(100)`     | `false` |
| `not(false)`   | `true`  |
| `not('false')` | `true`  |
| `not('no')`    | `true`  |
| `not(0)`       | `true`  |
| `not(null)`    | `null`  |
| `not("hello")` | `null`  |
