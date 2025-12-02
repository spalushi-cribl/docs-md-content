# C.Mask – Data Masking Methods


The `C.Mask` expression provides a suite of methods for data masking, which is the process of obscuring sensitive information in your data. This ensures data privacy and maintains compliance with regulations like GDPR and HIPAA. It can protect personally identifiable information (PII) before sending it to a downstream Destination.

Use the methods in this expression to:

- **Redact sensitive information:** Replace data with a literal value like `REDACTED`. For example, replacing a credit card number like `1234-5678-9101-1121` to `REDACTED`.
- **Obfuscate data:** Replace values with a random or repeating character set. For example, obfuscating a username like `jdoe@example.com` by replacing it with a string of repeating characters, such as `XXXXXXXXXXXXXXX`.
- **Pseudonymize data:** Transform values into a non-identifiable, consistent value using a cryptographic hash. This technique allows you to perform analytics on the masked data without risking a privacy breach. For example, psudonymize a user ID like `user:1234567` into a consistent hash, such as `4b4f5d21c9a63c`. This makes it possible to analyze the data without revealing the original value.

By using `C.Mask` expressions, you can ensure that you handle sensitive data securely and that your Pipelines meet the necessary privacy and compliance standards.

For easier navigation, this page organizes the `C.Mask` methods by their purpose:

