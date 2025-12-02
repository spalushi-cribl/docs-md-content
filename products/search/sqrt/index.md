# sqrt


The `sqrt` function returns the square root function.

## Syntax

    `sqrt( X )`

### Arguments

* **X**: A real number >= 0.

## Returns

* A positive number such that `sqrt(x) * sqrt(x) == x`.
* `null` if the argument is negative or cannot be converted to a `real` value.

## Examples {#examples}

This returns `4`:

```kusto {runSearch=true}
print sqrt ( 16 )
```

This returns `null`:

```kusto {runSearch=true}
print sqrt ( -64 )
```
