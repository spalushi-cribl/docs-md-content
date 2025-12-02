# parse_url


The `parse_url` function parses an absolute URL `string` and returns a `dynamic` object that contains URL parts.

## Syntax

    `parse_url( URL )`

### Arguments

* **URL**: A string representing a URL or the query part of the URL.

## Results

An object of type `dynamic` that included the URL components: Scheme, Host, Port, Path, Username, Password, Query Parameters, Fragment.

## Example

```kusto {runSearch=true}
print theData="scheme://username:password@the.host.com:1234/this/is/a/path?k1=v1&k2=v2#fragment" | extend parsed_data=parse_url(theData) | render event
```

```kusto {runSearch=true}
print theData="scheme://username:password@the.host.com:1234/this/is/a/path?k1=v1&k2=v2#fragment" | extend parsed_data=parse_url(theData) | extend query = parsed_data.Query_Parameters | render event
```

This example:

```kusto
parse_url("scheme://username:password@host:1234/this/is/a/path?k1=v1&k2=v2#fragment")
```

...returns:

```json
{
 	"Scheme":"scheme",
 	"Host":"host",
 	"Port":"1234",
 	"Path":"this/is/a/path",
 	"Username":"username",
 	"Password":"password",
 	"Query Parameters":"{"k1":"v1", "k2":"v2"}",
 	"Fragment":"fragment"
 }
 ```
