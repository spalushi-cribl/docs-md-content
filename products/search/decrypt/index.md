# decrypt


The `decrypt` function decrypts data with a key managed by a Cribl Stream Worker Group.

For more information on how to set up encryption keys, see the Cribl Stream docs:

- [Encryption of Data in Motion](/stream/securing-data-encryption)
- [Create and Manage Encryption Keys](/stream/securing-encryption-keys)
- [C.Crypto – Data Encryption and Decryption](/stream/expressions-crypto)

## Syntax

`decrypt(value, workerGroup)`

### Parameters

| Name          | Type               | Required | Description                                                                                    |
| ------------- | ------------------ | -------- | ---------------------------------------------------------------------------------------------- |
| `value`       | [`string`](string) | Yes      | A valid KQL expression, containing data encrypted with a key from the specified `workerGroup`. |
| `workerGroup` | [`string`](string) | Yes      | The name of the Stream Worker Group that has the encryption key.                               |

## Returns

Returns the input data, decrypted using the key from the specified Stream Worker Group.

## Permissions

You need access to the Stream Worker Group that contains the encryption key.

## Examples

Get the results of a past search (with ID `1704236905683.wgocax`), and decrypt a specific field (`dstport`).

```kusto
dataset="$vt_results" jobId='1704236905683.wgocax'
 | extend dstport = decrypt(dstport, <workerGroup>)
```
