# hash_sha1



The `hash_sha1` function returns a SHA1 hash value for the input value.

## Syntax

    `hash_sha1( Source )`

### Arguments

* **Source**: The value to be hashed.

## Returns

The SHA1 hash value of the given scalar, encoded as a hex string (a string of characters, each two of which represent a single Hex number between 0 and 255).

## Examples

* `h1=hash_sha1("World")`
* `h2=hash_sha1(datetime(2020-01-01))`

### Aggregate by hash

This example uses the `hash_sha1` function to aggregate StormEvents based on the State's SHA1 hash value.

```kusto
dataset=StormEvents
| summarize StormCount = count() by State, StateHash=hash_sha1(State)
| top 5 by StormCount desc
```

Returns the following:

State|StateHash|StormCount
-----|-----|-----
TEXAS|3128d805194d4e6141766cc846778eeacb12e3ea|4701
KANSAS|ea926e17098148921e472b1a760cd5a8117e84d6|3166
IOWA|cacf86ec119cfd5b574bde5b59604774de3273db|2337
ILLINOIS|03740763b16dae9d799097f51623fe635d8c4852|2022
MISSOURI|26d938907240121b54d9e039473dacc96e712f61|2016

