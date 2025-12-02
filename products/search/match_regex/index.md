# match_regex


The `match_regex` function searches a text string for a specific pattern defined by a regular expression. It returns a [`bool`](bool) value indicating whether the pattern was found in the text. This function is commonly used for pattern matching and validation tasks in text processing and data extraction.

## Syntax

    `match_regex(Field, Regex)`

### Arguments

* **Field**: The field or String to match against.
* **Regex**: An RE2 regular expression as a String wrapped in forward slashes, `"/regex/"`.

## Results

Returns a [`bool`](bool) value, where matches are `true` and non-matches are `false`.

## Examples

This example checks for source addresses that include the substring `42`:

```kusto {runSearch=true}
dataset="cribl_search_sample" 
| where match_regex(srcaddr, "/42/")
```

This example finds the Goats on the farm:

```kusto
match_regex("Goats on a farm.", "/Goats/")
```

This example looks for the substring `is 2`:

```kusto
match_regex("Number is 2.000000", @'/is (\d+)/')
```
