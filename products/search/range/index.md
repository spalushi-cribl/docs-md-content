# range


The `range` function generates a dynamic array, holding a series of equally spaced values.

> To generate a series of events, use the `range` [operator](range-operator).
>
{.box .info}

## Syntax

    `range( Start, Stop[, Step] )`

## Arguments

* **Start**: The value of the first element in the resulting array.
* **Stop**: The value of the last element in the resulting array, or the least value that is greater than the last element in the resulting array and within an integer multiple of step from start.
* **Step**: The difference between two consecutive elements of the array. The default value for step is `1` for numeric and `1h` for `timespan` or `datetime`.

## Results

Dynamic array whose values are: `Start`, `Start` + `Step`, ... up to and including `Stop`. The array will be truncated if the maximum number of values is reached.

## Examples {#examples}

This example returns `[1, 4, 7]`

```kusto {runSearch=true}
print range( 1, 8, 3 )
```

This example returns an array holding all days in the year 2015, as epoch times:

```kusto {runSearch=true}
print range( datetime(2015-01-01), datetime(2015-12-31), 1d )
```

This example returns `[1,2,3]`:

```kusto {runSearch=true}
print range( 1, 3 )
```



This example returns `1`:

```kusto {runSearch=true}
print range( 1,1000000000 ) | mv-expand r | count
```
