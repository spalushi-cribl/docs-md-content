# top



The `top` operator returns the first N events sorted by the specified fields.

> `top 5 by name` is equivalent to the expression `sort by name | take 5` both from semantic and performance perspectives.
>
{.box .info}

When sorting different data types, numeric values always appear first.

## Syntax

    `Scope | top NumberOfRows by Expression [ asc | desc ] [ nulls first | nulls last ]`

### Arguments

Name|Type|Required|Description
-----|-----|-----|-----
**Scope**|Tabular|Yes|The events to search.
**NumberOfRows**|Int|Yes|The number of rows to return.
**Expression**|String|Yes|Scalar expression by which to sort. The type of the values must be numeric, date, time or string.
`asc` or `desc`|String|No|Controls whether the selection is from the "bottom" or "top" of the range. Default desc. Numeric values always appear before other data types.
`nulls first` or `nulls last`|String|No|Controls whether null values appear at the "bottom" or "top" of the range. Default for `asc` is `nulls first`. Default for `desc` is `nulls last`.

## Examples

Show the top 3 events with the most errors.

```kusto
dataset=myDataset 
| top 3 by errorCount
```

Return the top 42 results sorted by the `event` field in descending order.

```kusto {runSearch=true}
dataset=$vt_dummy event<100 
| extend parity=iif(event%2==0, 'even', 'odd') 
| top 42 by event desc
```

