# tan



The `tan` function returns the tangent of a numeric expression.

## Syntax

    `tan( X )`

### Arguments

* **X**: A real number.

## Examples {#examples}

This example returns `-1.995200412208242`:

```kusto {runSearch=true}
print num = 90
| extend newField = tan( num )
```
