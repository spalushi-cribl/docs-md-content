# findfirstif


# ![](page/agg-icon.svg) findfirstif

The `findfirstif` aggregation function returns the first observed non-`null` value of **Expression** across the group for which **Predicate** evaluates to `true`.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `findfirstif( Expression, Predicate )`

### Arguments

* **Expression**: Expression used for aggregation calculation. Wildcards are not supported for field names.
* **Predicate**: Expression that will be used to filter rows.

## Usage {#usage}

To find an earliest event with respect to the `_time` field, instead use [`findearliestif`](findearliestif). 

You can use `findfirstif` after a [`sort`](sort) or [`order`](order) operator when sorting by a non-time field. Once events are sorted, this function acts much like [`minif`](minif).

## Example

This example returns the birthday for all names that have more than 4 letters.

```kusto
dataset=myDataset
| summarize findfirstif(day_of_birth, strlen(name) > 4)
```
