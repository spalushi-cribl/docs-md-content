# base64_encode_fromarray


The `base64_encode_fromarray` function encodes a base64 string from an array of characters or strings.

## Syntax

    `base64_encode_fromarray( BytesArray )`

### Arguments

* **BytesArray**: Input array of characters or strings to be concatenated and encoded into base64. (Numbers within the array will be stringified as individual numeric characters.)

## Results

Returns the base64 string encoded from the array of characters.

Trying to encode a base64 string from an invalid character array, which was generated from invalid UTF-8–encoded string, will return `null`.

## Examples

This example encodes an array of familiar characters into its base64 string equivalent:

```kusto {runSearch=true}
print enc=base64_encode_fromarray(dynamic(['C',"ri",'b','l']))
```

This example round-trips the input array to yield `["C","4","2","0",".","5","b","l"]`:

```kusto {runSearch=true}
print enc=base64_encode_fromarray(dynamic(['C',"420.5",'b','l'])),
dec=base64_decode_toarray(enc)
```
