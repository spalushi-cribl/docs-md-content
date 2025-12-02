# hash_md5



The `hash_md5` function returns an MD5 hash value for the input value.

## Syntax

    `hash_md5( Source )`

### Arguments

* **Source**: The value to be hashed.

## Returns

The MD5 hash value of the given scalar, encoded as a hex string (a string of characters, each two of which represent a single Hex number between 0 and 255).

## Examples

* `h1=hash_md5("World")`
* `h2=hash_md5(datetime(2020-01-01))`

### Aggregate by hash

This example uses the `hash_md5` function to aggregate StormEvents based on the State's MD5 hash value.

```kusto
dataset=StormEvents
| summarize StormCount = count() by State, StateHash=hash_md5(State)
| top 5 by StormCount
```

Returns the following:

State|StateHash|StormCount
-----|-----|-----
TEXAS|3b00dbe6e07e7485a1c12d36c8e9910a|4701
KANSAS|e1338d0ac8be43846cf9ae967bd02e7f|3166
IOWA|6d4a7c02942f093576149db764d4e2d2|2337
ILLINOIS|8c00d9e0b3fcd55aed5657e42cc40cf1|2022
MISSOURI|2d82f0c963c0763012b2539d469e5008|2016

