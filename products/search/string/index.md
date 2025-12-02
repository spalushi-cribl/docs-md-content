# string


The `string` data type represents a sequence of zero or more [Unicode](https://home.unicode.org/) characters.

## `string` Literals

There are several ways to encode literals of the `string` data type in a query text:

- Enclose the string in double-quotes
  `"`:<br />`"This is a string literal. Single quote characters (') don't require escaping. Double quote characters (\") are escaped by a backslash (\\)."`
- Enclose the string in single-quotes
  `'`:<br />`'Another string literal. Single quote characters (\') require escaping by a backslash (\\). Double quote characters (") do not require escaping.'`

In the two representations above, the backslash `\` character indicates escaping. The backslash is used to escape the
enclosing quote characters, tab characters `\t`, newline characters `\n`, and itself `\\`.

The newline character `\n` and the return character `\r` can't be included as part of the string literal without being
quoted.

## Verbatim `string` Literals

Verbatim string literals are also supported. You specify them by prepending an asperand `@` character. In this form, the
backslash character `\` stands for itself, and not as an escape character.

- Enclose in double-quotes
  `"`:<br />`@"This is a verbatim string literal that ends with a backslash\. Double quote characters ("") are escaped by a double quote ("")."`
- Enclose in single-quotes
  `'`:<br />`@'This is a verbatim string literal that ends with a backslash\. Single quote characters ('') are escaped by a single quote ('').'`

## Concatenation

Joining two or more string literals is supported. For example:

```kusto {runSearch=true}
dataset="$vt_dummy"
 | extend x = strcat('Hello', ', ', "world!")
```

Returns `x` as `Hello, world!`.

## Obfuscation

Obfuscating strings is not supported, however, the query will not complain if attempted.

## `string` Examples

- `string(123)` returns `"123"`
- `string("hello")` returns `"hello"`
- `string(null)` returns `null`
