# dynamic


The `dynamic` scalar data type is special in that it can take on any value of other scalar data types from the list
below, as well as arrays and property bags.

Specifically, a `dynamic` value can be any of the following:

- A value of any of the primitive scalar data types: [`bool`](bool), [`datetime`](datetime), [`int`](int),
  [`long`](long), [`real`](real), [`string`](string), and [`timespan`](timespan).
- A **dynamic array** (an array of `dynamic` values), holding zero or more values with zero-based indexing.
- A **property bag** (or **dictionary**) that maps keys (unique `string` values) to `dynamic` values. A property bag has
  zero or more such mappings (called **slots**), indexed by the keys. Slots are unordered.<br>Cribl Search doesn't
  attempt to preserve the order of key-to-value mappings in a property bag, and so you can't assume the order to be
  preserved. It's entirely possible for two property bags with the same set of mappings to yield different results when
  they are represented as `string` values.
- [`null`](null).

## `dynamic` Literals

A literal of type `dynamic` looks like this:

`dynamic(` _Value_ `)`

_Value_ can be:

- `null`, in which case the literal represents the null dynamic value: `dynamic(null)`.
- Another scalar data type literal, in which case the literal represents the `dynamic` literal of the "inner" type. For
  example, `dynamic(4)` is a dynamic value holding the value 4 of the long scalar data type.
- An array of dynamic or other literals: `[` _ListOfValues_ `]`. For example, `dynamic([1, 2, "hello"])` is a dynamic
  array of three elements, two `long` values and one `string` value.
- A property bag: `{` _Key_ `=` _Value_ ... `}`. For example, `dynamic({"a":1, "b":{"a":2}})` is a property bag with two
  slots, `a`, and `b`, with the second slot being another property bag.

## `dynamic` Object Accessors

To subscript a dictionary (in other words, reference a key in a property bag), use either dot notation (`dict.key`) or
bracket notation (`dict["key"]`). When the subscript is a string constant, both options are equivalent.

To use an expression as the subscript, use bracket notation. When using arithmetic expressions, the expression inside
the brackets must be wrapped in parentheses.

In the examples below `dict` and `arr` are columns of dynamic type:

| Expression                          | Accessor expression type             | Meaning                                                                              | Comments                                                                       |
| ----------------------------------- | ------------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| `dict[col]`                         | Entity name (column)                 | Subscripts a dictionary using the values of the column `col` as the key              | Column must be of type string                                                  |
| `arr[index]`                        | Entity index (column)                | Subscripts an array using the values of the column `index` as the index              | Column must be of type integer or boolean                                      |
| `arr[-index]`                       | Entity index (column)                | Retrieves the 'index'-th value from the end of the array                             | Column must be of type integer or boolean                                      |
| `arr[(-1)]`                         | Entity index                         | Retrieves the last value in the array                                                |                                                                                |
| `arr[toint(indexAsString)]`         | Function call                        | Casts the values of column `indexAsString` to int and use them to subscript an array |                                                                                |
| `dict[['where']]`                   | Keyword used as entity name (column) | Subscripts a dictionary using the values of column `where` as the key                | Entity names that are identical to some query language keywords must be quoted |
| `dict.['where']` or `dict['where']` | Constant                             | Subscripts a dictionary using `where` string as the key                              |                                                                                |

## Casting `dynamic` Objects

After subscripting a dynamic object, you must cast the value to a simple type.

| Expression           | Value                                            | Type       |
| -------------------- | ------------------------------------------------ | ---------- |
| `X`                  | `parse_json('[100,101,102]')`                    | array      |
| `X[0`]               | `parse_json('100')`                              | dynamic    |
| `toint(X[1])`        | `101`                                            | int        |
| `Y`                  | `parse_json('{"a1":100, "a b c":"2015-01-01"}')` | dictionary |
| `Y.a1`               | `parse_json('100')`                              | dynamic    |
| `Y["a b c"]`         | `parse_json("2015-01-01")`                       | dynamic    |
| `todate(Y["a b c"])` | `datetime(2015-01-01)`                           | datetime   |

Cast functions are:

- [tolong](tolong)
- [todouble](todouble)
- [todatetime](todatetime)
- [totimespan](totimespan)
- [tostring](tostring)
- [parse_json](parse-json)

## Functions Over `dynamic` Types {#functions}

Dynamic scalar functions allow you to manipulate objects by operating on `dynamic` values, including dynamic arrays and
property bags.


| Name                                   | Description                                                                     |
| -------------------------------------- | ------------------------------------------------------------------------------- |
| [`bag_has_key`](bag-has-key)           | Checks whether a property bag contains a given key.                             |
| [`bag_keys`](bag-keys)                 | Lists all root keys of a property bag.                                          |
| [`bag_merge`](bag-merge)               | Merges multiple property bags, discarding duplicate keys.                       |
| [`bag_pack`](bag-pack)                 | Creates a property bag from an alternating list of keys and values.             |
| [`bag_pack_columns`](bag-pack-columns) | Creates a property bag from a list of columns.                                  |
| [`bag_remove_keys`](bag-remove-keys)   | Removes key-value pairs from a property bag.                                    |
| [`bag_set_key`](bag-set-key)           | Adds or overwrites a key-value pair in a property bag.                          |
| [`bag_zip`](bag-zip)                   | Creates a property bag from two dynamic arrays.                                 |
| [`make_bag`](make-bag)                 | Creates a property bag from multiple input bags.                                |
| [`make_bag_if`](make-bag-if)           | Creates a property bag from those input bags that meet the specified condition. |
| [`zip`](zip)                           | Merges dynamic arrays, grouping elements by index.                              |

The following [string functions](string-functions) support `dynamic` types as well:

| Function                                         | Usage with dynamic data types              |
| ------------------------------------------------ | ------------------------------------------ |
| [`` extractjson(`path,object`) ``](extract-json) | Uses path to navigate into object.         |
| [`` parse_json(`source`) ``](parse-json)         | Turns a JSON string into a dynamic object. |
| [`` range(`from,to,step`) ``](range)             | Generates an array of values.              |

