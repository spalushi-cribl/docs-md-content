# hash



The `hash` function returns a hash value for the input value.

## Syntax

    `hash( Source, Mod )`

### Arguments

* **Source**: The value to be hashed.
* **Mod**: An optional modulo value to be applied to the hash result, so that the output value is between `0` and `Mod - 1`.

## Returns

The hash value of **Source**. If **Mod** is specified, the function returns the hash value modulo the value of **Mod**.

## Examples

* `hash("World")` returns as `1846988464401551951`
* `hash("World", 100)` returns as `51 (1846988464401551951 % 100)`
* `hash(datetime("2015-01-01"))` returns as `1380966698541616202`

### Sampling

You can use the `hash` function for sampling data if the values in one of its columns are uniformly distributed. In the following example, StartTime values are uniformly distributed and the function is used to run a query on 10% of the data.

```kusto
dataset=Dataset
| where hash(StartTime, 10) == 0
| summarize StormCount = count(), TypeOfStorms = dcount(EventType) by State 
| top 5 by StormCount desc
```

