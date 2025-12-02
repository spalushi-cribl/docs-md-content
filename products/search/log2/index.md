# log2


The `log2` function returns the (base-2) logarithm function.

## Syntax

    `log2( X )`

### Arguments

* **X**: A real number greater than 0.

## Results

* The logarithm is the base-2 logarithm: the inverse of the exponential function (exp) with base 2.
* `null` if the argument is negative or null or can't be converted to a `real` value.

## Examples {#examples}

This returns `4`:

```kusto {runSearch=true}
print log2( 16 )
```
