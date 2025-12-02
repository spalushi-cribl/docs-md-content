# format_bytes


The `format_bytes` function converts the input into a string that represents data size.

```kusto
// returns a string: "512 MB"
format_bytes(536870912)
```

## Purpose

Use `format_bytes` to display large, raw byte values in a user-friendly way.

## Syntax

    `
format_bytes( ByteCount [, Precision [, TargetUnit ] ] )`

### Parameters

| Name         | Type               | Required | Description                                                                                                                                      |
| ------------ | ------------------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `ByteCount`  | [`int`](int)       | Yes      | The value to convert into a data-size string.                                                                                                    |
| `Precision`  | [`int`](int)       | No       | The number of digits that the input value is rounded to after the decimal point. Default: `0`.                                                   |
| `TargetUnit` | [`string`](string) | No       | The unit of the output data size: `Bytes`, `KB`, `MB`, `GB`, `TB`, `PB`, or `EB`. If empty, Cribl Search selects the unit, based on input value. |

## Returns

Returns a [`string`](string) that represents data size, using the specified (or automatically selected) unit.

If the conversion is unsuccessful, the function returns an empty string.

## Examples

```kusto {runSearch=true}
// returns "564 Bytes"
print format_bytes(564)
```

```kusto {runSearch=true}
// returns "10.1 KB"
print format_bytes(10332, 1)
```

```kusto {runSearch=true}
// returns "19 MB"
print format_bytes(20010332)
```

```kusto {runSearch=true}
// returns "19.08 MB"
print format_bytes(20010332, 2)
```

```kusto {runSearch=true}
// returns "19541 KB"
print format_bytes(20010332, 0, "KB")
```
