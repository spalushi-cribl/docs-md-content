# has_any_index


The `has_any_index` function searches an input string for items specified in an input array, and returns the array position (zero-based) of the first item found in the string.

## Syntax

    `has_any_index( String, LookupArray )`

### Arguments

* **String**: A string.
* **LookupArray**: Array of scalar or literal expressions to look up. The values should be of type long, integer, double, decimal, string, or guid.

## Results

Zero-based index position of the first item in **LookupArray** that is found in **String**. 


## Examples

This example matches the first lookup item:

```kusto {runSearch=true}
print has_any_index("this is an example", dynamic(['this', 'example']))
```

This example matches the last lookup item:

```kusto {runSearch=true}
print has_any_index("this is an example", dynamic(['not', 'example']))
```

This example matches none of the lookup items:

```kusto {runSearch=true}
print has_any_index("this is an example", dynamic(['not', 'found']))
```

This example matches within the range of integers:

```kusto {runSearch=true}
print has_any_index("Example number 2", range(1, 3, 1))
```

This example doesn't match against the empty lookup array:

```kusto {runSearch=true}
print has_any_index("this is an example", dynamic([]))
```
