# Encryption


## Encryption of Data in Motion

With Cribl Stream, you can encrypt fields or patterns within events in real time, by using `C.Crypto.encrypt()` in a [Mask](mask-function) function. The Mask function accepts multiple replacement rules and multiple fields to apply them to. 

A **Match regex** defines the pattern of content to be replaced. The **Replace expression** is a JS expression or literal to replace matched content. The `C.Crypto.encrypt()` method can be used here to generate an encrypted string from a value passed to it.

> ##### C.Crypto.encrypt() Syntax
>
> (method) `Crypto.encrypt(value: any, keyclass: number, keyId?: string, defaultVal?: string): string`
> Encrypt the given value with the `keyId`, or with a `keyId` picked up automatically based on `keyclass`.
> 
> @param {string | Buffer} `value` – what to encrypt.
> @param – `keyclass` – if `keyId` isn't specified, pick one at the given `keyclass`.
> @param – `keyId` - encryption keyId, takes precedence over `keyclass`.
> @param – `defaultVal` – what to return if encryption fails for any reason; if unspecified, the original value is returned.
> @returns – if encryption succeeds, the encrypted value; otherwise, `defaultVal` if specified; otherwise, `value`.
>
{.box .info}

## Encryption Keys

Symmetric keys can be configured through the CLI or UI. Users are free to define as many keys as required. Each key is characterized by the following: 

  * `keyId`: ID of the key.
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

##  Configuring Keys with the CLI  {#cli}

When using the `local` key management system, encryption keys in Cribl Stream are encrypted with `$CRIBL_HOME/local/cribl/auth/cribl.secret` and stored in `$CRIBL_HOME/local/cribl/auth/keys.json`. Cribl monitors the `keys.json` file for changes every 60 seconds.

> When installed as a Splunk app, `$CRIBL_HOME` is `$SPLUNK_HOME/etc/apps/cribl`.
>
{.box .info}

### Listing Keys

Keys are added and listed using the `keys` [command](cli-reference#keys):

`$CRIBL_HOME/bin/cribl keys list -g <workerGroupID>`

``` {title="Sample Command Output"}
keyId  algorithm    keyclass  kms    created         expires     useIV
-----------------------------------------------------------------------
1      aes-256-cbc  0         local  1544906269.316  0           false
2      aes-256-cbc  1         local  1544906272.452  0           false
3      aes-256-cbc  2         local  1544906275.948  1545906275  true
4      aes-256-cbc  3         local  1544906278.026  0           false
```

### Adding Keys

Displaying `--help`:

`$CRIBL_HOME/bin/cribl keys add --help`

``` {title="Sample Command Output"}
Add encryption keys
Usage: [options] [args]

Options:
[-c <keyclass>] - key class to set for the key
[-k <kms>]      - KMS to use, must be configured, see cribl.yml
[-e <expires>]  - expiration time, epoch time
[-i]            - use an initialization vector
 -g <group>     - Group ID
```

Adding a key to keyclass `1`, with no expiration date, on the `default` Worker Group:

`$CRIBL_HOME/bin/cribl keys add -c 1 -i -g default`

``` {title="Sample Command Output"}
Adding key: success. Key count=1
```

(You would use the same syntax to reference a non-`default` Worker Group by its name.)
?
Listing keys to verify key generation:

`$CRIBL_HOME/bin/cribl keys list -g default`

``` {title="Sample Command Output"}
keyId  algorithm    keyclass  kms    created         expires     useIV
-----------------------------------------------------------------------
1      aes-256-cbc  1         local  1545243364.342  0           true
```

## Configuring Keys with the UI {#configuring-keys-with-ui}

In a single-instance deployment, you can access the key management interface through **Settings** > **Security > Encryption Keys**. In a distributed deployment, for each Group, select **Manage** > `<group‑name>` > **Group Settings** > **Security** > **Encryption Keys**.

Here, you can list and add new keys. To protect against accidental changes, a key's parameters, once saved, can be edited only through [configuration files](configuration-files). 

![List or create keys through Cribl Stream's UI](encryption-keys-2.611e211d7c.png)
{border="true"}



##  Sync `auth/(cribl.secret|keys.json)`  {#sync}

> Cribl Stream performs decryption only when the data is being sent to Splunk. For details, see [Decryption of Data in Splunk](/stream/securing-data-decryption)
{.box .success}

To successfully decrypt data, the `decrypt` command will need access to the same keys that were used to encrypt, **in the Cribl instance where encryption happened**. 

- In a single-instance deployment, the `cribl.secret` and `keys.json` files reside in: `$CRIBL_HOME/local/cribl/auth/`.

- In a distributed deployment, the `cribl.secret` and `keys.json` files reside on the Leader Node in: `$CRIBL_HOME/groups/<group‑name>/local/cribl/auth/`.

- When using the UI, you can download these files by clicking the **Get Key Bundle **button.

Sync/copy these files over to their counterparts on the Search Head/decrypting side, residing in: `$SPLUNK_HOME/etc/apps/cribl/local/cribl/auth/`.

###  Modifying Keys  {#modify}

When you update keys by editing the `keys.json` file, you must add them back to the directories above (respectively, on a single instance or on a distributed deployment's Leader Node).
