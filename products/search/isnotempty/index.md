# isnotempty


The `isnotempty` function returns `true` if the argument isn't an empty string, array, or object, and isn't null.

Alias: `notempty`

## Syntax

    `isnotempty( [ Value ] )`

## Results

A returned boolean value of `true` indicates that the argument is not empty or null.

| x                 | isnotempty(x) |
| ----------------- | ---------- |
| `""`              | false       |
| `"x"`             | true      |
| `parse_json("")`   | false       |
| `parse_json("[]")` | false      |
| `parse_json("{}")` | false      |

{#example}
## Examples {#examples}

This example reports on discovered events:

```kusto {runSearch=true}
dataset="cribl_internal_logs" 
| where isnotempty(stats.eventsFound)
```

This example returns `false`, because the argument is an empty string:

```kusto
isnotempty("")
```
