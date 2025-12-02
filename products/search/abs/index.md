# abs



The `abs` function calculates the absolute value of the input.

## Syntax

    `abs( X )`

### Arguments

* **X**: An integer or real number, or a timespan value.

## Returns

Absolute value of x.

## Examples {#examples}

```kusto {runSearch=true}
print num = -9999999
| extend newField = abs( num )
```
