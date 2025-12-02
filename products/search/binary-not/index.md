# binary_not



The `binary_not` function returns a bitwise negation of the input value.

## Syntax

    `binary_not( Number )`

### Arguments

* **Number**: A number.

## Example

```kusto
dataset=myDataset
| where bit != binary_not(101)
```

