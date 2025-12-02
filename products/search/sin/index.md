# sin


The `sin` function returns the sine of a numeric expression.

## Syntax

    `sin( X )`

## Arguments

* **X**: A real number.

## Examples {#examples}

This example returns `0.9092974268256817`:

```kusto {runSearch=true}
print num = 2
| extend newField = sin( num )
```
