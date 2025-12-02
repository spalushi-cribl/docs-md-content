# isinf


The `isinf` function returns whether input is an infinite (positive or negative) value.

## Syntax

    `isinf( X )`

### Arguments

* **X**: A real number.

## Results

A non-zero value (true) if **X** is a positive or negative infinite, and zero (false) otherwise.

## Examples {#examples}

This returns `false`:

```kusto {runSearch=true}
print isinf( 420 )
```

This returns `true`:

```kusto {runSearch=true}
print x = 42/0
| extend y = isinf( x )
```

This returns `true`:

```kusto {runSearch=true}
print isinf( world )
```
