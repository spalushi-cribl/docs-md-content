# strcat


The `strcat` function concatenates between 1 and 64 arguments to a single string.

## Syntax

`strcat( Argument1, Argument2 [, ArgumentN ] )`

### Arguments

- **Argument1 ... ArgumentN**: Expressions to be concatenated.

If an argument is not of the [`string`](string) type, it's forcibly converted to [`string`](string).

If an argument is [`null`](null), it's converted to a blank string.

## Results

Arguments concatenated to a single string.

## Usage Note {#usage-note}

Concatenate strings using the `strcat` or [`strcat-delim`](strcat-delim) function. Do not not rely on the `+` operator (with `"string1" + "string2"` notation).

## Examples

The first two examples return `hello world`:

```kusto {runSearch=true}
print str1="hello", str2="world" | extend newstr=strcat(str1," ", str2)
```

```kusto {runSearch=true}
print strcat("hello", " ", "world")
```

This example returns `"The value is: "`:

```kusto {runSearch=true}
print strcat('The value is: ', null)
```
