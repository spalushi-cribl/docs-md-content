# hash_combine



The `hash_combine` function combines hash values of two or more hashes.

## Syntax

    `hash_combine( Hash1 , Hash2 [, Hash3 ...] )`

### Arguments

* **Hash1**: Long value representing the first hash value.
* **Hash2**: Long value representing the second hash value.
* **Hash3**: Long value representing Nth hash value.

## Returns

The combined hash value of the given scalars.

## Example

```kusto
dataset=myDataset
| extend h1 = hash(value1), h2=hash(value2)
| extend combined = hash_combine(h1, h2)
```

