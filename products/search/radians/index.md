# radians


The `radians` function converts angle value in degrees into value in radians, using formula `radians = (PI / 180 ) * angle_in_degrees`.

## Syntax

    `radians( X )`

## Arguments

* **X**: Angle in degrees (a real number).

## Results

The corresponding angle in radians for an angle specified in degrees.

## Examples {#examples}

This example returns an approximation of π:

```kusto {runSearch=true}
print num = 180
| extend newField = radians( num )
```
