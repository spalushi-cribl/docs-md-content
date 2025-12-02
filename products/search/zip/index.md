# zip


The `zip` function merges two or more dynamic arrays, grouping elements of the same index.

## Syntax

    `
zip( Arrays )`

### Parameters

| Name     | Type                 | Required | Description                      |
| -------- | -------------------- | -------- | -------------------------------- |
| `Arrays` | [`dynamic`](dynamic) | Yes      | The dynamic array values to zip. |

## Returns

Returns an array whose elements are each an array holding the elements of the input arrays of the same index.

## Example

```kusto {runSearch=true}
print foo = zip(dynamic([1,3,5]), dynamic([2,4,6]))
```

Output:

```kusto
{
  "foo": [
    [
      1,
      2
    ],
    [
      3,
      4
    ],
    [
      5,
      6
    ]
  ]
}
```
