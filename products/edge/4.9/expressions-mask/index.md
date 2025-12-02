# C.Mask – Data Masking Methods


## `C.Mask.CC()` {#cc}

```js
Mask.CC(value: string, unmasked: number = -4, maskChar: string='X'): string
```

Checks whether a value could be a valid credit card number, and masks a subset of the value. By default, all digits except the last 4 will be replaced with `X`.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The string to mask, **if and only if** it could be a valid credit card number.
|`unmasked` | number | Number of digits to leave unmasked: positive for left, negative for right, `0` for none.
|`maskChar` | string | String to replace each digit with.

### Examples

Let's assume your parsed data contains a `creditCard` field.
You can mask this number by using a [Mask](mask-function) Function.
Apply it to the `cardNumber` field and create the following masking rule:

|Match Regex|Replace Expression|Example result|
|---|---|---|
|`(.*)` (catches the whole field contents)|`${C.Mask.CC(g1)}`|`cardNumber: XXXXXXXXXX4860`|

To control how the masking is performed, you can specify how many digit should be unmasked
and what masking character to use:

|Match Regex|Replace Expression|Example result|
|---|---|---|
|`(.*)` (catches the whole field contents)|`${C.Mask.CC(g1, 4, "Z")}`|`cardNumber: 6011ZZZZZZZZZZZZ`|

## `C.Mask.crc32()`

```js
Mask.crc32(value: string): string
```

Generates a CRC32 hash of given value.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The string to hash.

## `C.Mask.IMEI()` {#imei}

```js
Mask.IMEI(value: string, unmasked: number = -4, maskChar: string='X'): string
```

Checks whether a value could be a valid IMEI number, and masks a subset of the value. By default, all digits except the last 4 will be replaced with `X`.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The string to mask, **if and only if** it could be a valid IMEI number.
|`unmasked` | number | Number of digits to leave unmasked: positive for left, negative for right, `0` for none.
|`maskChar` |string | String to replace each digit with.

### Examples

Let's assume your parsed data contains an `imei` field.
You can mask this number by using a [Mask](mask-function) Function.
Apply it to the `imei` field and create the following masking rule:

|Match Regex|Replace Expression|Example result|
|---|---|---|
|`(.*)` (catches the whole field contents)|`${C.Mask.IMEI(g1)}`|`imei: XXXXXXXXXXX7011`|

To control how the masking is performed, you can specify how many digit should be unmasked
and what masking character to use:

|Match Regex|Replace Expression|Example result|
|---|---|---|
|`(.*)` (catches the whole field contents)|`${C.Mask.CC(g1, 4, "M")}`|`cardNumber: 5356MMMMMMMMMMM`|

## `C.Mask.isCC()`

```js
Mask.isCC(value: string): boolean
```

Checks whether the given value could be a valid credit card number, by computing the string's Luhn's checksum modulo 10 == `0`.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | String to check for being a valid credit card number.

### Examples

