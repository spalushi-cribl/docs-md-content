# base64_decode_toarray


The `base64_decode_toarray` function decodes a base64 string to an array of single-character strings.

## Syntax

    `base64_decode_toarray( String )`

### Arguments

* **String**: Input base64 string to be decoded from base64 to an array of single-character strings.

## Results

Returns an array of single-character strings decoded from a base64 string.

## Examples

This example returns an array of letters that spell out "Cribl":

```kusto {runSearch=true}
print dec=base64_decode_toarray("Q3JpYmw=")
```

This example decodes a base64 encoding of an array that began as `['C',"420.5",'b','l']`:

```kusto {runSearch=true}
print dec=base64_decode_toarray("QzQyMC41Ymw=")
```