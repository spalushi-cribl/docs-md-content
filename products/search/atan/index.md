# atan


The `atan` function returns the angle whose tangent is the specified number (the inverse operation of [`tan`](tan)).

## Syntax

    `atan( X )`

### Arguments

* **X**: A real number.

## Returns

The value of the arc tangent of **X**.

## Examples {#examples}

This example returns `1.5485777614681775`:

```kusto {runSearch=true}
print num = 45
| extend newField = atan( num )
```
