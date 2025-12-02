# asin


The `asin` function calculates the angle whose sine is the specified number (the inverse operation of [`sin`](sin)).

## Syntax

    `asin( X )`

### Arguments

* **X**: A real number in range [-1, 1].

## Returns

Returns the value of the arc sine of **X**. Returns `null` if **X** < -1 or **X** > 1.

## Examples {#examples}

This example returns `1.5707963267948966`;

```kusto {runSearch=true}
print num = 1
| extend newField = asin( num )
```
