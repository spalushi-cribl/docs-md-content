# parse_version


The `parse_version` function converts the input string representation of a version number to a comparable decimal number.

## Syntax

`parse_version( Expression )`

### Arguments

- **Expression**: A scalar expression of type `string` that specifies the version to be parsed.

  - Input string must contain from one to four version parts, represented as numbers and separated with dots `.`.
  - Each part of version may contain up to eight digits, with the max value at 99999999.
  - If the number of parts is less than four, all the missing parts are considered as trailing (`1.0` == `1.0.0.0`).

## Results

If the conversion is successful, returns a decimal.

If the conversion fails, returns `null`.

## Example

This example converts the two input strings to decimal values, and then compares the values:

```kusto {runSearch=true}
print theData="9.2", theData2="9.11.3" | extend v1=parse_version(theData), v2=parse_version(theData2) | extend is_larger=iff(v1>v2, true, false)
```


