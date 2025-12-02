# cot


The `cot` function calculates the trigonometric cotangent of the specified angle, in radians.

## Syntax

    `cot( X )`

### Arguments

* **X**: A real number.

## Results

The cotangent function value for **X**.

## Examples {#examples}

This example returns `0.6173696237835551`:

```kusto {runSearch=true}
print num = 45
| extend newField = cot( num )
```
