# isfinite


The `isfinite` function returns whether input is a finite value (is neither infinite nor NaN).

## Syntax

    `isfinite( X )`

### Arguments

* **X**: A real number.

## Results

A non-zero value (true) if **X** is finite, and zero (false) otherwise.

## Examples {#examples}

This returns `true`:

```kusto {runSearch=true}
print isfinite( 420 )
```

This returns `false`:

```kusto {runSearch=true}
print isfinite( hello )
```
