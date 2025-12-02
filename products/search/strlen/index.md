# strlen


The `strlen` function returns the length, in characters, of the input string.

## Syntax

    `strlen( Source )`

### Arguments

* **Source**:The source string that will be measured for string length.

## Results

Returns the length, in characters, of the input string.

This function counts Unicode [code points](https://en.wikipedia.org/wiki/Code_point).

## Examples

This example returns `5`:

```kusto {runSearch=true}
print str1="hello" | extend fieldlen=strlen(str1)
```
