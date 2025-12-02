# sign


The `sign` function returns the sign of a numeric expression.

## Syntax

    `sign( X )`

## Arguments

* **X**: A real number.

## Results

The positive (+1) zero (0) or negative (-1) sign of the specified expression.

## Examples {#examples}

This returns `1` for positive:

```kusto {runSearch=true}
print sign ( 10000 )
```

This returns `-1` for negative:

```kusto {runSearch=true}
print sign ( -9999 )
```
