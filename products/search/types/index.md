# Types


Supported types of data in Cribl Search.

---

Every data value (such as the value of an expression, or the parameter to a function) has a **type**.

The following table lists the types supported by Cribl Search, alongside additional aliases you can use to refer to
them.

| Type                   | Aliases                              |
| ---------------------- | ------------------------------------ |
| [`bool`](bool)         | `boolean`                            |
| [`datetime`](datetime) | `date`                               |
| [`double`](double)     | [`real`](real), [`decimal`](decimal) |
| [`dynamic`](dynamic)   |                                      |
| [`int`](int)           | [`long`](long)                       |
| [`string`](string)     |                                      |
| [`timespan`](timespan) | `time`                               |

All types include the special [`null`](null) value, which represents the lack of data or a mismatch of data. For
example, attempting to ingest the string `"abc"` into an `int` column results in [`null`](null).

## Unsupported types

The following table shows the types unsupported by Cribl Search:

| Type     | Alias   |
| -------- | ------- |
| `float`  |         |
| `int16`  |         |
| `uint16` |         |
| `uint32` | `uint`  |
| `uint64` | `ulong` |
| `uint8`  | `byte`  |
