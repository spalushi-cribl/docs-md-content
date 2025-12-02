# search


The `search` operator finds events with specific text strings.

> We recommend using the [`cribl`](cribl) operator, which has easier syntax and more capabilities.
>
{.box .success}

## Syntax

`[Scope |] search [in (Dataset, ...)] [kind=CaseSensitivity] Predicate`

### Arguments

**Scope**: An optional expression that acts as a data source to be searched over. Cannot appear together with the optional phrase that includes **FieldName**.

**CaseSensitivity**: An optional flag that controls the behavior of all string scalar operators with respect to case sensitivity. Valid values are the two synonyms `default` and `case_insensitive` (which is the default for operators such as `has`, namely being case-insensitive) and `case_sensitive` (which forces all such operators into case-sensitive matching mode).

**FieldName**: An optional comma-separated list of "wildcarded" field names to take part in the search.

**Predicate**: An expression that defines what to search for and is evaluated for every record in the input. The expression must return a [`bool`](bool) value. If the expression returns `true`, the record is outputted. The syntax for **Predicate** extends and modifies the normal syntax for expressions:

* **String matching extensions**: String literals that appear as terms in the **Predicate** indicate a term match between all fields and the literal using [`has`](has), [`hasprefix`](hasprefix), [`hassuffix`](hassuffix), and the inverted `!` or case-sensitive `cs` versions of these operators. Asterisks are not supported within the literal, nor as wildcard characters.

* **Field restriction**: By default, string matching extensions attempt to match against all fields of the Dataset. It is possible to restrict this matching to a particular field by using the following syntax: `FieldName:StringLiteral`.

* **String equality**: Exact matches of a field against a string value (instead of a term-match) can be done using the syntax `FieldName==StringLiteral`.

* **Other expressions**: The syntax supports all expressions that return a [`bool`](bool) value. For example, `"error" and x==123` means: search for records that have the term `error` in any of their fields, and have the value `123` in the `x` field."

* **Regex match**: Indicate regular expression matching using the syntax `FieldName matches regex RegularExpression`. For syntax details, see [Regex Examples](regex-matching#regex-examples), [Regex Flags](regex-matching#flags), and [Disambiguate Regex Characters](regex-matching#regex-distinguish).

## String Matching

#|Syntax|Meaning (equivalent `where`)
-----|-----|-----
1|`search "err"`|`where * has "err"`
2|`search in (T1,T2) "err"`|<code>union T1,T2 &#124; where * has "err"</code>
3|`search col:"err"`|`where col has "err"`
4|`search col=="err"`|`where col=="err"`
10|`search col matches regex "..."`|`where col matches regex "..."`
11|search kind=case_sensitive| All string comparisons are case-sensitive
12|`search "abc" and ("def" or "hij")`|`where * has "abc" and (* has "def" or * has hij")`
13|`search "err" or (A>a and A<b)`|`where * has "err" or (A>a and A<b)`

## Examples

Simple term search over all Datasets and fields:

```kusto
search "goats"
```

Looking only for records that match both terms:

```kusto
search "goats" and ("Billy" or "Nanny")
```

Performing a case-sensitive match of all terms:

```kusto
search kind=case_sensitive "BillB" and ("SteveB" or "SatyaN")
```

Restricting the match to certain fields:

```kusto
search CEO:"goats" or CSA:"goats"
```

Specific time limit:

```kusto
search "goats" and Timestamp >= datetime(1981-01-01)
```

Return results that match "test event":

```kusto {runSearch=true}
dataset=$vt_dummy event<10 
| extend _raw=iif(event%2>0, "This is a test event", "This is another event") 
| search _raw has "test event"
```

In all the above examples, `search` behaves as a filtering operator. You can use `search` as a data-**producing** operator in simple expressions of the form `search in (<Dataset>)`:

```kusto {runSearch=true}
search in (cribl_search_sample) "mozilla"
```

However, in the next example, `search` again behaves as a filtering operator. It follows this behavior whenever a `search` expression is placed after a data-producing operator (such an explicit or implict [`cribl`](cribl), or even another `search` operator in the pattern shown in the preceding example). This returns results where strings in the `useragent` field match "Mozilla":

```kusto {runSearch=true}
datatype="apache_access_combined" 
| limit 10 
| search useragent in ("Mozilla")
```
