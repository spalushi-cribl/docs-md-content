# binary_shift_left



The `binary_shift_left` function returns binary shift left operation on a pair of numbers.

## Syntax

    `binary_shift_left( Number1, Number2 )`

### Arguments

* **NumberX**: An integer.

## Example

```kusto
dataset=myDataset
| where bit != binary_shift_left(101,101)
```

