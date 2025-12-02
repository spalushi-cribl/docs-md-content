# base64_decode_tostring


The `base64_decode_tostring` function decodes a base64 string to a UTF-8 string.

## Syntax

    `base64_decode_tostring( String )`

### Arguments

* **String**: Input string to be decoded from base64 to UTF-8 string.

## Results

Returns UTF-8 **String** decoded from base64 string.

Trying to decode a base64 string that was generated from invalid UTF-8 encoding will return null.

## Example

```kusto {runSearch=true}
print 
encoded_string=base64_encode_tostring("Cribl!$#@^%#. foo bar baz "),
decoded_string=base64_decode_tostring(encoded_string)
```
