# isnan


The `isnan` function returns whether input is Not-a-Number (NaN) value.

## Syntax

    `isnan( X )`

### Arguments

* **X**: A real number.

## Results

A non-zero value (true) if **X** is NaN, and zero (false) otherwise.

## Examples {#examples}

This returns `false`:

```kusto {runSearch=true}
print isnan( 42 )
```

This returns `true`:

```kusto {runSearch=true}
print x = toint( 'pizza' )
| extend y = isnan( x )
```
