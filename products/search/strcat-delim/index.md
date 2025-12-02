# strcat_delim


The `strcat_delim` function concatenates between 2 and 64 arguments, with the specified delimiter.

## Syntax

`strcat_delim( Delimiter, Argument1, Argument2 [, ArgumentN ] )`

### Arguments

- **Delimiter**: String expression, which will be used as separator.
- **Argument1 ... ArgumentN**: Expressions to be concatenated.

If an argument is not of the [`string`](string) type, it's forcibly converted to [`string`](string).

If an argument is [`null`](null), it's converted to a blank string.

## Results

Arguments, concatenated to a single string with **Delimiter**.

## Usage Note {#usage-note}

Concatenate strings using the `strcat-delim` or [`strcat`](strcat) function. Do not not rely on the `+` operator (with `"string1" + "string2"` notation).

{#example}
## Examples

```kusto {runSearch=true}
print str1="hello", str2="world" | extend newstr=strcat_delim("|",str1,str2)
```

```kusto {runSearch=true}
print strcat_delim('-', 1, '2', 'A', 1s)
```
