# make_bag_if


# ![](page/agg-icon.svg) make_bag_if

The `make_bag_if` aggregation function creates a property bag from those input bags that meet the specified condition.



Use this function with the [`summarize`](summarize), [`eventstats`](eventstats), and [`timestats`](timestats) operators.

## Syntax

    `
make_bag_if( Expression, Predicate, [ MaxOutputSize ] )`

### Parameters

| Name            | Type                 | Required | Description                                                                          |
| --------------- | -------------------- | -------- | ------------------------------------------------------------------------------------ |
| `Expression`    | [`dynamic`](dynamic) | Yes      | Expression used for the aggregation calculation.                                     |
| `Predicate`     | [`bool`](bool)       | Yes      | Predicate that must evalute to `true` for an input bag to be included in the output. |
| `MaxOutputSize` | [`int`](int)         | No       | Maximum number of elements returned. Default: `1048576`. Max: `2^53`.                |

## Returns

Returns a single property bag, made of those `Expression` values that are property bags (other values are ignored) and
for whose the `Predicate` evaluates to `true`.

If a key appears in the input more than once, only its **first value** is kept.

The order of the key-value pairs returned is undetermined.

## Example

```kusto {runSearch=true}
range v from 1 to 3
 | extend foo = case(v == 1, dynamic({"a": 1, "foo": 42}), v == 2, dynamic({"b": 2, "foo": 17}), dynamic({"c": 3}))
 | summarize mb=make_bag_if(foo,v > 1)
```

Input (before `make_bag_if`):

```kusto
// event 1
{
  "_time": 1735831049.413,
  "v": 1,
  "foo": {
    "a": 1,
    "foo": 42
  }
}

// event 2
{
  "_time": 1735831049.413,
  "v": 2,
  "foo": {
    "b": 2,
    "foo": 17
  }
}

// event 3
{
  "_time": 1735831049.413,
  "v": 3,
  "foo": {
    "c": 3
  }
}
```

Output (after `make_bag_if`):

```kusto
{
  "mb": {
    "b": 2,
    "foo": 17,
    "c": 3
  }
}
```
