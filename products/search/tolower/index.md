# tolower



The `tolower` function converts a string to lower case.

If the input string is [`null`](null), the function returns a blank string.

## Syntax

    `tolower( String )`

### Arguments

* **String**: A string.

## Example

This example returns lowercase `hello, world`:

```kusto {runSearch=true}
print str1="Hello, World" | extend newstr=tolower(str1)
```
