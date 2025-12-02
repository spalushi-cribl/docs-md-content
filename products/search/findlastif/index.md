# findlastif


# ![](page/agg-icon.svg) findlastif

The `findlastif` aggregation function returns the last observed non-`null` value of **Expression** across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `findlastif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

## Usage {#usage}

To find a latest event with respect to the `_time` field, instead use [`findlatestif`](findlatestif). 

You can use `findlastif` after a [`sort`](sort) or [`order`](order) operator when sorting by a non-time field. Once events are sorted, this function acts much like [`maxif`](maxif).

## Example

This example returns the birthday for all names that have more than 4 letters.

```kusto
dataset=myDataset
| summarize findlastif(day_of_birth, strlen(name) > 4)
```
