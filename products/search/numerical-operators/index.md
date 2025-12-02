# Numerical Operators


A list of numerical operators supported by Cribl Search.

---

Numerical operators perform arithmetic operations on values of the following types:

- [`double`](double) (aliases: [`real`](real), [`decimal`](decimal))
- [`int`](int) (alias: [`long`](long))

You can use the following numerical operators between pairs of the above value types:


| Operator | Description              |
| -------- | ------------------------ |
| `==`     | Equal                    |
| `!=`     | Not Equal                |
| `>`      | Greater Than             |
| `>=`     | Greater Than or Equal To |
| `<`      | Less Than                |
| `<=`     | Less Than or Equal To    |
| `+`      | Add                      |
| `-`      | Subtract                 |
| `*`      | Multiply                 |
| `/`      | Divide                   |
| `%`      | Modulo                   |



> Don't use the `+` operator to concatenate strings (with the `"string1" +
> "string2"` notation). Use the [`strcat-delim`](strcat-delim) or [`strcat`](strcat)
> functions instead. 
{.box .warning}

See also: [`null` and Binary Operators](null#binary).
