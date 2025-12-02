# strrep



The `strrep` function repeats given string specified number of times. If first or third argument is not of a string type, it will be forcibly converted to string.

## Syntax

    `strrep( Value, Multiplier, [ Delimiter ] )`

### Arguments

* **Value**: Input expression.
* **Multiplier**: Positive integer value (from 1 to 1024).
* **Delimiter**: An optional string expression (default: empty string).

## Results

Value repeated for a specified number of times, concatenated with **Delimiter**.

If **Multiplier** is more than maximum allowed value (1024), input string will be repeated 1024 times.

## Examples

This example returns `HelloHelloHello`:

```kusto {runSearch=true}
print str1="Hello" | extend newstr=strrep(str1,3)
```

This example returns `Hello-Hello-Hello`:

```kusto {runSearch=true}
print str1="Hello" | extend newstr=strrep(str1,3,"-")
```

This example returns `ABCABC`:

```kusto {runSearch=true}
print strrep('ABC', 2)
```

This example returns `123.123.123`:

```kusto {runSearch=true}
print strrep(123,3,'.')
```

This example returns `3 3`:

```kusto {runSearch=true}
print strrep(3s,2,' ')
```
