# trim_start


The `trim_start` function removes a leading match of the specified regular expression.

## Syntax

    `trim_start( Regex, Source )`

### Arguments

* **Regex**: String or regular expression to be trimmed from the beginning of **Source**.
* **Source**: A string.

## Results

**Source** after trimming matches of **Regex** found in the beginning of **Source**.

## Example

Remove leading `-` characters:

```kusto {runSearch=true}
print trim_start(@"\-+", "---Hello, world!---")
```

