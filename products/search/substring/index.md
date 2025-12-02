# substring


The `substring` function extracts a substring from a source string starting from some index to the end of the string.

Optionally, the length of the requested substring can be specified.

## Syntax

    `substring( Source, StartingIndex [, Length] )`

### Arguments

* **Source**: The source string that the substring will be taken from.
* **StartingIndex**: The zero-based starting character position of the requested substring. Can be a negative number, in which case the substring will be retrieved from the end of the source string.
* **Length**: An optional parameter that can be used to specify the requested number of characters in the substring.

## Results

A substring from the given string. The substring starts at the **StartingIndex** (zero-based) character position and continues to the end of the string (or to **Length** characters, if specified).

## Examples

This example returns `23456`:

```kusto {runSearch=true}
print substring("123456", 1)
```

This example returns `34`:
```kusto {runSearch=true}
print substring("123456", 2, 2)
``` 

This example returns `AB`:
```kusto {runSearch=true}
print substring("ABCD", 0, 2)
``` 

This example returns `56`:
```kusto {runSearch=true}
print substring("123456", -2, 2)
``` 
