# loggamma


The `loggamma` function computes log of absolute value of the [loggamma function](https://en.wikipedia.org/wiki/loggamma_function).

## Syntax

    `loggamma( X )`

### Arguments

* **X**: Parameter for the gamma function.

## Results

Returns the natural logarithm of the absolute value of the gamma function of **X**.

## Examples {#examples}

This returns `4.787`:

```kusto {runSearch=true}
print loggamma( 6 )
```
