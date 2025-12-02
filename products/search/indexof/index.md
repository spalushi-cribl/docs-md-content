# indexof


The `indexof` function reports the zero-based index of the first occurrence of a specified string within the input string.

If the lookup or input string isn't of `string` type, the function forcibly casts the value to `string`.

## Syntax

    `indexof( Source, Lookup [, StartIndex [, Length [, Occurrence ]]] )`

### Arguments

* **Source**: Input string.
* **Lookup**: String to look up.
* **StartIndex**: Search start position. A negative value will offset the starting search position from the end of the **Source** by this many steps: `abs(StartIndex)`.
* **Length**: Number of character positions to examine. A value of -1 means unlimited length.
* **Occurrence**: The number of the occurrence. Default is `1`.

## Results

Zero-based index position of lookup.

* Returns -1 if the string isn't found in the input.
* For irrelevant inputs (occurrence < 0 or length < -1) - returns `null`.

## Examples

This example returns `3`:

```kusto {runSearch=true}
print indexof("Hello, world!", "lo")
```

This example returns `17`, because the `StartIndex` value of `5` skips over the first match:

```kusto {runSearch=true}
print indexof("Hello, world, hello!", "lo", 5)
```

This example returns `2`:

```kusto {runSearch=true}
print indexof("abcdefg", "cde")
```

This example returns `2`:

```kusto {runSearch=true}
print indexof("abcdefg", "cde", 1, 4)
``` 

This example returns `-1` for not found. The search starts from index `1`, but stops after 2 characters, so the full lookup string can't be found:

```kusto {runSearch=true}
print indexof("abcdefg", "cde", 1, 2)
``` 

This example returns `-1` for not found. The `3` value supplied for `StartIndex` is beyond where the lookup string begins:

```kusto {runSearch=true}
print indexof("abcdefg", "cde", 3, 4)
```

This example returns `5`. The negative `StartIndex` begins the search at the `b` character, allowing the lookup string to match:

```kusto {runSearch=true}
print indexof("abcdefg", "cde" ,-5)
```

This example returns `4`. The first two arguments are forcibly cast to strings `12345` and `5`:

```kusto {runSearch=true}
print indexof(1234567, 5, 1, 4)
```

This example returns `2`:

```kusto {runSearch=true}
print indexof("abcdefg", "cde", 2, -1)
``` 

This example returns `9`:

```kusto {runSearch=true}
print indexof("abcdefgabcdefg", "cde", 1, 11, 2)
``` 

This example returns `-1` for not found. The third occurrence of the lookup string is outside the specified range:

```kusto {runSearch=true}
print indexof("abcdefgabcdefg", "cde", 1, -1, 3)
```
