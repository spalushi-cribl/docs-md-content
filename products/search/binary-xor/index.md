# binary_xor



The `binary_xor` function returns the bitwise `xor` operation of the two values.

## Syntax

    `binary_xor( Number1, Number2 )`

### Arguments

* **NumberX**: A Long number. The Long data type represents a signed, 64-bit wide integer.

## Example

```kusto
dataset=myDataset
| where bit != binary_xor(101,101)
```

