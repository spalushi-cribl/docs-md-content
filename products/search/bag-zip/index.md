# bag_zip


The `bag_zip` function creates a property bag from two input dynamic arrays. The first array provides keys, the second
array provides values.

## Syntax

    `
bag_zip( KeysArray, ValuesArray )`

### Parameters

| Name          | Type                 | Required | Description                             |
| ------------- | -------------------- | -------- | --------------------------------------- |
| `KeysArray`   | Array of strings     | Yes      | Keys to set in the output property bag. |
| `ValuesArray` | [`dynamic`](dynamic) | Yes      | Values in the output property bag.      |

## Returns

Returns a property bag where keys come from the first array, and values from the second array. The keys and values are
matched in the order in which they are listed in the input (`key1: value1`, `key2: value2`).

- If there are more keys than values, missing values are filled with [`null`](null).
- If there are more values than keys, values with no matching keys are ignored.
- Keys that aren't strings are ignored.

The order of the key-value pairs returned is undetermined.

## Example

```kusto {runSearch=true}
print foo = dynamic(['key1', 'key2']), bar = dynamic(['value1', 'value2'])
 | project bag_zip(foo, bar)
```

Input (before `bag_zip`):

```kusto
{
  "foo": [
    "key1",
    "key2"
  ],
  "bar": [
    "value1",
    "value2"
  ]
}
```

Output (after `bag_zip`):

```kusto
{
  "bag_zip_foo_bar": {
    "key1": "value1",
    "key2": "value2"
  }
}
```
