# Encryption of Data in Motion


This page covers creating, modifying, and synchronizing symmetric keys.

Cribl Stream enables you to encrypt fields or patterns within events in real time, by using a `C.Crypto.encrypt()` [expression](cribl-reference#ccrypto--data-encryption-and-decryption-functions) in a [Mask Function](mask-function). The Mask Function accepts multiple replacement rules, and multiple fields to apply them to. 

In that Function's configuration, a **Match Regex** defines the pattern of content to be replaced. The **Replace Expression** is a JavaScript expression or literal to replace matched content. You can use the `C.Crypto.encrypt()` method here to generate an encrypted string from a passed value.

## Encryption Keys

You can configure symmetric keys through the CLI or UI. Users are free to define as many keys as required. Each key is characterized by the following: 

  * `keyId`: ID of the key.
  * `group`: Worker Group associated with the encryption key.
  * `algorithm`: Algorithm used with the key
  * `keyclass`: Cribl Key Class (below) that the key belongs to.
  * `kms`: Key management system for the key. Defaults to `local`. 
  * `created`: Time (epoch) when key was generated.
  * `expires`: Time (epoch) after which the key is invalid. Useful for key rotation.
  * `useIV`: Flag that indicates whether or not an initialization vector was used. 

## Key Classes 

Key Classes in Cribl Stream are collections of keys that can be used to implement multiple levels of access control. Users (or groups of users) with access to data with encrypted patterns can be associated with key classes, for even more granular, pattern-level compartmentalized access.

### Example

Users `U0, U1` have been given access to keyclass `0` which contains key IDs `0` and `1`. These keys are used to encrypt certain patterns in `datasetA`. Even though users `U0, U1, U2` have access to read this dataset, only `U0` and `U1` can decrypt its encrypted patterns. 

Key Class | Dataset
--- | ---
`keyclass: 0`<br/>Keys: `keyId: 0, keyId: 1`<br/>Users: `U0, U1` | `datasetA`<br/>Users: `U0, U1, U2`

User `U1` has been given access to an **additional** keyclass, `1`, which contains key IDs `11` and `22`. These keys are used to encrypt certain **other** patterns in `datasetA`. Even though users `U0, U1, U2` have access to read this dataset – same as above – only `U1` can decrypt the additional encrypted patterns. 

Key Class | Dataset
--- | ---
`keyclass: 1`<br/>Keys: `keyId: 11, keyId: 22`<br/>Users: `U1` | `datasetA`<br/>Users: `U0, U1, U2`

## Configure Keys

* **UI**: See how to [create and manage encryption keys](securing-encryption-keys) in the UI.
* **Command Line**: Use the `keys` [command](cli-keys).

## Key Storage

When using the `local` key management system, encryption keys in Cribl Stream are encrypted with `$CRIBL_HOME/local/cribl/auth/cribl.secret` and stored in `$CRIBL_HOME/local/cribl/auth/keys.json`. Cribl monitors the `keys.json` file for changes every 60 seconds.

> When installed as a Splunk app, `$CRIBL_HOME` is `$SPLUNK_HOME/etc/apps/cribl`.
>
{.box .info}
