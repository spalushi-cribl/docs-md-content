# isnotnull


The `isnotnull` function returns `true` if the argument is not null.

Alias: `notnull`

## Syntax

    `isnotnull( [ Value ] )`

## Example

This example returns `true`:

```kusto {runSearch=true}
print isnotnull('Hello World!')
```

This example would return all events in which a field named `PossiblyNull` is not null:

```kusto
dataset=myDataset
| where isnotnull(PossiblyNull)
```
