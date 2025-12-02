# bag_pack_columns


The `bag_pack_columns` function creates a property bag from a list of fields (columns).

## Syntax

    `
bag_pack( FieldName[, ...] )`

### Parameters

| Name        | Type                 | Required | Description                                            |
| ----------- | -------------------- | -------- | ------------------------------------------------------ |
| `FieldName` | Any scalar data type | Yes      | The name of a field to add to the output property bag. |

## Returns

Returns a property bag made from the specified fields. The source field names serve as property names (keys), and the
field values as property values.

## Example

```kusto {runSearch=true}
dataset="$vt_dummy"
 | extend foo=1
 | extend bar=2
 | project Packed = bag_pack_columns(foo, bar)
```

Output:

```kusto
{
  "Packed": {
    "foo": 1,
    "bar": 2
  }
}
```
