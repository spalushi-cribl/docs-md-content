# translate


The `translate` function replaces a set of characters with another set of characters in a given string. The function searches for characters in the `SearchList` and replaces them with the corresponding characters in `ReplacementList`.

## Syntax

    `translate( SearchList, ReplacementList, Source )`

### Arguments

* **SearchList**: The list of characters that should be replaced.
* **ReplacementList**: The list of characters that should replace the characters in `SearchList`.
* **Source**: A string to search.

## Results

**Source** after replacing all occurrences of characters in `ReplacementList` with the corresponding characters in `SearchList`.

## Examples

This example returns `xxx`:
```kusto {runSearch=true}
print translate("abc", "x", "abc")
```

This example returns `kusto`:

```kusto {runSearch=true}
print translate("krasp", "otsku", "spark")
```

This example returns `hello world`:

```kusto {runSearch=true}
print translate("abcdefghijklmopqrstuvwxyz", "zyxwvutsrqpomlkjihgfedcba", "svool dliow")
```

This example also returns `hello world`, by calling [`reverse`](reverse):

```kusto {runSearch=true}
print translate("abcdefghijklmopqrstuvwxyz", reverse("abcdefghijklmopqrstuvwxyz"), "svool dliow")
```
