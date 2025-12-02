# where


The `where` operator filters events based on the specified predicate.

## Syntax

`Scope | where Predicate`

### Arguments

- **Scope**: The events to search.
- **Predicate**: An expression that returns a [`bool`](bool) value for each event. This can match on strings, numeric
  values, or regular expressions.

### Comparison and String Operators in `where` {#operators}

You can construct a **Predicate** using the comparison and string operators shown below. Note the abbreviations
used in these tables:

- Operators with a `_cs` suffix are case-sensitive.
- RHS = right-hand side of the predicate.
- LHS = left-hand side of the predicate.

#### Comparison Operators in `where` {#comparison-operators}

To compare on wildcards (`*`), use the [comparison operators of the `cribl` operator](cribl#comparisonoperator). The
`where` operator supports wildcards only inside regex literals (`/^abc.*xyz$/`).

When comparing values of different [types](types), Cribl Search performs automatic type conversion wherever possible,
giving priority to number comparisons.

Some comparison examples:

- `“1.0” == 1` is `true`
- `“1.0” == “1”` is `true`
- `5 < 100` is `true`
- `“5” < “100”` is `true`
- `“abc” < “100”` is `false`
- `“abc” < 100` is `null`

| Operator              | Description                                                                        | Example (Returns `true`) | Case-Sensitive | Accepts `null` | Supports Regex         | Regex Example         |
| --------------------- | ---------------------------------------------------------------------------------- | ------------------------ | -------------- | -------------- | ---------------------- | --------------------- |
| [`==`](equals-cs)     | Equals                                                                             | `"aBc" == "aBc"`         | Yes            | Yes            | Only in regex literals | `foo == /^abc.*xyz$/` |
| [`!=`](not-equals-cs) | Not equals                                                                         | `"abc" != "ABC"`         | Yes            | Yes            | Only in regex literals | `foo != /^abc.*xyz$/` |
| [`=~`](equals)        | Equals                                                                             | `"abc" =~ "ABC"`         | No             | Yes            | Only in regex literals | `foo =~ /^abc.*xyz$/` |
| [`!~`](not-equals)    | Not equals                                                                         | `"aBc" !~ "xyz"`         | No             | Yes            | Only in regex literals | `foo !~ /^abc.*xyz$/` |
| `>`                   | Greater than                                                                       |                          | –              | No             | –                      | –                     |
| `<`                   | Less than                                                                          |                          | –              | No             | –                      | –                     |
| `>=`                  | Greater than or equal to                                                           |                          | –              | No             | –                      | –                     |
| `<=`                  | Less than or equal to                                                              |                          | –              | No             | –                      | –                     |
| `!`                   | Unary inversion (NOT)<br><br>Logical values only;<br>string values are unsupported |                          | –              | No             | –                      | –                     |

#### String Operators in `where` {#string-operators}

To search on wildcards (`*`), use the [string operators of the `cribl` operator](cribl#string-operators). The `where`
operator supports wildcards only inside regex literals (`/^abc.*xyz$/`).

| Operator                              | Description                                                   | Example (Returns `true`)                              | Case-Sensitive | Regex                  | Regex Example                                |
| ------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------- | -------------- | ---------------------- | -------------------------------------------- |
| [`contains`](contains)                | RHS occurs as a subsequence of LHS                            | `"FabriKam" contains "BRik"`                          | No             | No                     | –                                            |
| [`!contains`](not-contains)           | RHS doesn't occur in LHS                                      | `"Fabrikam" !contains "xyz"`                          | No             | No                     | –                                            |
| [`contains_cs`](contains-cs)          | RHS occurs as a subsequence of LHS                            | `"FabriKam" contains_cs "Kam"`                        | Yes            | No                     | –                                            |
| [`!contains_cs`](not-contains-cs)     | RHS doesn't occur in LHS                                      | `"Fabrikam" !contains_cs "Kam"`                       | Yes            | No                     | –                                            |
| [`endswith`](endswith)                | RHS is a closing subsequence of LHS                           | `"Fabrikam" endswith "Kam"`                           | No             | No                     | –                                            |
| [`!endswith`](not-endswith)           | RHS isn't a closing subsequence of LHS                        | `"Fabrikam" !endswith "brik"`                         | No             | No                     | –                                            |
| [`endswith_cs`](endswith-cs)          | RHS is a closing subsequence of LHS                           | `"Fabrikam" endswith_cs "kam"`                        | Yes            | No                     | –                                            |
| [`!endswith_cs`](not-endswith-cs)     | RHS isn't a closing subsequence of LHS                        | `"Fabrikam" !endswith_cs "brik"`                      | Yes            | No                     | –                                            |
| [`has`](has)                          | Right-hand-side (RHS) is a whole term in left-hand-side (LHS) | `"North America" has "america"`                       | No             | No                     | –                                            |
| `:`                                   | Alias for [`has`](has)                                        | `"North America" : "america"`                         | No             | No                     | –                                            |
| [`!has`](not-has)                     | RHS isn't a full term in LHS                                  | `"North America" !has "amer"`                         | No             | No                     | –                                            |
| [`has_all`](has-all)                  | Same as `has` but works on all of the events                  | `"North and South America" has_all("south", "north")` | No             | No                     | –                                            |
| [`has_any`](has-any)                  | Same as `has` but works on any of the events                  | `"North America" has_any("south", "north")`           | No             | No                     | –                                            |
| [`!has_all`](has-all)                 | Same as `has` but works on all of the events                  | `"North and South America" has_all("south", "north")` | No             | No                     | –                                            |
| [`!has_any`](has-any)                 | Same as `has` but works on any of the events                  | `"North America" has_any("south", "north")`           | No             | No                     | –                                            |
| [`has_cs`](has-cs)                    | RHS is a whole term in LHS                                    | `"North America" has_cs "America"`                    | Yes            | No                     | –                                            |
| [`!has_cs`](not-has-cs)               | RHS isn't a full term in LHS                                  | `"North America" !has_cs "amer"`                      | Yes            | No                     | –                                            |
| [`hasprefix`](hasprefix)              | RHS is a term prefix in LHS                                   | `"North America" hasprefix "ame"`                     | No             | No                     | –                                            |
| [`!hasprefix`](not-hasprefix)         | RHS isn't a term prefix in LHS                                | `"North America" !hasprefix "mer"`                    | No             | No                     | –                                            |
| [`hasprefix_cs`](hasprefix-cs)        | RHS is a term prefix in LHS                                   | `"North America" hasprefix_cs "Ame"`                  | Yes            | No                     | –                                            |
| [`!hasprefix_cs`](not-hasprefix-cs)   | RHS isn't a term prefix in LHS                                | `"North America" !hasprefix_cs "CA"`                  | Yes            | No                     | –                                            |
| [`hassuffix`](hassuffix)              | RHS is a term suffix in LHS                                   | `"North America" hassuffix "ica"`                     | No             | No                     | –                                            |
| [`!hassuffix`](not-hassuffix)         | RHS isn't a term suffix in LHS                                | `"North America" !hassuffix "americ"`                 | No             | No                     | –                                            |
| [`hassuffix_cs`](hassuffix-cs)        | RHS is a term suffix in LHS                                   | `"North America" hassuffix_cs "ica"`                  | Yes            | No                     | –                                            |
| [`!hassuffix_cs`](not-hassuffix-cs)   | RHS isn't a term suffix in LHS                                | `"North America" !hassuffix_cs "icA"`                 | Yes            | No                     | –                                            |
| [`in`](in-cs)                         | Equal to any of the events                                    | `"abc" in ("123", "345", "abc")`                      | Yes            | Only in regex literals | `foo in ("ye*ah", /^no.+way/i, 'whee')`      |
| [`!in`](not-in-cs)                    | Not equal to any of the events                                | `"bca" !in ("123", "345", "abc")`                     | Yes            | Only in regex literals | `foo !in ("ye*ah", /^no.+way/i, 'whee')`     |
| [`in~`](in)                           | Equal to any of the events                                    | `"Abc" in~ ("123", "345", "abc")`                     | No             | Only in regex literals | `foo in~ ("ye*ah", /^no.+way/i, 'whee')`     |
| [`!in~`](not-in)                      | Not equal to any of the events                                | `"bCa" !in~ ("123", "345", "ABC")`                    | No             | Only in regex literals | `foo !in~ ("ye*ah", /^no.+way/i, 'whee')`    |
| [`matches regex`](matches-regex)      | LHS contains a match for RHS                                  | `"Fabrikam" matches regex "b.*k"`                     | Yes            | Only in regex literals | `foo matches regex /^some.+(body\|thing)$/i` |
| [`startswith`](startswith)            | RHS is an initial subsequence of LHS                          | `"Fabrikam" startswith "fab"`                         | No             | No                     | –                                            |
| [`!startswith`](not-startswith)       | RHS isn't an initial subsequence of LHS                       | `"Fabrikam" !startswith "kam"`                        | No             | No                     | –                                            |
| [`startswith_cs`](startswith-cs)      | RHS is an initial subsequence of LHS                          | `"Fabrikam" startswith_cs "Fab"`                      | Yes            | No                     | –                                            |
| [`!startswith_cs`](not-startswith-cs) | RHS isn't an initial subsequence of LHS                       | `"Fabrikam" !startswith_cs "fab"`                     | Yes            | No                     | –                                            |

## Results

Returns events for which the **Predicate** evaluates to `true`.

## Examples {#examples}

Here are a few examples of how to use the `where` operator in queries.

### Simple Comparisons First

This example retrieves events that are no older than 1 hour, come from a source called `MyCluster`, and have two fields
containing the same value:

```kusto
dataset=myDataset
| where Timestamp > ago(1h)
    and Source == "MyCluster"
    and ActivityId == SubActivityId
```

### Columns Contain String

Retrieve all the events in which the word "Cribl" appears on word boundaries in `field`:

```kusto
dataset=myDataset
| where field has "Cribl"
```

In `where` string comparisons, note that `:` is a substitute for the `has` operator. So in the above example,
`where field:"Cribl"` is equivalent to `where field has "Cribl"`.

### Match on AND Condition

Return results where the field `event` is greater than 20 and `parity` is odd.

```kusto {runSearch=true}
dataset=$vt_dummy event<100
| extend parity=iif(event%2==0, 'even', 'odd')
| where event>20 and parity=='odd'
```

### Match on OR Condition

Return results that match either of two conditions:

```kusto {runSearch=true}
dataset="cribl_internal_logs" | limit 50 | where method == "GET" or method == "POST"
```

### Match with Regex {#regex-examples}

The `where` operator supports regular expressions to match against strings or regex literals. You can see this in action
in our [Regex Examples](regex-matching#regex-examples).

### Search on Non-String Values

When using `where` to search on non-[`string`](string) values (such as [`int`](int) numbers), omit the quotes (`""`,
`''`). For example, instead of `dataset=$vt_dummy event<"10"`, do this:

```kusto {runSearch=true}
dataset=$vt_dummy event<10
```

An exception to this is searching for high-precision decimal numbers.
