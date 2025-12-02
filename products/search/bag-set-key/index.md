# bag_set_key


The `bag_set_key` function adds a key-value pair to a property bag, or, if the specified key already exists, overwrites
the key's value.

## Syntax

    `
bag_set_key( Bag, Key, Value )`

### Parameters

| Name    | Type                 | Required | Description                 |
| ------- | -------------------- | -------- | --------------------------- |
| `Bag`   | [`dynamic`](dynamic) | Yes      | The property bag to modify. |
| `Key`   | [`string`](string)   | Yes      | The key to set.             |
| `Value` | [`dynamic`](dynamic) | Yes      | The value to set.           |

## Returns

Returns the input property bag, adding the specified key-value pair if the `Key` doesn't exist, or overwriting the value
if the `Key` is already there.

The order of the key-value pairs returned is undetermined.

If the `Bag` specified is not a valid property bag, `bag_set_key` returns [`null`](null).

## Examples

### Add a Key-Value Pair to a Property Bag

```kusto {runSearch=true}
print bag = dynamic({ "foo": 42, "bar": true })
 | project result = bag_set_key(bag, "new", 1)
```

Input (before `bag_set_key`):

```kusto
{
  "bag": {
    "foo": 42,
    "bar": true
  }
}
```

Output (after `bag_set_key`):

```kusto
{
  "result": {
    "foo": 42,
    "bar": true,
    "new": 1
  }
}
```

### Overwrite a Key-Value Pair in a Property Bag

```kusto {runSearch=true}
print bag = dynamic({ "foo": 42, "bar": true })
 | project result = bag_set_key(bag, "foo", 100)
```

Input (before `bag_set_key`):

```kusto
{
  "bag": {
    "foo": 42,
    "bar": true
  }
}
```

Output (after `bag_set_key`):

```kusto
{
  "result": {
    "foo": 100,
    "bar": true
  }
}
```
