# hash_xxhash64



The `hash_xxhash64` function returns a 64-bit hash value for the input value.

## Syntax

    `hash_xxhash64( Source, Mod )`

### Arguments

* **Source**: The value to be hashed.
* **Mod**: An optional modulo value to be applied to the hash result, so that the output value is between `0` and `Mod - 1`.

## Returns

The hash value of **Source**. If **Mod** is specified, the function returns the hash value modulo the value of **Mod**.

## Examples

* `xxhash64("World")` returns as `1846988464401551951`
* `xxhash64("World", 100)` returns as `51 (1846988464401551951 % 100)`
* `xxhash64(datetime("2015-01-01"))` returns as `1380966698541616202`

