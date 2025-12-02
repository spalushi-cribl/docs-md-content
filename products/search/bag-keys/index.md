# bag_keys


The `bag_keys` function lists all root keys of a property bag.

## Syntax

    `
bag_keys( Bag )`

### Parameters

| Name  | Type                 | Required | Description                                        |
| ----- | -------------------- | -------- | -------------------------------------------------- |
| `Bag` | [`dynamic`](dynamic) | Yes      | The property bag whose root keys you want to list. |

## Returns

Returns an array of strings. The order of the keys is undetermined.

## Example

```kusto {runSearch=true}
print bag = dynamic({ "a": "b", "c": { "d": 123 }})
 | extend listOfKeys = bag_keys(bag)

// returns: ['a', 'c']
```
