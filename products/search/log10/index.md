# log10


The `log10` function returns the common (base-10) logarithm function.

## Syntax

    `log10( X )`

### Arguments

* **X**: A real number greater than 0.

## Results

* The common logarithm is the base-10 logarithm: the inverse of the exponential function (exp) with base 10.
* `null` if the argument is negative or null or can't be converted to a `real` value.

## Examples {#examples}

This returns `4`:

```kusto {runSearch=true}
print log10( 10000 )
```
