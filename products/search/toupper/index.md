# toupper


The `toupper` function converts a string to upper case.

If the input string is [`null`](null), the function returns a blank string.

## Syntax

    `toupper( String )`

### Arguments

* **String**: A string.

## Example

This example returns uppercase `HELLO, WORLD`:

```kusto {runSearch=true}
print str1="Hello, World" | extend newstr=toupper(str1)
```
