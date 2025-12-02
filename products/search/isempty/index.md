# isempty


The `isempty` function returns `true` if the argument is an empty string, array, or object, or is null.

## Syntax

    `isempty( [ Value ] )`

## Results

A returned boolean value of `true` indicates that the argument is empty or null.

| x                 | isempty(x) |
| ----------------- | ---------- |
| `""`              | true       |
| `"x"`             | false      |
| `parse_json("")`   | true       |
| `parse_json("[]")` | true      |
| `parse_json("{}")` | true      |

{#example}
## Examples {#examples}

Report on missing events:

```kusto {runSearch=true}
dataset="cribl_internal_logs" 
| where isempty(stats.eventsFound)
```

Report on empty values in an arbitrary field:

```kusto
dataset=myDataset
| where isempty(fieldName)
```
