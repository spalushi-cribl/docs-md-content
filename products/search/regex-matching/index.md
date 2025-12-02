# Regex Matching


Regular expressions usage, with examples, supported flags, and syntax details.

---

The [`matches regex`](matches-regex), [`search`](search), [`where`](where), [`in`](in-cs), [`!in`](not-in-cs), [`in~`](in), and [`!in~`](not-in) operators can use regex syntax to match against strings and regex literals. 

To match against regex literals, the supported comparison operators are [`==`](equals-cs), [`=~`](equals), [`!=`](not-equals-cs), and [`!~`](not-equals).

## Regex Examples {#regex-examples}

The examples below focus on using regular expressions with the `where` operator.

### Match on String Values {#regex-string}

Match a corresponding regex literal, with wildcard. This is case-sensitive by default:

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where State == /K.*S/
```

Case-insensitive version of the above query, using the `/i` flag:

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where State == /k.*s/i
```

Case-insensitive, using the `=~` operator instead of the `/i` flag:

```kusto
dataset=myDataset 
| summarize event_count=count() by State
| where State =~ /k.*s/
```

### Match on `in` Lists or Combined Targets {#regex-in-combo}

Mix and match string targets with regex literal targets:

```kusto {runSearch=true}
dataset="cribl_internal_logs" method=* 
| limit 1000
| where method in ("GET", /^po.*/i)
```

Same query, using the `in~` operator for implicit `/i`:

```kusto {runSearch=true}
dataset="cribl_internal_logs" method=* 
| limit 1000
| where method in~ ("GET", /^po.*/)
```

An "or" query with `|` notation:

```kusto {runSearch=true}
dataset="cribl_internal_logs" method=* 
| limit 1000
| where method == /post|get/i
```

A negation query:

```kusto {runSearch=true}
dataset="cribl_internal_logs" method=* 
| limit 1000
| where method != /post|get/i
```

Find the string value `bar` in the field `foo`:

```kusto
cribl foo = /bar/
```

## Regex Flags {#flags}

Evaluation is case-sensitive by default. You can ignore case by using the `/i` flag or `=~` operator, as shown in the examples below.

This operator supports any combination of the `/i`, `/m`, `/s`, and `/u` flags:

| Flag | Description |
| ---- | ----------- |
| `i`  | Case-insensitive match. | 
| `m`  | Multi-line mode: Allows `^` and `$` to match next to newline characters. |
| `s`  | Single-line ("dotail") mode: Allows `.` to match newline characters. |
| `u`  | Unicode (full) mode; Allows matching against 4-byte Unicode characters. |

## Disambiguate Regex Characters {#regex-distinguish}

In some expressions, you might need to distinguish regex literal delimiters from division symbols. Consider this expression:

```kusto
dataset=myDataset 
| <query>
| where value == 10/20/5
```

The `/20/` portion **could** be a regular expression. But given the numeric characters surrounding it, Cribl Search interprets as division. However, a similar expression with non-numeric characters is ambiguous:

```kusto
dataset=myDataset 
| <query>
| where value == x/y/z
```

Here, Cribl Search would try to interpret `/y/` as a regular expression. You can disambiguate the expression as a series of divisions by inserting blank spaces, conceptually like this:

```kusto
where value == x / y / z
```

But to insert the blank spaces within regex syntax, you'd use the `\s` character qualifier:

```kusto
dataset=myDataset 
| <query>
| where value == /x\s\/\sy\s\/\sz/
```

> In the above style of regular expression, `/` must be escaped as `\/`.
>
{.box .info}
