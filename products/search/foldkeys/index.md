# foldkeys


The `foldkeys` operator takes field names that contain a separator character, and folds them into a nested structure. This operator supports regex literals.

```kusto
// fold all field names by `/`, and delete the original entries
Scope
 | foldkeys separator="/"
```

## Purpose

Use `foldkeys` to normalize hierarchical [field names](build-a-search#field-names). If you have data where a hierarchy
of fields is encoded in the name (for example, `email.address` and `email.user`), `foldkeys` can transform the keys (not
the values) into a nested structure.

## Syntax

    `
Scope
 | foldkeys [delete_original=true|false]
            [separator="<Separator>"]
            ["<SelectionRegex>"]`

### Parameters

| Parameter         | Required | Description                                                                                                                            |
| ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `Scope`           | ✓        | Events to [search](build-a-search#syntax).                                                                                             |
| `delete_original` |          | `true` (default): output only the folded field names.<br>`false`: output both the folded and original field names.                     |
| `separator`       |          | Set a `Separator` character to use as the path separator, for example: `separator="/"`. The default is `.`.                            |
| `SelectionRegex`  |          | Optional regular expression. If defined, only field names that match this expression are folded (along with the `SeparatorCharacter`). |

  > To pass regex literals as **SelectRegex** parameters, see syntax details at [Regex Examples](regex-matching#regex-examples), [Regex Flags](regex-matching#flags), and [Disambiguate Regex Characters](regex-matching#regex-distinguish).
  >
  {.box .success}

## Returns

All the input events, but with hierarchical field names transformed into nested structures.

## Examples

### Fold All Field Names

Fold all field names, using the default `.` separator. Don't keep the original field names in the output.

```kusto {runSearch=true}
dataset="$vt_dummy" event < 1
 | extend ["email.address"]="foo", ["email.user"]="bar"
 | foldkeys
```

Input (before `foldkeys`):

```kusto
{
  "_time": 1723036428.379,
  "event": 0,
  "dataset": "$vt_dummy",
  "email.address": "foo",
  "email.user": "bar"
}
```

Output (after `foldkeys`):

```kusto
{
  "_time": 1723036570.006,
  "event": 0,
  "dataset": "$vt_dummy",
  "email": {
    "address": "foo",
    "user": "bar"
  }
}
```

### Fold Selected Field Names and Keep the Originals

Fold all field names that start with `eMail` (case-insensitive), using `/` as a separator. Keep the original field names
in the output.

```kusto {runSearch=true}
dataset="$vt_dummy" event < 3
 | extend ["email/name"]=strcat("foo", event +5), ["email/number"]=event * 3, ["rolf/sub/field"]=event + 10
 | foldkeys delete_original=false separator="/" '/^eMail/i'
```

Input (before `foldkeys`):

```kusto
// event 0
{
  "_time": 1723036048.423,
  "event": 0,
  "dataset": "$vt_dummy",
  "email/name": "foo5",
  "email/number": 0,
  "rolf/sub/field": 10
}

// event 1
{
  "_time": 1723036048.423,
  "event": 1,
  "dataset": "$vt_dummy",
  "email/name": "foo6",
  "email/number": 3,
  "rolf/sub/field": 11
}

// event 2
{
  "_time": 1723036048.423,
  "event": 2,
  "dataset": "$vt_dummy",
  "email/name": "foo7",
  "email/number": 6,
  "rolf/sub/field": 12
}
```

Output (after `foldkeys`):

```kusto
// event 0
{
  "_time": 1723036188.255,
  "event": 0,
  "dataset": "$vt_dummy",
  "email/name": "foo5",
  "email/number": 0,
  "rolf/sub/field": 10,
  "email": {
    "name": "foo5",
    "number": 0
  }
}

// event 1
{
  "_time": 1723036188.255,
  "event": 1,
  "dataset": "$vt_dummy",
  "email/name": "foo6",
  "email/number": 3,
  "rolf/sub/field": 11,
  "email": {
    "name": "foo6",
    "number": 3
  }
}

// event 2
{
  "_time": 1723036188.255,
  "event": 2,
  "dataset": "$vt_dummy",
  "email/name": "foo7",
  "email/number": 6,
  "rolf/sub/field": 12,
  "email": {
    "name": "foo7",
    "number": 6
  }
}
```

