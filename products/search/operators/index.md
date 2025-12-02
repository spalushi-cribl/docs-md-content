# Operators in Cribl Search


A comprehensive list of all operators supported in Cribl Search, grouped by category.

---

An operator in Cribl Search is a query component that processes data, performing actions such as filtering, counting, or
transforming events. Operators can use [functions](functions), and are delimited by the pipe character `|`.

For example, the [`limit`](limit) operator sets the maximum number of events returned from a search:

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 100
```

## Aggregation Operators {#aggregation}

Aggregation operators summarize data by grouping it based on specified fields and applying aggregation functions like
[`sum`](sum), [`avg`](avg), or [`max`](max) to produce meaningful insights.


| Name                       | Description                                                                |
| -------------------------- | -------------------------------------------------------------------------- |
| [`count`](operators-count) | Returns the number of all input events.                                    |
| [`eventstats`](eventstats) | Aggregates events and adds the results as new fields to the source events. |
| [`summarize`](summarize)   | Produces a table that aggregates the input data.                           |
| [`timestats`](timestats)   | Aggregates events by time periods or bins..                                |


## Data Operators {#data}

Data operators transform, enrich, or manipulate events, enabling actions like renaming, joining, or exporting.


| Name                               | Description                                                                              |
| ---------------------------------- | ---------------------------------------------------------------------------------------- |
| [`centralize`](centralize)         | Forces subsequent operators to the coordinator.                                          |
| [`export`](export)                 | Exports search results to a [lookup](lookups) or a [Cribl Lake Dataset](/lake/datasets). |
| [`extend`](extend)                 | Appends fields created by expressions.                                                   |
| [`extract`](extract-operator)      | Extracts data from a field.                                                              |
| [`foldkeys`](foldkeys)             | Folds hierarchical field names into a nested structure.                                  |
| [`ip-lookup`](ip-lookup)           | Enriches events with IP address data.                                                    |
| [`join`](join)                     | Merges events from two different data scopes.                                            |
| [`lookup`](lookup)                 | Enriches events with lookup files.                                                       |
| [`mv-expand`](mv-expand)           | Expands an object into multiple events.                                                  |
| [`mv-pull`](mv-pull)               | Pulls key-value pairs from array objects into events or objects/bags.                          |
| [`project-rename`](project-rename) | Renames fields.                                                                          |
| [`union`](union)                   | Appends one set of results to another.                                                   |


## Display Operators {#display}

Display operators control how data is presented in the output, allowing formatting, sorting, or visualizing results for
better readability.


| Name                              | Description                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------------- |
| [`limit`](limit) ([`take`](take)) | Limits the number of events.                                                                |
| [`order`](order) ([`sort`](sort)) | Arranges events into order by one or more fields.                                           |
| [`print`](print)                  | Outputs expression results.                                                                 |
| [`project`](project)              | Keeps only the fields specified, and can also rename fields and insert new computed fields. |
| [`project-away`](project-away)    | Excludes specific fields from the results..                                                 |
| [`range`](range)                  | Generates a series of events.                                                               |
| [`render`](render)                | Enforces a specific visualization of the search results.                                    |
| [`top`](top)                      | Returns the first N events sorted by the specified fields.                                  |


## Filter Operators {#filter}

Filter operators exclude or include events based on specified conditions, allowing you to narrow down the Dataset to
relevant records.


| Name                   | Description                                      |
| ---------------------- | ------------------------------------------------ |
| [`dedup`](dedup)       | Filters out duplicate events.                    |
| [`distinct`](distinct) | Finds unique field values.                       |
| [`search`](search)     | Finds events that contain the specified text.    |
| [`where`](where)       | Filters events based on the specified predicate. |


## Logical Operators {#logical}

Logical operators perform comparisons and evaluations.


| Operator     | Description                                                                  |
| ------------ | ---------------------------------------------------------------------------- |
| `AND`        | Returns `true` if both operands are `true`, otherwise returns `false`.       |
| `NOT` or `!` | Returns `true` if the operand is `false`, otherwise returns `false`.         |
| `OR`         | Returns `true` if at least one operand is `true`, otherwise returns `false`. |


## Numerical Operators {#numerical}

Numerical operators perform arithmetic operations on numerical values, enabling calculations like addition or
subtraction.


| Operator | Description              |
| -------- | ------------------------ |
| `==`     | Equal                    |
| `!=`     | Not Equal                |
| `>`      | Greater Than             |
| `>=`     | Greater Than or Equal To |
| `<`      | Less Than                |
| `<=`     | Less Than or Equal To    |
| `+`      | Add                      |
| `-`      | Subtract                 |
| `*`      | Multiply                 |
| `/`      | Divide                   |
| `%`      | Modulo                   |


## Search Operators {#search}

Search operators retrieve events based on defined criteria, enabling efficient data discovery.


| Name             | Description                                                                                                        |
| ---------------- | ------------------------------------------------------------------------------------------------------------------ |
| [`cribl`](cribl) | Finds specific events. The fundamental Cribl Search operator, implicit in queries that do not specify an operator. |
| [`find`](find)   | Finds specific events.                                                                                             |


## String Operators {#string}

String operators manipulate and transform text, enabling actions like concatenation, trimming, replacement, or
extraction.


| Operator                              | Description                                                   | Case-Sensitive | Example (yields `true`)                               |
| ------------------------------------- | ------------------------------------------------------------- | -------------- | ----------------------------------------------------- |
| [`==`](equals-cs)                     | Equals                                                        | Yes            | `"aBc" == "aBc"`                                      |
| [`!=`](not-equals-cs)                 | Not equals                                                    | Yes            | `"abc" != "ABC"`                                      |
| [`=~`](equals)                        | Equals                                                        | No             | `"abc" =~ "ABC"`                                      |
| [`!~`](not-equals)                    | Not equals                                                    | No             | `"aBc" !~ "xyz"`                                      |
| [`contains`](contains)                | RHS occurs as a subsequence of LHS                            | No             | `"FabriKam" contains "BRik"`                          |
| [`!contains`](not-contains)           | RHS doesn't occur in LHS                                      | No             | `"Fabrikam" !contains "xyz"`                          |
| [`contains_cs`](contains-cs)          | RHS occurs as a subsequence of LHS                            | Yes            | `"FabriKam" contains_cs "Kam"`                        |
| [`!contains_cs`](not-contains-cs)     | RHS doesn't occur in LHS                                      | Yes            | `"Fabrikam" !contains_cs "Kam"`                       |
| [`endswith`](endswith)                | RHS is a closing subsequence of LHS                           | No             | `"Fabrikam" endswith "Kam"`                           |
| [`!endswith`](not-endswith)           | RHS isn't a closing subsequence of LHS                        | No             | `"Fabrikam" !endswith "brik"`                         |
| [`endswith_cs`](endswith-cs)          | RHS is a closing subsequence of LHS                           | Yes            | `"Fabrikam" endswith_cs "kam"`                        |
| [`!endswith_cs`](not-endswith-cs)     | RHS isn't a closing subsequence of LHS                        | Yes            | `"Fabrikam" !endswith_cs "brik"`                      |
| [`has`](has)                          | Right-hand-side (RHS) is a whole term in left-hand-side (LHS) | No             | `"North America" has "america"`                       |
| [`!has`](not-has)                     | RHS isn't a full term in LHS                                  | No             | `"North America" !has "amer"`                         |
| [`has_all`](has-all)                  | Same as [`has`](has) but works on all of the events           | No             | `"North and South America" has_all("south", "north")` |
| [`!has_all`](not-has-all)             | Not all of the RHS terms are present in LHS                   | No             | `"North and South America" !has_all("south", "east")` |
| [`has_any`](has-any)                  | Same as [`has`](has) but works on any of the events           | No             | `"North America" has_any("south", "north")`           |
| [`!has_any`](not-has-any)             | None of the RHS terms are present in LHS                      | No             | `"North and South America" !has_any("east", "west")`  |
| [`has_cs`](has-cs)                    | RHS is a whole term in LHS                                    | Yes            | `"North America" has_cs "America"`                    |
| [`!has_cs`](not-has-cs)               | RHS isn't a full term in LHS                                  | Yes            | `"North America" !has_cs "amer"`                      |
| [`hasprefix`](hasprefix)              | RHS is a term prefix in LHS                                   | No             | `"North America" hasprefix "ame"`                     |
| [`!hasprefix`](not-hasprefix)         | RHS isn't a term prefix in LHS                                | No             | `"North America" !hasprefix "mer"`                    |
| [`hasprefix_cs`](hasprefix-cs)        | RHS is a term prefix in LHS                                   | Yes            | `"North America" hasprefix_cs "Ame"`                  |
| [`!hasprefix_cs`](not-hasprefix-cs)   | RHS isn't a term prefix in LHS                                | Yes            | `"North America" !hasprefix_cs "CA"`                  |
| [`hassuffix`](hassuffix)              | RHS is a term suffix in LHS                                   | No             | `"North America" hassuffix "ica"`                     |
| [`!hassuffix`](not-hassuffix)         | RHS isn't a term suffix in LHS                                | No             | `"North America" !hassuffix "americ"`                 |
| [`hassuffix_cs`](hassuffix-cs)        | RHS is a term suffix in LHS                                   | Yes            | `"North America" hassuffix_cs "ica"`                  |
| [`!hassuffix_cs`](not-hassuffix-cs)   | RHS isn't a term suffix in LHS                                | Yes            | `"North America" !hassuffix_cs "icA"`                 |
| [`in`](in-cs)                         | Equal to any of the events                                    | Yes            | `"abc" in ("123", "345", "abc")`                      |
| [`!in`](not-in-cs)                    | Not equal to any of the events                                | Yes            | `"bca" !in ("123", "345", "abc")`                     |
| [`in~`](in)                           | Equal to any of the events                                    | No             | `"Abc" in~ ("123", "345", "abc")`                     |
| [`!in~`](not-in)                      | Not equal to any of the events                                | No             | `"bCa" !in~ ("123", "345", "ABC")`                    |
| [`matches regex`](matches-regex)      | LHS contains a match for RHS                                  | Yes            | `"Fabrikam" matches regex "b.*k"`                     |
| [`startswith`](startswith)            | RHS is an initial subsequence of LHS                          | No             | `"Fabrikam" startswith "fab"`                         |
| [`!startswith`](not-startswith)       | RHS isn't an initial subsequence of LHS                       | No             | `"Fabrikam" !startswith "kam"`                        |
| [`startswith_cs`](startswith-cs)      | RHS is an initial subsequence of LHS                          | Yes            | `"Fabrikam" startswith_cs "Fab"`                      |
| [`!startswith_cs`](not-startswith-cs) | RHS isn't an initial subsequence of LHS                       | Yes            | `"Fabrikam" !startswith_cs "fab"`                     |

