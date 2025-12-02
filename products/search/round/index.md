# round


The `round` function returns the rounded source to the specified precision.

## Syntax

    `round( Source [, Precision] )`

## Arguments

* **Source**: The source scalar the round is calculated on.
* **Precision**: Number of digits the source will be rounded to. (default value is 0)

## Results

The rounded source to the specified precision.

Round is different than [`bin`](bin) and [`floor`](floor) in that the first rounds a number to a specific number of digits while the last rounds value to an integer multiple of a given bin size (round(2.15, 1) returns 2.2 while bin(2.15, 1) returns 2).

## Examples {#examples}

This example returns `2.2`:

```kusto {runSearch=true}
print round( 2.15, 1 )
```

This example returns `2`.  It's equivalent to `print round( 2.15, 0 )`:

```kusto {runSearch=true}
print round( 2.15 )
```

This example returns `-100`:

```kusto {runSearch=true}
print round( -50.55, -2 )
```

This example returns `20`:

```kusto {runSearch=true}
print round( 21.5, -1 )
```
