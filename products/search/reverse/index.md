# reverse


The `reverse` function reverses the order of the input `string`. If the input value isn't of type string, then the function forcibly casts the value to type `string`.

## Syntax

    `reverse( Source )`

### Arguments

* **Source**: Input value.

## Results

The reverse order of a string value.

## Examples

This example returns `olleH`:

```kusto {runSearch=true}
print str1="Hello" | extend newstr=reverse(str1)
```

This example returns `ZYXWVUTSRQPONMLKJIHGFEDCBA`:

```kusto {runSearch=true}
print reverse("ABCDEFGHIJKLMNOPQRSTUVWXYZ")
```
