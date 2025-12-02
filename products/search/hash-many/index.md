# hash_many



The `hash_many` function returns a combined hash value of multiple values.

## Syntax

    `hash_many( Value1 , Value2 [, ValueN ...] )`

### Arguments

* **Value1**, **Value2**, **ValueN**: Input values that will be hashed together.

## Returns

The `hash` function is applied to each of the specified scalars. The resulting hashes are combined into a single hash and returned.

## Example

```kusto
dataset=myDataset
| extend combined = hash_many(value1, value2)
```

