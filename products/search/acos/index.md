# acos


The `acos` function calculates the angle whose cosine is the specified number (the inverse operation of [`cos`](cos)).

## Syntax

    `acos( X )`

### Arguments

* **X**: A real number in range [-1, 1].

## Results

The value of the arc cosine of **X**. Returns `null` if **X** < -1 or **X** > 1

## Examples {#examples}

This example returns `3.141592653589793`:

```kusto {runSearch=true}
print num = -1
| extend newField = acos( num )
```
