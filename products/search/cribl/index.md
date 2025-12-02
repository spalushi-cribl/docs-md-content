# cribl


The `cribl` operator finds specific events. It is the fundamental Cribl Search operator, and is implicit in queries that
do not specify an operator.

## Syntax

`[cribl] StringExpression ComparisonExpression [ BooleanOperator ] [ StringExpression | ComparisonExpression ]`

Either a **StringExpression** or a **ComparisonExpression** is required.

### Rules

- `cribl` is implicit. You don't need to specify it in a query.
- But to use `cribl` in a [subquery](common-examples#subqueries), you must specify it.
- `cribl`, like [`find`](find), can be used to initiate a query expression.
- `cribl` doesn't support functions (or any kind of expressions). For this, you need to use the [`where`](where)
  operator.
- Wildcards `*` represent zero or more characters.
- An escaped `\*` represents a literal asterisk.
- `cribl` doesn't support including both a wildcard `*` and a literal `\*` in the same query. To perform this type of
  search, you need to use the [`where`](where) operator.
- `cribl` interprets both the `==` and `=` tokens as comparison (not assignment) operators.
- `cribl` implicitly matches against the `_raw` field if you specify no field; otherwise, it matches against an
  extracted field.

### Arguments

The `cribl` operator accepts a flexible range of arguments.

#### StringExpression {#stringexpression}

Case-insensitive string of characters. For example, `"error"` or `error`. To see if you can omit the double quotes `""`,
see [Quotes in `cribl` Operator](#quotes). Either a **StringExpression** or a
[**ComparisonExpression**](#comparisonexpression) is required.

#### BooleanOperator {#booleanoperator}

`not`, `or`, `and`, in that order of precedence. This order differs from other operators. Use parentheses `()` to
override this precedence.

Here, whitespace ` ` is equivalent to `and`. For example, `foo and bar` is the same as `foo bar`.

#### ComparisonExpression {#comparisonexpression}

Compare numbers, strings, and/or [regular expressions](where#regex-examples); perform evaluations that evaluate to a
boolean. Either a **ComparisonExpression** or a [**StringExpression**](#stringexpression) is required.

Syntax: `Key ComparisonOperator Value` or `Key in (ListOfValues)` or `TimeExpression`

We break down these parameters below.

##### Key {#key}

This is a field name.

##### Comparison Operators in `cribl` {#comparisonoperator}

Formal comparison operators are `=`, [`==`](equals-cs), `=~`, [`!=`](not-equals-cs), `!==`, `>`, `>=`, `<`, `<=`,
[`=~`](equals), and [`!~`](not-equals). (For comparisons against regex literals, we support only the [`==`](equals-cs),
[`=~`](equals), [`!=`](not-equals-cs), and [`!~`](not-equals) operators.) See also [String Operators](#string-operators)
below.



| Operator | Description | Case-Sensitive | Wildcards | Regex | Examples  | Wildcard Example | Regex Example  |
| ---------------------------------------- | --------------------------------------------------------------------------------- | -------------- | --------- | ----- | ------------------------------------ | ---------------- | --------------------- |
| Equal [`=`](equals) or [`==`](equals-cs) | Returns `true` if the operands are equal.                                         | No             | Yes       | Yes   | <div style="width: 100px"> `3 == var1`<br> `"3" == var1`<br> `3 == '3'` | `foo == "abc*"`  | `foo == /^abc.*xyz$/` |
| Not equal [`!=`](not-equals-cs)          | Returns `true` if the operands are not equal.                                     | No             | Yes       | Yes   | `var1 != 4`<br> `var2 != "3"`           | `foo != "abc*"`  | `foo != /^abc.*xyz$/` |
| Greater than `>`                         | Returns `true` if the left operand is greater than the right operand.             | No             | No        | No    | `var2 > var1` `"12" > 2`|    |   |
| Less than `<`                            | Returns `true` if the left operand is less than the right operand.                | No             | No        | No    | `var1 < var2`<br> `"2" < 12` |   |   |
| Greater than or equal to `>=`            | Returns `true` if the left operand is greater than or equal to the right operand. | No             | No        | No    | `var2 >= var1`<br> `var1 >= 3`  |    |   |
| Less than or equal to `<=`   | Returns `true` if the left operand is less than or equal to the right operand.    | No  | No        | No    | `var1 <= var2`<br> `var2 <= 5`   |   |   |
| `!`                                      | Unary inversion (NOT) – logical values only; string values are unsupported        | No             | Yes       | Yes   |     |   |   |

###### String Operators in `cribl` {#string-operators}

The **ComparisonOperator** parameter also supports the following string operators.

| Operator | Description | Case-Sensitive | Wildcards | Regex | Example (yields `true`) | Regex Example  |
| ------------------ | --------------------- | -------------- | --------- | ----- | ----------------------- | ---------------------- |
| [`in`](in-cs) | Equal to any of the events | Yes | No | Yes | `"abc" in ("123", "345", "abc")` | `foo in ("ye*ah", /^no.+way/i, 'whee')`   |
| [`!in`](not-in-cs) | Not equal to any of the events | Yes | No | Yes | `"bca" !in ("123", "345", "abc")` | `foo !in ("ye*ah", /^no.+way/i, 'whee')`  |
| [`in~`](in) | Equal to any of the events | No | No | Yes | `"Abc" in~ ("123", "345", "abc")` | `foo in~ ("ye*ah", /^no.+way/i, 'whee')`  |
| [`!in~`](not-in) | Not equal to any of the events | No | No | Yes | `"bCa" !in~ ("123", "345", "ABC")` | `foo !in~ ("ye*ah", /^no.+way/i, 'whee')` |

##### Value {#value}

This is a [**StringExpression**](#stringexpression).

- **in**: [`in`](in-cs) (or [`!in`](not-in-cs), or [`in~`](in), or [`!in~`](not-in)) filters events with
  **ListOfValues**.

##### ListOfValues {#listofvalues}

This is a comma-separated list of one or more [**StringExpression**](#stringexpression)s. For example:  
 `(StringExpression, StringExpression, ...)`

##### TimeExpression {#timeexpression}

Relative or absolute time range. Syntax: `earliest=TimeString` and/or `latest=TimeString`. `earliest` is inclusive `>=`,
while `latest` is exclusive `<`. The limit of `earliest` is 90 days (7776000 seconds) ago. See
[time syntax](#time-syntax) for details.

##### RegularExpression {#regularexpression}

Regex literal. Syntax: `/<regular expression>/<flags>`. For syntax details, see [Regex Examples](where#regex-examples),
[Regex Flags](where#flags), and [Disambiguating Regex Characters](where#regex-distinguish).

##### ComparisonExpression Examples {#comparison-examples}

- `foo=bar`, `foo23.4`, `bar42`, `foo!=bar*`
- `level in (INFO, DEBUG, ERROR)`
- `method in ("GET", "POST")`
- `earliest=-1h`, `latest=-42m`, `earliest=-42m@h latest=now`, `earliest=-2h@m latest=+2h@m`,
  `earliest=1700511360.123 latest=1700511420.123`
- `method == /ge*/i`
- `method =~ /ge*|p.*t/i`
- `method in ("GET", /^po.*/i)`

(In the final example above, note that your **ComparisonExpression** can mix string and regex target values.)

#### Quotes in `cribl` Operator {#quotes}

You **don't** need to enclose your [**StringExpression**](#arguments) in double quotes `""`, as long as it's made of the
following characters only:

- `a`-`z`
- `A`-`Z`
- `0`-`9`
- `$` `_` `*` `.` `-`

If your [**StringExpression**](#arguments) contains any other character, you **do** need to enclose it in double quotes
`""`, for example:

| Characters                                   | Need quotes `""` | Examples                                         |
| -------------------------------------------- | ---------------- | ------------------------------------------------ |
| `@` `+` `-` `\` `/` `(` `)` `[` `]`          | ✓                | `"john.doe@whatever.com"`<br><br>`"/api/events"` |
| Space (` `)                                  | ✓                | `"test event"`                                   |
| Keywords `dataset`, `earliest`, and `latest` | ✓                | `"earliest=testEvent"`                           |
| `a`-`z` `A`-`Z` `0`-`9`                      |                  | `testEvent0` or `"testEvent0"`                   |
| `$` `_` `*` `.` `-`                          |                  | `us-*` or `"us-*"`                               |

Cribl Search keywords, such as operator or function names, don't need double quotes. The only the exceptions are
`dataset`, `earliest`, and `latest`.

To escape double quotes, use the backslash `\`, for example: `"style=\"goatee\""`

### Time Syntax {#time-syntax}

The `TimeString` used for `earliest` and `latest` can be one of the following:

- A relative timestamp, expressed in the human-readable format described below. For example, `earliest=-3h`.
- An absolute timestamp, expressed in Unix time. For example, `earliest=1729290112`.

For relative timestamps, use the following syntax:

`[+|-]TimeNumberTimeUnit[@SnapToTimeUnit]`

| Argument          | Values Supported                                                                                                                                                     |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `+` or `-`        | Optional, defaults to `now`. Use `-` for times in the past, or `+` for times in the future.                                                                          |
| `TimeNumber`      | A positive integer.                                                                                                                                                  |
| `TimeUnit`        | Unit of time. Supports `s[econds]`, `m[inutes]`, `h[ours]`, `d[ays]`, `w[eeks]`, `mon[ths]`, `q[uarters]`, `y[ears]`.                                                |
| `@SnapToTimeUnit` | Optional. Append the `@` modifier, followed by any of the above `TimeUnit`s, to round down to the nearest instance of that unit. (See the next section for details.) |

For absolute timestamps, use the following syntax:

`TimeNumber`

| Argument     | Values Supported                                                                                                                                                                                                                  |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `TimeNumber` | A [Unix time](https://en.wikipedia.org/wiki/Unix_time) value (number of seconds since 1 Jan 1970). Can be either whole seconds (for example, `1700511360`) or fractional to express milliseconds (for example, `1700511420.123`). |

#### SnapToTimeUnit Syntax {#snaptotimeunit}

The `@` snap modifier always rounds **down** (backwards) from any specified time. This is true even in relative time
expressions with `+` (future) offsets. For example:

- `@d` snaps back to the beginning of today, 12:00 AM (midnight) UTC.

- `+128m@h` looks forward 128 minutes, then snaps back to the nearest round hour. (If you specified this in the `Latest`
  field, and ran the Collector at 4:20 PM, collection would end at 6:00 PM. The expression would look forward to 6:28
  PM, but snap back to 6:00 PM.)

Other options:

- `@w` or `@w7` to snap back to the beginning of the week – defined here as the preceding Sunday.
- To snap back to other days of a week, use `w1` (Monday) through `w6` (Saturday).
- `@mon` to snap back to the 1st of a month.
- `@q` to snap back to the beginning of the most recent quarter – Jan. 1, Apr. 1, Jul. 1, or Oct. 1.
- `@y` to snap back to Jan. 1.

## Examples {#examples}

Simple term search over all events in the `default` Dataset:

```kusto
"goats"
```

Looking only for records that match both terms:

```kusto
"goats" and ("climb" or "rock climb")
```

Looking in the `network` field for "sector7":

```kusto
network in ("sector7")
```

Looking in the `region` field for values that start with `us-`:

```kusto
region in (us-*)
```

Restricting the search to certain fields:

```kusto
disk="f9" or process="index"
```

Specific time limit:

```kusto
earliest=-2h@h latest=-1h@min
```

Searching Datasets that start with `foo` for specific terms and field values within a specified time range:

```kusto
dataset=foo* (blocked or "access denied") method in (POST, GET) az !in (us-east*, us-west-2) host!=local* earliest=-2h@h latest=-1h@min
```

Return results that match "test event".

```kusto {runSearch=true}
dataset=$vt_dummy event<10
| extend _raw=iif(event%2>0, "This is a test event", "This is another event")
| cribl "test event"
```
