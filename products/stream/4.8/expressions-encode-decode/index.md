# C.Decode and C.Encode - Encoding and Decoding


## C.Decode – Data Decoding Methods {#decode}

### `C.Decode.base64()`

```js
Decode.base64(val: string, resultEnc: string = 'utf8'): string | Buffer | undefined
```

Performs base64 decoding of the given string.

Returns a string or Buffer, depending on the `resultEnc` value.

|Parameter|Type|Description|
|---|---|---|
|`val` | string | Value to decode.
|`resultEnc` | `'utf8'` \| `'utf8-valid'` \| `'buffer'` | Encoding to use to convert the binary data to a string. Use `'utf8‑valid'` to validate that result is valid UTF8; use `'buffer'` if you need the binary data in a Buffer. Defaults to `'utf8'`. 

### Examples

To decode an `encoded` field and return the decoded value as a Buffer, use:

```js
C.Decode.base64(encoded, 'buffer')
```

### `C.Decode.gzip()`

```js
Decode.gzip(value: Buffer | string, encoding?: BufferEncoding) : string
```

Gunzips the supplied value.

|Parameter|Type|Description|
|---|---|---|
|`value` | any | Value to gunzip.
|`encoding` | string | Encoding of `value`, for example: `'base64'`, `'hex'`, `'utf-8'`, `'binary'`. Default is `'base64'`. If data is received as Buffer (from gzip with encoding:`'none'`), decoding is skipped.

### Examples

To decode a `hostname` field which had been encoded using `hex`, run:

```js
C.Decode.gzip(hostname, 'hex')
```

### `C.Decode.hex()`

```js
Decode.hex(val: string): number
```

Performs hex to number conversion.
Returns `NaN` if value cannot be converted to a number.

|Parameter|Type|Description|
|---|---|---|
|`val` | string | Hex string to parse to a number (for example, "0xcafe").

### `C.Decode.uri()`

```js
Decode.uri(val: string): string
```

Performs URI-decoding of the given string.

|Parameter|Type|Description|
|---|---|---|
|`val` | string | Value to decode.

### `C.Decode.inflate()`

```js
Decode.inflate(value: Buffer | string, encoding: BufferEncoding = 'base64', isRaw = false): string
```

Inflates the supplied value.

|Parameter|Type|Description|
|---|---|---|
|`value` | string \| Buffer | The value to inflate. May be a Buffer or a string.
|`encoding` | string | If a string `value` is provided, this specifies how it has been encoded. If no `encoding` is provided, `base64` is used. This parameter is ignored if `value` is already a Buffer.
|`isRaw` | boolean | Indicates whether the data is in raw deflate format (without zlib headers). Default value is `false`.

### Examples

To base64-decode and inflate the value contained in a `_raw` field, you can use:

```js
C.Decode.inflate(_raw)
```

To additionally specify a different encoding to use, run:

```js
C.Decode.inflate(_raw, 'hex')
```

## C.Encode – Data Encoding Methods {#encode}

### `C.Encode.base64()`

```js
Encode.base64(val: string | Buffer, trimTrailEq: boolean = false, encoding?: BufferEncoding): string
```

Returns a base64 representation of the given string or Buffer.

|Parameter|Type|Description|
|---|---|---|
|`val` | any | Value to encode.
|`trimTrailEq` | boolean | Whether to trim any trailing `=`.
|`encoding` | string | Optional parameter that specifies the encoding of the value. Options include `base64`, `hex`, `utf-8`, and `binary`. The default is `utf-8`.

### Examples

To base64-encode the `username` field, and remove the trailing `=`, use:

```js
C.Encode.base64(username, true)
```

To base64-encode the `hexValue` field, where the value of the field is a hex string:

```js
C.Encode.base64('4672616e6b7575757575', undefined, 'hex')
```
### `C.Encode.gzip()`

`Encode.gzip(value: Buffer | string, encoding?: string) : string | Buffer`

Gzips, and optionally base64-encodes, the supplied value.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | Value to gzip.
|`encoding` | string | Encoding of `value`, for example: `'base64'`, `'hex'`, `'utf-8'`, `'binary'`, `'none'`. Default is `'base64'`. If `'none'` is specified, data will be returned as a Buffer.

### Examples

To encode the `hostname` field, for example using `hex` encoding, run:

```js
C.Encode.gzip(hostname, 'hex')
```

### `C.Encode.hex()`

```js
Encode.hex(val: string | number): string
```

Rounds the number to an integer and returns its hex representation (lowercase). If a string is provided,  it will be parsed into a number or `NaN`.

|Parameter|Type|Description|
|---|---|---|
|`val` | string \| number | Value to convert to hex.

### `C.Encode.uri()`

```js
Encode.uri(val: string): string
```

Returns the URI-encoded representation of the given string.

|Parameter|Type|Description|
|---|---|---|
|`val` | string | Value to encode.

### `C.Encode.deflate()`

```js
Encode.deflate(value: Buffer | string, encoding: BufferEncoding | 'none' = 'base64', toRaw = false): string | Buffer
```

Deflates and optionally base64-encodes the supplied value.

|Parameter|Type|Description|
|---|---|---|
|`value` | string \| Buffer | Value to deflate.
|`encoding` | string | Encoding to apply to the deflated result, for example: `base64`, `hex`, `utf-8`, `binary`, `none`. Default is `base64`. If `none` is specified, data will be returned as a Buffer.
|`toRaw` | boolean | Indicates whether to return the raw deflated data, without zlib headers. Default is `false`, which will prepend normal zlib header bytes to the returned value.

### Examples

To deflate and hex-decode the value provided in an `inflated` field, run:

```js
C.Encode.deflate(inflated, 'hex')
```
