# min_of



The `min_of` function returns the minimum value of several evaluated numeric expressions.

## Syntax

    `min_of( Expression_1, Expression_2, ... )`

### Arguments

**Expression_i**: A scalar expression, to be evaluated.
* All arguments must be of the same type.
* Maximum of 64 arguments is supported.
* Non-null values take precedence to null values.

## Results

The minimum value of all argument expressions.

## Example

* `min_of(10, 1, -3, 17)` returns as `-3`

