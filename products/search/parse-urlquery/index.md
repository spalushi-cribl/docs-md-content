# parse_urlquery


The `parse_urlquery` function returns a `dynamic` object that contains URL query parameters from an input string.

## Syntax

    `parse_urlquery( Query )`

### Arguments

* **Query**: A string represents a URL query. Format should follow URL query standards `key=value& ...`.


## Results

An object of type `dynamic` that includes URL query parameters from the input string.

## Example

```kusto {runSearch=true}
print theData="k1=v1&k2=v2&k3=v3" | extend parsed_data=parse_urlquery(theData), theQueryParameters=parsed_data["Query Parameters"] |  render event
```

This example:

```kusto
parse_urlquery("k1=v1&k2=v2&k3=v3")
```

...returns:


```json
{
  "Query Parameters":"{"k1":"v1", "k2":"v2", "k3":"v3"}",
}
 ```
