# base64_encode_tostring


The `base64_encode_tostring` function encodes a string as base64 string.

## Syntax

    `base64_encode_tostring( String )`

### Arguments

* **String**: Input string to be encoded as base64 string.

## Results

Returns the **String** encoded as base64 string.

## Example

```kusto {runSearch=true}
print 
encoded_string=base64_encode_tostring("Cribl!$#@^%#. foo bar baz "),
decoded_string=base64_decode_tostring(encoded_string)
```


