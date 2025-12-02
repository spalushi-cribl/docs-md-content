# bag_pack


The `bag_pack` function creates a property bag from an alternating list of keys and values.

## Syntax

    `
bag_pack( Key1, Value1, Key2, Value2, ... )`

### Parameters

| Name    | Type                 | Required | Description      |
| ------- | -------------------- | -------- | ---------------- |
| `Key`   | [`string`](string)   | Yes      | The key.         |
| `Value` | [`dynamic`](dynamic) | Yes      | The key's value. |

You need to provide an even number of arguments (each `Key` needs a `Value`).

## Returns

Returns a single property bag made from the input keys and values.

The order of the key-value pairs returned is undetermined.

## Example

```kusto {runSearch=true}
print bag = bag_pack("Level", "Information", "ProcessID", 1234, "Data", bag_pack("url", "www.cribl.io"))
```

Output:

```kusto
{
  "bag": {
    "Level": "Information",
    "ProcessID": 1234,
    "Data": {
      "url": "www.cribl.io"
    }
  }
}
```
