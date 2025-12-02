# make_bag


# ![](page/agg-icon.svg) make_bag

The `make_bag` aggregation function creates a property bag from multiple input bags.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `
make_bag( Expression, [ MaxOutputSize ] )`

### Parameters

| Name            | Type                 | Required | Description                                                           |
| --------------- | -------------------- | -------- | --------------------------------------------------------------------- |
| `Expression`    | [`dynamic`](dynamic) | Yes      | Expression used for the aggregation calculation.                      |
| `MaxOutputSize` | [`int`](int)         | No       | Maximum number of elements returned. Default: `1048576`. Max: `2^53`. |

## Returns

Returns a single property bag, made of those `Expression` values that are property bags (other values are ignored).

If a key appears in the input more than once, only its **first value** is kept.

The order of the key-value pairs returned is undetermined.

## Example

```kusto {runSearch=true}
range v from 1 to 3
 | extend foo = case(v == 1, dynamic({"a": 1, "foo": 42}), v == 2, dynamic({"b": 2, "foo": 17}), dynamic({"c": 3}))
 | summarize mb=make_bag(foo)
```

Input (before `make_bag`):

```kusto
// event 1
{
  "_time": 1735830806.91,
  "v": 1,
  "foo": {
    "a": 1,
    "foo": 42
  }
}

// event 2
{
  "_time": 1735830806.91,
  "v": 2,
  "foo": {
    "b": 2,
    "foo": 17
  }
}

// event 3
{
  "_time": 1735830806.91,
  "v": 3,
  "foo": {
    "c": 3
  }
}
```

Output (after `make_bag`):

```kusto
{
  "mb": {
    "a": 1,
    "foo": 42,
    "b": 2,
    "c": 3
  }
}
```
