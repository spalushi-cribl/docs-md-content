# isnull



The `isnull` function evaluates its sole argument and returns a [`bool`](bool) value indicating whether the argument evaluates to a null value.

## Syntax

    `isnull( [ Expression ] )`

## Results

True or false, depending on whether the value is or is not null.

`string` values cannot be null. Use [isempty](isempty) to determine whether a value of type `string` is empty.

x|isnull(x)
-----|-----
""|false
"x"|false
parse_json("")|true
parse_json("[]")|false
parse_json("{}")|false

## Example

This example returns `true`:

```kusto {runSearch=true}
print isnull( parse_json("No JSON here") )
```

