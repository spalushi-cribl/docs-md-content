# lookup


The `lookup` operator retrieves fields from a [lookup](lookups) through a first-match lookup process. It returns all
rows of the table by default, incorporating any additional fields from the lookups wherever there is a match.


> To enrich events with IP address data, use the [`ip-lookup`](ip-lookup) operator.
{.box .info}

## Syntax

```kusto
Scope | lookup [ output=OutputFields[, ...] [ defaultVal=Default ]]
               [ ignoreCase=Case ]
               [ matchMode=Mode [ matchType=Type ]]
               LookupTable on Conditions[, ...]
```

### Arguments

The arguments are case-insensitive.

- **Scope**: The events to search.
- **OutputFields**: Field(s) to add from the lookup after matching. Defaults to **all** if not specified.
- **Default**: Assigns a default value to **OutputFields** when no value is present. Ignored if `output` is not defined.
- **Case**: Matching ignores case by default; set as `true`. Accepts boolean values – `yes`, `true`, `t`, `1` or `no`,
  `false`, `f`, `0`.
- **Mode**: Set to the matching logic of the lookup field(s). Supports `exact`, `cidr`, and `regex`. Defaults to
  `exact`. For example, if the lookup field contains a regular expression you'd use `regex`.
- **Type**: How to resolve multiple matches for `cidr` and `regex` modes. Defaults to `first`.
  - `first` returns the first matching entry.
  - `specific` scans all entries for the most specific match.
  - `all` returns all matches in the output, as arrays.
- **LookupTable**: The [lookup](lookups) filename, with or without the extension (you can enter `foo.csv` as `foo`). If
  the filename contains spaces, surround it with quotes (enter `foo bar.csv` as `"foo bar"`, `'foo bar'`, or
  `['foo bar']`).
- **Conditions**: Field(s) to look for in the lookup. There are three supported syntax variations:
  - Looking in the lookup for the **exact** field name as it appears in events. Syntax: `CommonFieldName`.
  - Single mapping between the event field and the lookup field name. Syntax: `EventFieldName = lookupFieldName`.
  - A join condition that specifies how to combine the data from the two tables.
    Syntax: `$left.eventFieldName == $right.lookupFieldName` where the event is
    referenced as `$left` and the lookup as `$right`. You can have `$left` and `$right` on either side of the expression.

## Examples

Look for a common field in the events and the `service_names_port_numbers.csv` lookup file.

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 100
 | lookup service_names_port_numbers on commonField
```

Map an event field (`dstport`) to a lookup field (`port_number`).

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 100
 | lookup service_names_port_numbers on dstport=port_number
```

Look for a common field, a mapped field, and use a join condition.

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 100
 | lookup service_names_port_numbers on commonField, eventField1=lookupField1, $left.eventField2 == $right.lookupField2
```

Look up an IP address range, using CIDR notation.

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 100
 | lookup matchMode='cidr' matchType='all' service_names_port_numbers on $left.hostip==$right.cidr
```

Match using a regular expression.

```kusto {runSearch=true}
dataset="cribl_search_sample"
 | limit 100
 | lookup matchMode='regex' matchType='all' service_names_port_numbers on eventfield=lookupfieldregex, file=file, user=user
```

Create a lookup to use in the next example. This search won't display any results, but it creates a lookup file
named `mymethods.csv`.

```kusto {runSearch=true}
dataset=$vt_dummy event<600
 | extend _time=_time-rand(600), method=iif(event%2>0, "GET", "POST")
 | summarize cnt=count() by method
 | export description="Table with http methods count" to lookup mymethods
```

Enrich events with a field `cnt` that comes from the `mymethods.csv` lookup file created in the example above.

```kusto {runSearch=true}
dataset=$vt_dummy event<100
 | extend method=split("GET,POST", ",")
 | mv-expand method
 | lookup mymethods on method
```

Use a lookup that's part of a [Pack](packs#use-lookup):

```kusto
dataset=myDataset
 | lookup pack(myPack).LookupTable on commonField
```
