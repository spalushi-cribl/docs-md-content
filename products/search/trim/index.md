# trim


The `trim` function removes all leading and trailing matches of the specified string or regular expression.

## Syntax

    `trim( {Regex | String}, Source )`

### Arguments

* **Regex**: Regular expression or string to trim from the beginning and/or the end of **Source**. Pass a regular expression in the format `@'<regex>'`; pass a string the format `"<string>"`. If you omit this argument, the function applies regex to match all whitespace.
* **Source**: A string.

## Results

**Source**, after trimming matches of **Regex** or **String** found in the beginning and/or the end of **Source**.

## Examples

Add some leading characters with [`strcat`](strcat), then remove them with `trim`:

```kusto {runSearch=true}
print str1="ABC" 
| extend str2=strcat("  >>",str1),
    thestr=trim("  >>",str2)
```

Remove leading and trailing `-` characters, **Regex** first argument:
```kusto {runSearch=true}
print trim(@"\-+", "---Hello, world!---")
```

Automatically remove leading and trailing whitespace, using implicit regex:

```kusto {runSearch=true}
dataset="$vt_dummy"
| extend string_to_trim = "   Hello, world!   "
| project
    string_to_trim,
    trimmed_string = trim(string_to_trim)
```


