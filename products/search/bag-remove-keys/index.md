# bag_remove_keys


The `bag_remove_keys` function removes key-value pairs from a property bag.

## Syntax

    `
bag_remove_keys( Bag, Keys )`

### Parameters

| Name   | Type                 | Required | Description                                 |
| ------ | -------------------- | -------- | ------------------------------------------- |
| `Bag`  | [`dynamic`](dynamic) | Yes      | The property bag from which to remove keys. |
| `Keys` | [`dynamic`](dynamic) | Yes      | A key, or a list of keys, to remove.        |

## Returns

Returns the input property bag stripped of the specified keys and their values.

The order of the key-value pairs returned is undetermined.

## Example

```kusto {runSearch=true}
print bag = dynamic({ "foo": 42, "bar": true })
 | project result = bag_remove_keys(bag, "foo")
```

Input (before `bag_remove_keys`):

```kusto
{
  "bag": {
    "foo": 42,
    "bar": true
  }
}
```

Output (after `bag_remove_keys`):

```kusto
{
  "result": {
    "bar": true
  }
}
```
