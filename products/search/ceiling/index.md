# ceiling


The `ceiling` function rounds up an input numeric expression's value to the nearest matching integer.

Alias: [`ceil`](ceil) (`ceil` and `ceiling` are synonyms).

## Syntax

    `ceiling(number)`

## Arguments

|Name|Type|Required|Description|
|--|--|--|--|
| **number** | int, long, or real | Yes | The value to round up. | 

## Results

The smallest integer equal to, or greater than, the value of the specified numeric expression.

## Examples {#examples}

This example returns `100`:

```kusto {runSearch=true}
print num = 99.01
| extend newField = ceiling( num )
```
