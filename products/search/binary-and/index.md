# binary_and



The `binary_and` function returns a result of the bitwise `and` operation between two values.

## Syntax

    `binary_and( Number1, Number2 )`

### Arguments

* **NumberX**: A Long number. The Long data type represents a signed, 64-bit wide integer.

## Example

```kusto
dataset=myDataset
| where bit != binary_and(101,101)
```

