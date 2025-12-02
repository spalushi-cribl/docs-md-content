# countof



The `countof` function counts occurrences of a substring in a string. Plain string matches may overlap; regex matches don't.

## Syntax

    `countof( Source, Search [, Kind] )`

### Arguments

* **Source**: A string.
* **Search**: The plain string or regular expression to match inside source.
* **Kind**: `normal` or `regex`, defaults to `normal`.

## Results

The number of times that the search string can be matched in the container. Plain string matches may overlap; regex matches don't.

## Examples

This example returns `3`:

```kusto {runSearch=true}
print countof("Hello, world!", "l")
```

This example returns `1`:

```kusto {runSearch=true}
print countof("Hello, world!", "ll")
```

The next few examples show the different treatment of overlapping substrings in `normal` versus `regex` searches. 

This example returns `3`:

```kusto {runSearch=true}
print countof("aaa", "a")
```

This example returns `3` (not `2`!) because it matches all overlapping occurrences:

```kusto {runSearch=true}
print countof("aaaa", "aa")
```

 This example returns `2`:

```kusto {runSearch=true}
print countof("ababa", "ab", "normal")
```

This example returns `2`:

```kusto {runSearch=true}
print countof("ababa", "aba")
```

This example returns `1`, because the `regex` argument prevents matches on overlapping occurrences:

```kusto {runSearch=true}
print countof("ababa", "aba", "regex")
```

This example returns `2`: 

```kusto {runSearch=true}
print countof("abcabc", "a.c", "regex")
```

This example returns `6`:

```kusto {runSearch=true}
print countof("abcdefedcba", "[abc]", "regex")
```

This example returns `2`: 

```kusto {runSearch=true}
print countof("abc1234abcd1234", "[0-9]+", "regex")
```
