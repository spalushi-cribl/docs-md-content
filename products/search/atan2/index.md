# atan2


The `atan2` function calculates the angle, in radians, between the positive x-axis and the ray from the origin to the point (y, x).

## Syntax

    `atan2( Y, X )`

### Arguments

* **X**: X coordinate (a real number).
* **Y**: Y coordinate (a real number).

## Returns

Returns the angle in radians between the positive x-axis and the ray from the origin to the point (y, x).

## Examples {#examples}

This example returns `1.571`:

```kusto {runSearch=true}
print atan2( 1,0 )
```
