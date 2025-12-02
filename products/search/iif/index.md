# iif


The `iif` function evaluates the first argument (the predicate), and returns the value of either the second or third
arguments, depending on whether the predicate evaluated to `true` (second) or `false` (third).

The second and third arguments must be of the same type.

Alias: [iff](iff) (`iif` and `iff` are synonyms.)

## Syntax

`iif( Predicate, IfTrue, IfFalse )`

### Arguments

- **Predicate**: An expression that evaluates to a `boolean` value.
- **IfTrue**: An expression that gets evaluated and its value returned from the function if **Predicate** evaluates to
  `true`.
- **IfFalse**: An expression that gets evaluated and its value returned from the function if **Predicate** evaluates to
  `false`.

## Results

This function returns the value of **IfTrue** if predicate evaluates to `true`, or the value of **IfFalse** otherwise.

## Example

- `| extend day = iif(floor(Timestamp, 1d)==floor(now(), 1d), "today", "anotherday")`