The following example expression uses `isCC()` to check whether the `cardNumber` field
actually contains a credit card number.
If it does, the card number is replaced with the [`REDACTED`](#redacted) string,
otherwise, en empty string is returned.

```js
C.Mask.isCC(cardNumber) ? C.Mask.REDACTED : ""
```

## `C.Mask.isIMEI()`

```js
Mask.isIMEI(value: string): boolean
```

Checks whether the given value could be a valid IMEI number, by computing the string's Luhn's checksum modulo 10 == `0`.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | String to check for being a valid IMEI number.

### Examples

The following example expression uses `isCC()` to check whether the `imei` field
actually contains ab IMEI number number.
If it does, the IMEI number is replaced with the [`REDACTED`](#redacted) string,
otherwise, en empty string is returned.

```js
C.Mask.isIMEI(imei) ? C.Mask.REDACTED : ""
```

## `C.Mask.luhn()`

```js
Mask.luhn(value: string, unmasked: number = -4, maskChar: string='X'): string
```

Checks that value Luhn's checksum mod 10 is `0`, and masks a subset of the value. By default, all digits except the last 4 will be replaced with `X`. If the value's Luhn's checksum mod 10 is not `0`, then the value is returned unmodified.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The string to mask, **if and only if** the value's Luhn's checksum mod 10 is `0`.
|`unmasked` | number | Number of digits to leave unmasked: positive for left, negative for right, `0` for none.
|`maskChar` | string | String to replace each digit with.

## `C.Mask.luhnChecksum()`

```js
Mask.luhnChecksum(value: string, mod: number=10): number
```

Generates the Luhn checksum (used to validate certain credit card numbers, IMEIs, etc.). By default, the mod 10 of the checksum is returned. Pass mod = `0` to get the actual checksum.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | String whose digits you want to perform the Luhn checksum on.
|`mod` | number | Return checksum modulo this number. If `0`, skip modulo. Default is `10`.

## `C.Mask.md5()`

```js
Mask.md5(value: string, len?: string | number, encoding?: BufferEncoding): string
```

Generates an MD5 hash of a given value.

> `C.Mask.md5()` doesn't work when Cribl Stream is running in [FIPS mode](/stream/fips-mode#fips-mode-restrictions-on-cryptographic-algorithms).
{.box .warning}

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The string to hash.
|`len` | string \| number | Length of hash to return: 0 for full hash, a +number for left or a -number for right substring. If a string is passed it's length will be used.
`encoding` | string | Optional parameter that specifies the encoding of the value. Options include `base64`, `hex`, `utf-8`, and `binary`. The default is `utf-8`.

### Examples

To hash the contents of a `username` field with the MD5 algorithm, run:

```js
C.Mask.md5(username)
```

To hash the contents of an `authToken` field, where the value of the field is a base64-encoded string, with the MD5 algorithm, run:
```js
C.Mask.md5(authToken, undefined, 'base64')
```

## `C.Mask.random`

```js
Mask.random(len?: string | number): string
```

Generates a random alphanumeric string.

|Parameter|Type|Description|
|---|---|---|
|`len` |string \| number | Length of the result; or, if a string, use its length.

### Examples

Assuming you have some identifying user information in a `username` field,
you can replace it with a random string of the same length with:

```js
C.Mask.random(username)
```

|Input|Output|
|---|---|
|`McGoatFace`|`xY2KLHkyTg`|

## `C.Mask.repeat()`

```js
Mask.repeat(len?: number | string, char: string = 'X'): string
```

Generates a repeating char/string pattern, e.g., `XXXX`.

|Parameter|Type|Description|
|---|---|---|
|`len` | string \| number | Length of the result; or, if a string, use its length.
|`char` | string | Pattern to repeat `len` times.

### Examples

The following example will repeat the `GOAT` string as many time as there are characters in the first parameter, `goat`, so four times:

```js
C.Mask.repeat("goat", "GOAT")
```

## `C.Mask.sha1()`, `C.Mask.sha256()`, `C.Mask.sha512()`

```js
Mask.sha1(value: string, len?: string | number, encoding?: BufferEncoding): string
Mask.sha256(value: string, len?: string | number, encoding?: BufferEncoding): string
Mask.sha512(value: string, len?: string | number, encoding?: BufferEncoding): string
```

Generates an SHA1, SHA256, or SHA512 hash of given value.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | The string to hash.
`len` | string \| number | Length of hash to return: `0` for full hash, a +number for left, or a -number for right substring. If a string is passed, its length will be used.
`encoding` | string | Optional parameter that specifies the encoding of the value. Options include `base64`, `hex`, `utf-8`, and `binary`. The default is `utf-8`.

### Examples

To hash the contents of a `secret` field with the SHA256 algorithm, run:

```js
C.Mask.sha256(secret)
```

To hash the contents of an `authToken` field, where the value of the field is a base64-encoded string, with the SHA256 algorithm, run:

```js
C.Mask.sha256(authToken, undefined, 'base64')
```

## `C.Mask.REDACTED` {#redacted}

```js
Mask.REDACTED: string
```

The literal `'REDACTED'`.