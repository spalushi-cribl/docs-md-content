# coalesce


The `coalesce` function evaluates a list of expressions and returns the first non-null expression.

## Syntax

    `coalesce( Expression_1, Expression_2, ... )`

### Arguments

**Expression_i**: A scalar expression, to be evaluated.
* All arguments must be of the same type.
* Maximum of 64 arguments is supported.

## Results

The value of the first **Expression_1** whose value isn't null (or not-empty for string expressions).

## Example

* `coalesce(tolong("not a number"), tolong("42"), 33)` returns as `42`
