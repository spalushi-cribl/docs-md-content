# Create and Manage Encryption Keys


You can create and manage keys that Cribl Stream will use for real-time encryption of fields and patterns within events. For details on applying the keys that you define here, see [Encryption Keys](/stream/securing-data-encryption#encryption-keys).

> In Cribl Stream v.4.1 and newer, for enhanced security, Cribl encrypts TLS certificate private keys in configuration files when you add or modify them. Before upgrading to v.4.1.0 (or newer), back up your existing unencrypted key files – see [Safeguarding Unencrypted Private Keys for Rollback](/stream/upgrading#safeguarding). If you need to roll back to a pre-4.1 version, see [Restore Unencrypted Private Keys](#restoring) below.
>
{.box .warning}

## Access Keys {#keys-access}

- In a Single-instance deployment, select **Settings** > **Security > Encryption Keys**.
- In a Distributed deployment with one Worker Group, select **Settings > Security > Encryption Keys**.
- In a Distributed deployment with multiple Worker Groups, keys are managed per Worker Group. Select **Manage** > **Groups** > `<group-name>` **Group Settings > Security > Encryption Keys**.

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

## Restore Unencrypted Private Keys {#restoring}

If you need to roll back from Cribl Stream v.4.1 or later to an earlier version, follow this procedure to restore the unencrypted private key files that you earlier [backed up](/stream/upgrading#safeguarding).

1. Stop Cribl Stream v.4.1 (or later) on hosts – the Leader and all Worker Nodes.
2. Untar the older Cribl Stream version (v.4.0.4, for example) onto the Leader and all Worker Nodes.
3. Manually edit each Cribl Stream config file that contains an encrypted private key. Replace each key with its prior unencrypted version.
4. Start up Cribl Stream, commit and deploy, and resume normal operations.
