# replace_regex


The `replace_regex` function replaces all RE2 regular expression matches with another string.

## Syntax

    `replace_regex( Text, Regex, Rewrite )`

### Arguments

* **Text**: A string.
* **Regex**: The RE2 regular expression to search text. The expression can contain capture groups in parentheses.
* **Rewrite**: The replacement regex for any match made by **Regex**. Use `\0` to refer to the whole match, `\1` for the first capture group, `\2` and so on for subsequent capture groups.

## Results

**Source** after replacing all matches of **Regex** with evaluations of **Rewrite**. Matches do not overlap.

## Examples

This example replaces the input string's "is" with "was":

```kusto {runSearch=true}
print theText="Number is 2.000000"| extend theNewText=replace_regex(theText, @'is (\d+)', @'was: \1')
```

This example returns `Number was: 2.000000`:

```kusto
replace_regex("Number is 2.000000", @'is (\d+)', @'was: \1')
```

This example returns `Number was: 5.000000`:

```kusto
replace_regex("Number is 5.000000", @'is (\d+)', @'was: \1')
```