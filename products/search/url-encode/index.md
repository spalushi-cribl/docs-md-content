# url_encode


The `url_encode` function converts characters of the input URL into a format that can be transmitted over the internet.

## Syntax

    `url_encode( URL )`

### Arguments

* **URL**: Input URL (string).

## Results

URL (string) converted into a format that can be transmitted over the internet.

## Examples

This example encodes user names – for example, replacing spaces with their `%20` hexadecimal equivalents:

```kusto {runSearch=true}
dataset="cribl_internal_logs" source="file:///opt/cribl/log/access.log" user="*" 
| extend encodedName=url_encode(user)
```
