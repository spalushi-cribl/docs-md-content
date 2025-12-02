# null


The `null` value in Cribl Search can indicate:

- The absence of a field or value.
- A type mismatch in a conversion, operation, or function. For example, `toint('abc')` and `123 / 'hello'` both evaluate
  to `null` because strings can't be converted to numbers.

## `null` Literals {#literals}

A `null` literal is represented in Cribl Search as `null`. For example:

- `where field==null`
- `extend result=iff(value > 100, value, null)`

## `null` Equality and Inequality {#equality}

Cribl Search supports the `null` value in equality/inequality comparisons. These comparisons are equivalent to using the
[`isnull`](isnull) and [`isnotnull`](isnotnull) functions.


> This is different from standard Kusto Query Language (KQL), which returns `null` if both sides of the comparison are `null`.
{.box .info}

See the following examples:

| Expression                 | Result  |
| -------------------------- | ------- |
| `5 == null`                | `false` |
| `5 != null`                | `true`  |
| `null == 5`                | `false` |
| `'abc' == null`            | `false` |
| `'null' == null`           | `false` |
| `0 == null`                | `false` |
| `int('abc') == null`       | `true`  |
| `int('abc') == int('def')` | `true`  |

## `null` and Strings {#strings}

Cribl Search supports the `null` value in [`string`](string) comparisons.


> This is different from standard Kusto Query Langauge (KQL), which does not allow the `null` value for strings.
{.box .info}

See the following examples:

| Expression       | Result  |
| ---------------- | ------- |
| `tostring(null)` | `null`  |
| `string(null)`   | `null`  |
| `isnull('abc')`  | `false` |
| `'abc' == null`  | `false` |

## `null` and Binary Operators {#binary}

Binary operators are those that perform an operation on two values. These operators include `and`, `or`, `==`, `!=`,
`>=`, `<=`, numeric operators (`+`, `-`, `*`), and others.

If one or both of the values passed to the binary operator are `null`, then the output of the binary operator is also
`null`. For example, `null + 10` returns `null`.

Exceptions to the rule above are:

- For the equality `==` and inequality `!=` operators, either or both sides of the operation can be `null`, and will
  return `true` or `false` rather than `null` (as described in [`null` Equality and Inequality](#equality)).
- For the logical `AND` operator, if one of the values is `false`, the result is also `false`.
- For the logical `OR` operator, if one of the values is `true`, the result is also `true`.

See the following examples:

| Expression           | Result  |
| -------------------- | ------- |
| `5 > 3`              | `true`  |
| `null > 3`           | `null`  |
| `null > int('abc')`  | `null`  |
| `null == int('abc')` | `true`  |
| `null >= int('abc')` | `true`  |
| `null >= 3`          | `null`  |
| `null > int('abc')`  | `null`  |
| `10 + null`          | `null`  |
| `10 + 'abc'`         | `null`  |
| `10 - ''`            | `null`  |
| `10 - null`          | `null`  |
| `10 + unknown_field` | `null`  |
| `10 - toint('abc')`  | `null`  |
| `null * 10`          | `null`  |
| `null > 3 and true`  | `null`  |
| `null > 3 and false` | `false` |
| `null > 3 or true`   | `true`  |
| `null > 3 or false`  | `null`  |

## `null` and the `where` Operator

The [`where`](where) operator treats `null` as `false`. This means that events with conditions evaluating to `null`
won't appear in the search results.

For example, `dataset="foo" | where tobool("abc")` returns no results because `tobool("abc")` evaluates to `null`.

## `null` and the Logical `NOT` Operator

If the value passed to the logical `NOT` (`!`) operator is `null`, then the output is also `null`.

| Expression        | Result  |
| ----------------- | ------- |
| `not(true)`       | `false` |
| `not(false)`      | `true`  |
| `not(null)`       | `null`  |
| `not(int('abc'))` | `null`  |
| `!int('abc')`     | `null`  |
