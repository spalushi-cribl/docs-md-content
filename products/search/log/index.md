# log


The `log` function returns the natural logarithm function.

## Syntax

    `log( X )`

### Arguments

* **X**: A real number greater than 0.

## Results

* The natural logarithm is the base-e logarithm: the inverse of the natural exponential function (exp).
* `null` if the argument is negative or null or can't be converted to a `real` value.

## Examples {#examples}

This returns `4`:

```kusto {runSearch=true}
print log( 54.598 )
```
