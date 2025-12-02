# bag_has_key


The `bag_has_key` function checks whether a property bag contains a given key.

## Syntax

    `
bag_has_key( Bag, Key )`

### Parameters

| Name  | Type                 | Required | Description                  |
| ----- | -------------------- | -------- | ---------------------------- |
| `Bag` | [`dynamic`](dynamic) | Yes      | The property bag to check.   |
| `Key` | [`string`](string)   | Yes      | The key for which to search. |

## Returns

Returns `true` or `false`, depending on whether the key exists in the bag.

## Example

```kusto {runSearch=true}
print bag = dynamic({ "foo": 42, "bar": true })
 | extend fooExists = bag_has_key(bag, "foo"),
          blubbExists = bag_has_key(bag, "blubb")

// fooExists: `true`
// blubbExists: `false`
```
