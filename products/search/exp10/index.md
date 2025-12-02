# exp10


The `exp10` function calculates the base-10 exponential function of x, which is 10 raised to the power x: `10^x`.

## Syntax

    `exp10( X )`

### Arguments

* **X**: A real number, value of the exponent.

## Results

Exponential value of **X**.

## Examples {#examples}

This example returns `10,000`:

```kusto {runSearch=true}
print exp10( 4 )
```
