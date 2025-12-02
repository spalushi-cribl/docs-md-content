# degrees



The `degrees` function converts angle value in radians into value in degrees, using formula `degrees = (180 / PI ) * angle_in_radians`.

## Syntax

    `degrees( X )`

### Arguments

* **X**: Angle in radians (a real number).

## Results

The corresponding angle in degrees for an angle specified in radians.

## Examples {#examples}

This example returns approximately `360` degrees:

```kusto {runSearch=true}
print num = 6.283
| extend newField = degrees( num )
```
