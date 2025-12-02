# encrypt


The `encrypt` function encrypts data with a key managed by a Cribl Stream Worker Group.

For more information on how to set up encryption keys, see the Cribl Stream docs:

- [Encryption of Data in Motion](/stream/securing-data-encryption)
- [Create and Manage Encryption Keys](/stream/securing-encryption-keys)
- [C.Crypto – Data Encryption and Decryption](/stream/expressions-crypto)

## Syntax

`encrypt(value, workerGroup, keyId)`

### Parameters

| Name          | Type               | Required | Description                                                      |
| ------------- | ------------------ | -------- | ---------------------------------------------------------------- |
| `value`       | [`string`](string) | Yes      | A valid KQL expression. This is the data to encrypt.             |
| `workerGroup` | [`string`](string) | Yes      | The name of the Stream Worker Group that has the encryption key. |
| `keyId`       | [`string`](string) | Yes      | The ID of the encryption key.                                    |

## Returns

Returns the input data, encrypted using the specified key from the specified Stream Worker Group, encoded as a Base64
string.

## Permissions

You need access to the Stream Worker Group that contains the encryption key.

## Example

Encrypt a specific field (`dstport`), and export it to a Cribl Lake Dataset.

```kusto
dataset="cribl_search_sample"
 | limit 1000
 | extend dstport = encrypt(dstport, <workerGroup>, <keyId>)
 | export to lake myLakeDataset
```
