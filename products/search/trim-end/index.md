# trim_end



The `trim_end` function removes a trailing match of the specified regular expression.

## Syntax

    `trim_end( Regex, Source )`

### Arguments

* **Regex**: String or regular expression to be trimmed from the end of **Source**.
* **Source**: A string.

## Results

**Source** after trimming matches of **Regex** found at the end of **Source**.

## Example

Remove trailing `-` characters:

```kusto {runSearch=true}
print trim_end(@"\-+", "---Hello, world!---")
```


