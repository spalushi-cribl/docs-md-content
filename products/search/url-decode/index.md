# url_decode


The `url_decode` function converts encoded URL into a regular URL representation.

## Syntax

    `url_decode( EncodedURL )`

### Arguments

* **EncodedURL**: Encoded URL (string).

## Results

URL (string) in a regular representation.

## Examples

This example decodes endpoints from API calls:

```kusto {runSearch=true}
dataset="cribl_internal_logs" source="file:///opt/cribl/log/access.log" 
| where match_regex(url, "/filterExp/") 
| extend parsedUrl=url_decode(url)
```
