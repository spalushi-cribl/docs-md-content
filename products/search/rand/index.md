# rand


The `rand` function returns a random number.

## Syntax

* `rand()` - returns a value of type `real` with a uniform distribution in the range [0.0, 1.0).
* `rand( N )` - returns a value of type `real` chosen with a uniform distribution from the set {0.0, 1.0, ..., N - 1}.

## Examples {#examples}

```kusto {runSearch=true}
print rand()
```

```kusto {runSearch=true}
print rand( 1000 )
```
