# Create and Manage Encryption Keys


You can create and manage keys that Cribl Stream will use for real-time encryption of fields and patterns within events. For details on applying the keys that you define here, see [Encryption Keys](/stream/securing-data-encryption#encryption-keys).

> After setting up a key, you can also reference it in Cribl Search,
> using the [`encrypt`](/search/encrypt) and [`decrypt`](/search/decrypt) functions in your queries.
{.box .success}

## Access Keys {#keys-access}

- In a Single-instance deployment, select **Settings** > **Security > Encryption Keys**.
- In a Distributed deployment with one Worker Group, select **Settings > Security > Encryption Keys**.
- In a Distributed deployment with multiple Worker Groups, keys are managed per Worker Group. Select **Manage** > **Groups** > `<groupName>` **Group Settings > Security > Encryption Keys**.

On the resulting **Manage Encryption Keys** page, you can configure existing keys, and/or use the following options to add new keys.

## Get Key Bundle {#keys-import}

To import existing keys, click **Get Key Bundle**. You'll be prompted to supply a login and password to proceed.

## Add New Key {#keys-add}

To define a new key, click **New Key**. The resulting **New Key** modal provides the following controls:

**Key ID**: Cribl Stream will automatically generate this unique identifier.

**Description**: Optionally, enter a description summarizing the purpose of the key.

**Encryption algorithm**: Currently, Cribl Stream supports the `aes-256-cbc` (default) and `aes-256-gcm` algorithms.

**KMS for this key**: Currently, the only option supported here is `local` (the [Key Management Service](securing-kms-config) built into Cribl Stream).

**Key class**: Classes are arbitrary collections of keys that you can map to different levels of access control. For details, see [Key Classes](/stream/securing-data-encryption#key-classes). This value defaults to `0`; you can assign more classes, as needed. 

**Expiration time**: Optionally, assign the key an expiration date. Directly enter the date or select it from the date picker.

**Use initialization vector**: If enabled, Cribl Stream will seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Optional (and defaults to disabled) with the `aes‑256‑cbc` algorithm; automatically enabled (and cannot be disabled) with the `aes‑256‑gcm` algorithm.

**Initialization vector size**: Length of the initialization vector (IV), in bytes. This option is displayed only with the `aes‑256‑gcm` algorithm. Defaults to `12` bytes to optimize interoperability, but you can use the drop-down to set this anywhere between `12` to `16` bytes.

## Modify Keys {#modify}

To protect against accidental changes, a key's parameters – once saved – can be edited only through [configuration files](configuration-files).

When you update keys by editing the `keys.json` file, you must add them back to the directories above (respectively, on a single instance or on a Distributed deployment's Leader Node).

## Sync `auth/(cribl.secret|keys.json)` {#sync}

> Cribl Stream performs decryption only when the data is being sent to Splunk. For details, see [Decryption of Data in Splunk](/stream/securing-data-decryption)
{.box .success}

To successfully decrypt data, the `decrypt` command will need access to the same keys that were used to encrypt, **in the Cribl instance where encryption happened**. 

- In a Single-instance deployment, the `cribl.secret` and `keys.json` files reside in: `$CRIBL_HOME/local/cribl/auth/`.

- In a Distributed deployment, the `cribl.secret` and `keys.json` files reside on the Leader Node in: `$CRIBL_HOME/groups/<group‑name>/local/cribl/auth/`.

- When using the UI, you can download these files by selecting **Get Key Bundle**.

Sync/copy these files over to their counterparts on the Search Head/decrypting side, residing in: `$SPLUNK_HOME/etc/apps/cribl/local/cribl/auth/`. 