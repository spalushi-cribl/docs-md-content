# binary_or



The `binary_or` function returns a result of the bitwise `or` operation of the two values.

## Syntax

    `binary_or( Number1, Number2 )`

### Arguments

* **NumberX**: A Long number. The Long data type represents a signed, 64-bit wide integer.

## Example

```kusto
dataset=myDataset
| where bit != binary_or(101,101)
```

