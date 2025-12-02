# max_of



The `max_of` function returns the maximum value of several evaluated numeric expressions.

## Syntax

    `max_of( Expression_1, Expression_2, ... )`

### Arguments

**Expression_i**: A scalar expression, to be evaluated.
* All arguments must be of the same type.
* Maximum of 64 arguments is supported.
* Non-null values take precedence to null values.

## Results

The maximum value of all argument expressions.

## Example

* `max_of(10, 1, -3, 17)` returns as `17`

