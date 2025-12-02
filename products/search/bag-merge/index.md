# bag_merge


The `bag_merge` function merges two or more property bags, discarding duplicate keys.

## Syntax

    `
bag_merge( Bag1, Bag2[, ...] )`

### Parameters

| Name  | Type                 | Required | Description                 |
| ----- | -------------------- | -------- | --------------------------- |
| `Bag` | [`dynamic`](dynamic) | Yes      | The property bags to merge. |

## Returns

Returns a single property bag containing the merged results of all input bags.

If the same key is present in multiple input bags, only the value associated with the key from the leftmost argument is
kept.

The order of the key-value pairs returned is undetermined.

## Example

```kusto {runSearch=true}
print foo = dynamic({'A1':12, 'B1':2, 'C1':3}), bar = dynamic({'A2':81, 'B2':82, 'A1':1})
 | project result = bag_merge(foo, bar)
```

Input (before `bag_merge`):

```kusto
{
  "foo": {
    "A1": 12,
    "B1": 2,
    "C1": 3
  },
  "bar": {
    "A2": 81,
    "B2": 82,
    "A1": 1
  }
}
```

Output (after `bag_merge`):

```kusto
{
  "result": {
    "A2": 81,
    "B2": 82,
    "A1": 12,
    "B1": 2,
    "C1": 3
  }
}
```
