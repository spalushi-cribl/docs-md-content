# cos


The `cos` function returns the cosine of the specified numeric expression.

## Syntax

    `cos( X )`

### Arguments

* **X**: A real number.

## Examples {#examples}

This example returns `0.5403023058681398`:

```kusto {runSearch=true}
print num = 1
| extend newField = cos( num )
```
