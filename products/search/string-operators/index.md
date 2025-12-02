# String Operators


A list of string operators supported by Cribl Search.

---

String operators manipulate and transform text, enabling actions like concatenation, trimming, replacement, or
extraction.

Note that these operators' support, and their case-sensitivity, vary depending on their parent
[search operators](search-operators) and [filter operators](filter-operators). For exact details, see these pages:

- [`cribl`](cribl#comparisonoperator) operator.
- [`where`](where#operators) operator.

We use the following abbreviations in the table below:

- Operators with a `_cs` suffix are case-sensitive.
- RHS = right-hand side of the expression.
- LHS = left-hand side of the expression.


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

