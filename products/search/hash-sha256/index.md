# hash_sha256



The `hash_sha256` function returns a SHA-256 hash value for the input value.

## Syntax

    `hash_sha256( Source )`

### Arguments

* **Source**: The value to be hashed.

## Returns

The SHA-256 hash value of the given scalar, encoded as a hex string (a string of characters, each two of which represent a single Hex number between 0 and 255).

## Examples

* `h1=hash_sha256("World")`
* `h2=hash_sha256(datetime(2020-01-01))`

### Aggregate by hash

This example uses the `hash_sha256` function to aggregate StormEvents based on the State's SHA-256 hash value.

```kusto
dataset=StormEvents
| summarize StormCount = count() by State, StateHash=hash_sha256(State)
| top 5 by StormCount desc
```

Returns the following:

State|StateHash|StormCount
-----|-----|-----
TEXAS|9087f20f23f91b5a77e8406846117049029e6798ebbd0d38aea68da73a00ca37|4701
KANSAS|c80e328393541a3181b258cdb4da4d00587c5045e8cf3bb6c8fdb7016b69cc2e|3166
IOWA|f85893dca466f779410f65cd904fdc4622de49e119ad4e7c7e4a291ceed1820b|2337
ILLINOIS|ae3eeabfd7eba3d9a4ccbfed6a9b8cff269dc43255906476282e0184cf81b7fd|2022
MISSOURI|d15dfc28abc3ee73b7d1f664a35980167ca96f6f90e034db2a6525c0b8ba61b1|2016

