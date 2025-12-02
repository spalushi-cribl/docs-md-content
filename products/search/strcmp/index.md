# strcmp



The `strcmp` function compares two strings.

The function starts comparing the first character of each string. If they are equal to each other, it continues with the following pairs until the characters differ or until the end of the shorter string is reached.

## Syntax

    `strcmp( String1, String2 )`

### Arguments

* **String1**: First input string for comparison.
* **String2**: Second input string for comparison.

## Results

Returns an integral value indicating the relationship between the strings:

* `<0` : the first character that does not match has a lower value in **String1** than in **String2**
* `0`  : the contents of both strings are equal
* `>0` : the first character that does not match has a greater value in **String1** than in **String2**

## Examples

This example returns `-1` for lower value:

```kusto {runSearch=true}
print str1="ABC", str2="ABCd" | extend match=strcmp(str1,str2)
```

This example returns `0`  for equality:

```kusto {runSearch=true}
print strcmp(ABC,ABC)
```

This example returns `1` for greater value:

```kusto {runSearch=true}
print strcmp(abc,ABC)
```

This example returns `1` for greater value:

```kusto {runSearch=true}
print strcmp(abcde,abc)
```