- **[Identity Validation and Masking](#identity-validation):** Methods for masking common identifiers like credit card and IMEI numbers, which also perform a basic validation check.
- **[Checksums and Validation](#checksums):** Methods that generate checksums and perform validation, which are the building blocks for more complex validation rules.
- **[Hashing](#hashing):** A collection of cryptographic hashing functions for pseudonymizing data while preserving its uniqueness for analytics.
- **[String Manipulation](#string):** General-purpose functions for creating random or repeating strings.

## Identity Validation and Masking {#identity-validation}

### `C.Mask.CC()` {#cc}

Checks whether a value could be a valid credit card number. If the value matches the respective format, it masks a subset of the value. By default, all digits except the last 4 will be replaced with `X`.

**Usage:**

```js
C.Mask.CC(value: string, unmasked: number = -4, maskChar: string='X'): string
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | The string to mask, **if and only if** it could be a valid credit card number. |
|`unmasked` | number | Number of digits to leave unmasked: positive for left, negative for right, `0` for none. |
|`maskChar` | string | String to replace each digit with. |

**Examples:**

Assuming your parsed data contains a `creditCard` field, you can mask this number by using a [Mask](mask-function) Function. Apply it to the `cardNumber` field and create the following masking rule:

| Match Regex | Replace Expression | Example result |
| ----------- | ------------------ | -------------- |
|`(.*)` (catches the whole field contents) | `${C.Mask.CC(g1)}` | `cardNumber: XXXXXXXXXX4860`|

To control how the masking is performed, you can specify how many digit should be unmasked
and what masking character to use:

| Match Regex | Replace Expression | Example result |
| ----------- | ------------------ | -------------- |
|`(.*)` (catches the whole field contents) | `${C.Mask.CC(g1, 4, "Z")}` | `cardNumber: 6011ZZZZZZZZZZZZ`|

### `C.Mask.IMEI()` {#imei}

Checks whether a value could be a valid IMEI number. If the value matches the respective format, it masks a subset of the value. By default, all digits except the last 4 will be replaced with `X`.

**Usage:**

```js
C.Mask.IMEI(value: string, unmasked: number = -4, maskChar: string='X'): string
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | The string to mask, **if and only if** it could be a valid IMEI number. |
|`unmasked` | number | Number of digits to leave unmasked: positive for left, negative for right, `0` for none. |
|`maskChar` |string | String to replace each digit with. |

**Examples:**

Assuming your parsed data contains an `imei` field, you can mask this number by using a [Mask](mask-function) Function. Apply it to the `imei` field and create the following masking rule:

| Match Regex | Replace Expression | Example result |
| ----------- | ------------------ | -------------- |
|`(.*)` (catches the whole field contents) | `${C.Mask.IMEI(g1)}` | `imei: XXXXXXXXXXX7011`|

To control how the masking is performed, you can specify how many digit should be unmasked and what masking character to use:

| Match Regex | Replace Expression | Example result |
| --- | --- | --- |
| `(.*)` (catches the whole field contents) |`${C.Mask.IMEI(g1, 4, "M")}` | `cardNumber: 5356MMMMMMMMMMM` |

### `C.Mask.isCC()`

Checks whether the given value could be a valid credit card number, by computing the string's Luhn's checksum modulo 10 == `0`.

**Usage:**

```js
C.Mask.isCC(value: string): boolean
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | String to check for being a valid credit card number. |

**Examples:**

The following example expression uses `isCC()` to check whether the `cardNumber` field actually contains a credit card number.
If it does, the card number is replaced with the [`REDACTED`](#redacted) string, otherwise, en empty string is returned.

```js
C.Mask.isCC(cardNumber) ? C.Mask.REDACTED : ""
```

### `C.Mask.isIMEI()`

Checks whether the given value could be a valid IMEI number, by computing the string's Luhn's checksum modulo 10 == `0`.

**Usage:**

```js
C.Mask.isIMEI(value: string): boolean
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | String to check for being a valid IMEI number. |

**Examples:**

The following example expression uses `isIMEI()` to check whether the `imei` field actually contains an IMEI number number.
If it does, the IMEI number is replaced with the [`REDACTED`](#redacted) string, otherwise, en empty string is returned.

```js
C.Mask.isIMEI(imei) ? C.Mask.REDACTED : ""
```

### `C.Mask.REDACTED` {#redacted}

`C.Mask.REDACTED` is a literal string (not a function) that evaluates to `REDACTED`. It's used to completely remove sensitive data by replacing it with a clear, non-identifiable placeholder.

**Usage:**

```js
C.Mask.REDACTED: string
```

**Examples:**

This example demonstrates using `C.Mask.REDACTED` within a conditional expression. It checks if the `cardNumber` field is a valid credit card number. If it is, the value is replaced with `REDACTED`. Otherwise, it keeps the original value.

```js
C.Mask.isCC(cardNumber) ? C.Mask.REDACTED : cardNumber
```

## Checksums and Validation {#checksums}

### `C.Mask.luhn()`

Masks a value only if its checksum is valid. This is useful for sanitizing data and ensuring that only correctly formatted numbers are sent downstream.

It checks that value's Luhn checksum mod 10 is `0`, and masks a subset of the value. By default, all digits except the last 4 will be replaced with `X`. If the value's Luhn's checksum mod 10 is not `0`, then the value is returned unmodified.

**Usage:**

```js
C.Mask.luhn(value: string, unmasked: number = -4, maskChar: string='X'): string
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | The string to mask, **if and only if** the value's Luhn checksum mod 10 is `0`. |
|`unmasked` | number | Number of digits to leave unmasked: positive for left, negative for right, `0` for none. |
|`maskChar` | string | String to replace each digit with. |

**Examples:**

| Expression | Input Value | Output |
| ---------- | ----------- | ------ |
| `C.Mask.luhn(value)` | `49927398716` (a valid Luhn number) | `XXXXXXX8716` |
| `C.Mask.luhn(value)` | `49927398717` (an invalid Luhn number) |	`49927398717` |
| `C.Mask.luhn(value, 4, "Y")` | `49927398716` (a valid Luhn number) | `4992YYYYYYY` |

### `C.Mask.luhnChecksum()`

Generates a checksum that can be used to validate a number. This is useful for building your own custom validation rules or for debugging.

Generates the Luhn checksum (used to validate certain credit card numbers, IMEIs, and more). By default, the mod 10 of the checksum is returned. Pass mod = `0` to get the actual checksum.

**Usage:**

```js
C.Mask.luhnChecksum(value: string, mod: number=10): number
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | String whose digits you want to perform the Luhn checksum on. |
|`mod` | number | Return checksum modulo for this number. If `0`, skip modulo. Default is `10`. |

**Examples:**

This example demonstrates how to get the default `mod 10` checksum. This expression returns the last digit of the checksum, which is commonly used to validate a number. A valid Luhn number will return `0`.

```js
// This will return the number 0
C.Mask.luhnChecksum("49927398716")
```

This example demonstrates how to get the full checksum. You can get the full checksum by setting `mod` to `0`.

```js
// This will return the number 70, which is the full checksum
C.Mask.luhnChecksum("49927398716", 0)
```

## Hashing {#hashing}

### `C.Mask.crc32()`

Generates a CRC32 hash of given value.

**Usage:**

```js
C.Mask.crc32(value: string): string
```

**Parameter:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | The string to hash. |

**Examples:**

| Expression | Input Value | Output |
| ---------- | ----------- | ------ |
| `C.Mask.crc32(value)` | `example_log_123` | `cf0e3d3a` |
| `C.Mask.crc32(host + file_path)` | `webserver01/var/log/app.log` | `b9b661b6` |

### `C.Mask.md5()`

Generates an MD5 hash of a given value.

> `C.Mask.md5()` doesn't work when Cribl Stream is running in [FIPS mode](/stream/fips-mode#fips-mode-restrictions-on-cryptographic-algorithms).
>
{.box .warning}

**Usage:**

```js
C.Mask.md5(value: string, len?: string | number, encoding?: BufferEncoding): string
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | The string to hash. |
|`len` | string or number | Length of hash to return: `0` for full hash, a +number for left or a -number for right substring. If a string is passed, its length will be used. |
|`encoding` | string | Optional parameter that specifies the encoding of the value. Options include `base64`, `hex`, `utf-8`, and `binary`. The default is `utf-8`. |

**Examples:**

To hash the contents of a `username` field with the MD5 algorithm, run:

```js
C.Mask.md5(username)
```

To hash the contents of an `authToken` field, where the value of the field is a base64-encoded string, with the MD5 algorithm, run:

```js
C.Mask.md5(authToken, undefined, 'base64')
```

### C.Mask.sha1(), C.Mask.sha256(), C.Mask.sha512(), C.Mask.sha3_256(), C.Mask.sha3_512()

Generates a hash of a given value. Supported algorithms:

- SHA1
- SHA256
- SHA512
- SHA3-256
- SHA3-512

**Usage:**

```js
C.Mask.sha1(value: string, len?: string | number, encoding?: BufferEncoding): string
C.Mask.sha256(value: string, len?: string | number, encoding?: BufferEncoding): string
C.Mask.sha512(value: string, len?: string | number, encoding?: BufferEncoding): string
C.Mask.sha3_256(value: string, len?: string | number, encoding?: BufferEncoding): string
C.Mask.sha3_512(value: string, len?: string | number, encoding?: BufferEncoding): string
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`value` | string | The string to hash. |
|`len` | string or number | Length of hash to return: `0` for full hash, a +number for left, or a -number for right substring. If a string is passed, its length will be used. |
|`encoding` | string | Optional parameter that specifies the encoding of the value. Options include `base64`, `hex`, `utf-8`, and `binary`. The default is `utf-8`. |

**Examples:**

To hash the contents of a `secret` field with the SHA256 algorithm, run:

```js
C.Mask.sha256(secret)
```

To hash the contents of an `authToken` field, where the value of the field is a base64-encoded string, with the SHA256 algorithm, run:

```js
C.Mask.sha256(authToken, undefined, 'base64')
```

## String Manipulation {#string}

### `C.Mask.random`

Generates a random alphanumeric string.

**Usage:**

```js
C.Mask.random(len?: string | number): string
```

**Parameters:**

| Parameter | Type | Description |
| --------- | ---- | ----------- |
|`len` |string or number | Length of the result; or, if a string, use its length. |

**Examples:**

Assuming you have some identifying user information in a `username` field,
you can replace it with a random string of the same length with:

```js
C.Mask.random(username)
```

| Input | Output |
| ----- | ------ |
|`McGoatFace`|`xY2KLHkyTg`|

### `C.Mask.repeat()`

Generates a repeating character or string pattern, such as `XXXX`.

**Usage:**

```js
C.Mask.repeat(len?: number | string, char: string = 'X'): string
```

**Parameters:**

| Parameter | Type | Description |
| --- | ---- | --- |
|`len` | string or number | Length of the result; or, if a string, use its length. |
|`char` | string | Pattern to repeat `len` times. |

**Examples:**

The following example will repeat the `GOAT` string as many time as there are characters in the first parameter, `goat`, so four times:

```js
C.Mask.repeat("goat", "GOAT")
```
