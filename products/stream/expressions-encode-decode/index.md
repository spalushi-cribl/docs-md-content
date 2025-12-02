# C.Decode and C.Encode - Encoding and Decoding


## C.Decode – Data Decoding Methods {#decode}

### `C.Decode.base64()`

```js
C.Decode.base64(val: string, resultEnc: string = 'utf8'): string | Buffer | undefined
```

Performs Base64 decoding of the given string.

Returns a string or Buffer, depending on the `resultEnc` value.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`val` | string | Value to decode. |
|`resultEnc` | `'utf8'` \| `'utf8-valid'` \| `'buffer'` | Encoding to use to convert the binary data to a string. Use `'utf8‑valid'` to validate that result is valid UTF8; use `'buffer'` if you need the binary data in a Buffer. Defaults to `'utf8'`. |

#### Examples

To decode an `encoded` field and return the decoded value as a Buffer, use:

```js
C.Decode.base64(encoded, 'buffer')
```

### `C.Decode.gzip()`

```js
C.Decode.gzip(value: Buffer | string, encoding?: BufferEncoding) : string
```

Gunzips the supplied value.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | any | Value to gunzip. |
|`encoding` | string | Encoding of `value`, for example: `'base64'`, `'hex'`, `'utf-8'`, `'binary'`. Default is `'base64'`. If data is received as Buffer (from gzip with encoding:`'none'`), decoding is skipped. |

#### Example

To decode a `hostname` field which had been encoded using `hex`, run:

```js
C.Decode.gzip(hostname, 'hex')
```

### `C.Decode.hex()`

```js
C.Decode.hex(val: string): number
```

Performs hex to number conversion.
Returns `NaN` if value cannot be converted to a number.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`val` | string | Hex string to parse to a number (for example, `0xcafe`). |

### `C.Decode.inflate()`

```js
C.Decode.inflate(value: Buffer | string, encoding: BufferEncoding = 'base64', isRaw = false): string
```

Inflates the supplied value.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string \| Buffer | The value to inflate. May be a Buffer or a string. |
|`encoding` | string | If a string `value` is provided, this specifies how it has been encoded. If no `encoding` is provided, `base64` is used. This parameter is ignored if `value` is already a Buffer. |
|`isRaw` | boolean | Indicates whether the data is in raw deflate format (without zlib headers). Default value is `false`. |

#### Examples

To Base64-decode and inflate the value contained in a `_raw` field, you can use:

```js
C.Decode.inflate(_raw)
```

To additionally specify a different encoding to use, run:

```js
C.Decode.inflate(_raw, 'hex')
```

### `C.Decode.mime()`

```js
C.Decode.mime(val: string): string
```

