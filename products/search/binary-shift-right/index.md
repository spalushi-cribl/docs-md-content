# binary_shift_right



The `binary_shift_right` function returns binary shift right operation on a pair of numbers.

## Syntax

    `binary_shift_right( Number1, Number2 )`

### Arguments

* **NumberX**: A Long number. The Long data type represents a signed, 64-bit wide integer.

## Example

```kusto
dataset=myDataset
| where bit != binary_shift_right(101,101)
```

