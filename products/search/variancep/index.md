# variancep



# ![](page/agg-icon.svg) variancep

The `variancep` aggregation function calculates the variance of **Expression** across the group, considering the group as a [population](https://en.wikipedia.org/wiki/Statistical_population).



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

Used formula:

![variancep formula](variance-population.c31ee085b8.png)
{scale="50%"}

## Syntax

    `variancep( Expression )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.

## Results

The variance value of **Expression** across the group.

## Example

```kusto
dataset=myDataset
| summarize variancep(x)
```

