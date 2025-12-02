# C.Crypto – Data Encryption and Decryption


## `C.Crypto.decrypt()`

```js
C.Crypto.decrypt(value: string, escape: boolean = false, escapeSeq = '"'): string
```

Decrypts all occurrences of ciphers in the given value. Instances that cannot be decrypted (for any reason) are left intact.

Returns `value` with ciphers decrypted.

|Parameter|Type|Description|
|---|---|---|
|`value` | string | String in which to look for ciphers. |
|`escape` | boolean | Set to `true` to escape double quotes in output after decryption (for example, for data encrypted in Splunk.) Defaults to `false`. |
|`escapeSeq` | string | String used to escape double quotes. The default `'"'` escapes CSV output. |

### Examples

To decrypt the contents of the `encrypted` field, use:

```js
C.Crypto.decrypt(encrypted)
```

## `C.Crypto.encrypt()`

```js
C.Crypto.encrypt(value: Buffer | string, keyclass: number, keyId?: string, defaultVal?: string): string
```

Encrypts the given value with the `keyId`, or with a `keyId` picked up automatically based on `keyclass`.

Returns the encrypted value. If encryption does not succeed, returns `defaultVal` if specified; otherwise, `value`.

|Parameter|Type|Description|
|---|---|---|
| `value` | string \| Buffer | The value to encrypt. |
| `keyclass` | number | [Key Class](/stream/securing-data-encryption#key-classes) to pick a key from, if `keyId` isn't specified. |
| `keyId` | string | Encryption [`keyId`](/stream/securing-data-encryption#encryption-keys), takes precedence over `keyclass`. |
| `defaultVal` | string | Value to return if encryption fails; if unspecified, the original value is returned. |

> You can decrypt the encrypted data in Cribl Search,
> using the [`encrypt`](/search/encrypt) and [`decrypt`](/search/decrypt) functions in your queries.
{.box .success}

### Examples

To encrypt the `customer` field using key with `keyId` 2, use:

```js
C.Crypto.encrypt(customer, -1, 2)
```

To encrypt the same field with a key selected from the Key Class 3, without specifying a `keyId`:

```js
C.Crypto.encrypt(customer, 3)
```

## `C.Crypto.createHmac()`

```js
C.Crypto.createHmac(value: string | Buffer, secret: string, algorithm: string = 'sha256', outputFormat: 'base64' | 'hex' | 'latin1' = 'hex'): string
```

Generates an [HMAC](https://en.wikipedia.org/wiki/HMAC) that can be added to events, or can be used to validate events that contain an HMAC.

Returns the calculated HMAC digest on success; otherwise, `value`.

|Parameter|Type|Description|
|---|---|---|
|`value` | string \| Buffer | The data to encrypt. When the `outputFormat` is invalid or undefined, this parameter is returned as the digest, via a Buffer.
|`secret` |string | The secret key used to generate the MAC.
|`algorithm` | string | The hash algorithm used to generate the MAC. Defaults to `'sha256'`. Run `openssl list -digest-algorithms` to see the list of available algorithms.
|`outputFormat` | `'base64'` \| `'hex'` \| `'latin1'` | Output format for the MAC. Defaults to `'hex'`.

### Examples

To generate HMAC from the `customer` field, provide the secret as the second argument:

``` js
C.Crypto.createHmac(customer, 'kETvYtLKZkgIzBvfRdVcZULBITANjOMd')
```

To additionally specify that you want to use the SHA512 hash algorithm and to output the HMAC in `latin1` format, run:

```js
C.Crypto.createHmac(customer, 'kETvYtLKZkgIzBvfRdVcZULBITANjOMd', 'sha512', 'latin1')
```