Decode string that has been encoded with [MIME RFC 2047 standard](https://datatracker.ietf.org/doc/html/rfc2047). This standard is frequently used in email subject lines within logs to handle non-ASCII characters.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `val` | string | The input string to decode. This string must be formatted according to RFC 2047, typically starting and ending with `=?<charset>?<encodingType>?...?=`. |

#### Examples

To decode a MIME-encoded string using the Quoted-Printable format, typically seen when the non-ASCII content is mostly readable text, use:

```js
 C.Decode.mime('=?UTF-8?Q?R=C3=A9sum=C3=A9?=') // Returns: 'Résumé'
```

To decode a MIME-encoded string using the Base64 format, often used when the non-ASCII content is complex or binary data, use:

```js
C.Decode.mime('=?UTF-8?B?Q2Fmw6k=?=') // Returns: 'Café'.

### `C.Decode.uri()`

```js
C.Decode.uri(val: string): string
```

Performs URI-decoding of the given string.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`val` | string | Value to decode. |

## C.Encode – Data Encoding Methods {#encode}

### `C.Encode.base64()`

```js
C.Encode.base64(val: string | Buffer, trimTrailEq: boolean = false, encoding?: BufferEncoding): string
```

Returns a Base64 representation of the given string or Buffer.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`val` | any | Value to encode. |
|`trimTrailEq` | boolean | Whether to trim any trailing `=`. |
|`encoding` | string | Optional parameter that specifies the encoding of the value. Options include `base64`, `hex`, `utf-8`, and `binary`. The default is `utf-8`. |

#### Examples

To Base64-encode the `username` field, and remove the trailing `=`, use:

```js
C.Encode.base64(username, true)
```

To Base64-encode the `hexValue` field, where the value of the field is a hex string:

```js
C.Encode.base64('4672616e6b7575757575', undefined, 'hex')
```

### `C.Encode.deflate()`

```js
C.Encode.deflate(value: Buffer | string, encoding: BufferEncoding | 'none' = 'base64', toRaw = false): string | Buffer
```

Deflates and optionally Base64-encodes the supplied value.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string or Buffer | Value to deflate. |
|`encoding` | string | Encoding to apply to the deflated result, for example: `base64`, `hex`, `utf-8`, `binary`, `none`. Default is `base64`. If `none` is specified, data will be returned as a Buffer. |
|`toRaw` | boolean | Indicates whether to return the raw deflated data, without zlib headers. Default is `false`, which will prepend normal zlib header bytes to the returned value. |

#### Examples

To deflate and hex-decode the value provided in an `inflated` field, run:

```js
C.Encode.deflate(inflated, 'hex')
```


### `C.Encode.gzip()`

```js
C.Encode.gzip(value: Buffer | string, encoding?: string) : string | Buffer
```

Gzips, and optionally Base64-encodes, the supplied value.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | Value to gzip. |
|`encoding` | string | Encoding of `value`, for example: `'base64'`, `'hex'`, `'utf-8'`, `'binary'`, `'none'`. Default is `'base64'`. If `'none'` is specified, data will be returned as a Buffer. |

#### Examples

To encode the `hostname` field, for example using `hex` encoding, run:

```js
C.Encode.gzip(hostname, 'hex')
```

### `C.Encode.hex()`

```js
C.Encode.hex(val: string | number): string
```

Rounds the number to an integer and returns its hex representation (lowercase). If a string is provided,  it will be parsed into a number or `NaN`.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`val` | string \| number | Value to convert to hex. |

### `C.Encode.mime()`

```js
C.Encode.mime(val: string, encoding: string = 'Q', maxLineLength: number = 0, charset?: string): string
```

Encode a string with [MIME RFC 2047 standard](https://datatracker.ietf.org/doc/html/rfc2047). This method is used to prepare header values that contain non-ASCII characters for transport, such as email subject lines.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `val` | string | The raw string to be encoded, including any non-ASCII characters. |
| `encoding` | MIME Encoding | Defines the encoding type. Use `Q` for Quoted-Printable, which is the default and can be omitted. Use `B` for Base64. |
| `maxLineLength` | number | Used for folding long lines, which is required by the RFC for headers. If greater than zero, inserts a newline delimiter after this defined number characters to ensure line wrapping. If `0`, no folding is performed. |
| `charset` | string | The character set of the input string, if it is not UTF-8. |

#### Examples

To encode a MIME-encoded string using the Quoted-Printable format, use:

```js
C.Encode.mime(_raw)
```

To encode a MIME-encoded string using the Base64 format, use:

```js
C.Encode.mime(_raw, `B`)
```

To encode a string using Quoted-Printable and apply line folding to ensure no encoded line exceeds 20 characters:

```js
C.Encode.mime('A much longer subject line with a non-ascii character: Español', 'Q', 20)
// Returns `=?UTF-8?Q?A_much_longer_subject_line_with_a_non-ascii_characte?= \n =?UTF-8?Q?r:_Espa=C3=B1ol?=`
```

### `C.Encode.uri()`

```js
C.Encode.uri(val: string): string
```

Returns the URI-encoded representation of the given string.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`val` | string | Value to encode. |
