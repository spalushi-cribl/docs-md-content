# Stream-to-Splunk Encryption

---

## Encrypt at Ingest-Time and Decrypt in Splunk

With Cribl Stream, you can [encrypt](securing-data-encryption) your sensitive data in real time before it's forwarded to and stored at a destination. Using the out-of-the-box [Mask](mask-function) function, you can define patterns to [encrypt](securing-data-encryption) with specific key IDs or key classes. To decrypt in Splunk, you will need to install [Cribl App for Splunk](https://cribl.io/download) on your search head. (The app will default to `mode-searchhead`.)

## Keys and Key Classes

Symmetric encryption [keys](securing-data-encryption#section-encryption-keys) can be configured through the CLI or the UI. They're used to encrypt the patterns, and users are free to define as many keys as required. 

[Key classes](securing-data-encryption#section-key-classes) are collections of keys that can be used to implement multiple levels of access control. Users (or groups of users) that have access to data with encrypted patterns can be associated with key classes. You can use these classes to provide more-granular access rights, such as read versus decryption permissions on a dataset.

## Encrypt in Cribl Stream and Decrypt in Splunk

1. Define one or more [keys](securing-data-encryption#section-encryption-keys) and [key classes](securing-data-encryption#section-key-classes) on Cribl Stream. (See [UI](securing-encryption-keys)- and [CLI](cli-keys)-based instructions.)

2. Sync `auth` with the decryption side (Splunk Search Head). (The Splunk-side directory is `$SPLUNK_HOME/etc/apps/cribl/local/cribl/auth/`.)

3. Apply the [Mask](mask-function) function to patterns of interest, using [C.Crypto.encrypt()](cribl-reference#ccrypto--data-encryption-and-decryption-functions).

4. Decrypt on the Splunk search head, using Role-Based Access Control on the `decrypt` command.

![Encrypting in Cribl Stream, decrypting in Splunk](encrypting-sensitive-data-3.0.7e800e74c4.png)

## Examples

### Encryption Side

You can generate keys via the UI or the CLI.

To generate keys via the UI, access **Group Settings** > **Security > Encryption Keys**:

![Adding a new encryption key](encryption-keys-4101.png)
{border="true"}

To generate one or more keys via the CLI, pattern your commands after these examples. 

In a single-instance deployment:

  `$CRIBL_HOME/bin/cribl keys add -c 1 -i`
  `...`
  `$CRIBL_HOME/bin/cribl keys add -c <N> -i`

  In a distributed deployment, to generate keys on a Worker Group named `uk`:

  `$CRIBL_HOME/bin/cribl keys add -c 1 -i -g uk`
  `...`
  `$CRIBL_HOME/bin/cribl keys add -c <N> -i -g uk`
  
  Add `-e <epoch>` to the above commands if you'd like to set expiration for your keys. 

> For all command/syntax options, see [Adding Keys](securing-data-encryption#adding-keys).
>
{.box .success}

### Decryption Side

* Download the Cribl Stream App for Splunk from Cribl's [Download Cribl Stream](https://cribl.io/download) page: In the **Select Release Version** dropdown, select the Cribl App for Splunk option.



* To install the Cribl Stream App for Splunk on your search head, untar the package into your `$SPLUNK_HOME/etc/apps` directory. The app will default to `mode-searchhead`. 

* Assign [permissions](https://docs.splunk.com/Documentation/Splunk/latest/Search/Controlaccesstothecustomcommand) to the `decrypt` command, per your requirements. 

* Assign [capabilities](https://docs.splunk.com/Documentation/Splunk/latest/Security/Rolesandcapabilities) to your Roles, per your requirements. Capability names should follow the format `cribl_keyclass_N`, where `N` is the Cribl Key Class. For example, a role with capability `cribl_keyclass_1` has access to all key IDs associated with key class `1`. You can use more capabilities, as long as they follow this naming convention.

![Selecting capabiities](encryption-capabilties.d3604f046d.png)
{border="true"}

* In the `$SPLUNK_HOME/etc/apps/cribl/local/cribl/auth/` directory, sync `cribl.secret`|`keys.json`. (To successfully decrypt data, the `decrypt` command will need access to the same keys that were used to encrypt, **in the Cribl instance where encryption happened**.)  

  - In a single-instance deployment, the `cribl.secret` and `keys.json` files reside in: `$CRIBL_HOME/local/cribl/auth/`.

  - In a distributed deployment, these files reside on the Leader Node in: `$CRIBL_HOME/groups/<group‑name>/local/cribl/auth/`.

  - When using Cribl Stream's UI, you can download these files by clicking **Get Key Bundle**.

  Sync/copy these files over to their counterparts on the search head (decryption side). In a non-Splunk integration, you would copy these assets to wherever decryption will take place.

> ##### Modifying Keys
>
> When you update keys by editing the `keys.json` file, you must add them back to the directories above (respectively, on a single instance or on a distributed deployment's Leader Node).
>
{.box .info}

### Usage

**Before Encryption**: Sample un-encrypted events. Notice the values of `fieldA` and `fieldB`.

![Events before encryption](unencrypted-data.28df8b3eb1.png)

Next, encrypt `fieldA` values with key class `1`, and `fieldB` with key class `2`.

![Encrypting two fields with separate key classes](encrypting-data.4f59e4a70b.png)

**After Encryption**: again, notice the values of `fieldA` and `fieldB`.

![Both fields encrypted](encrypted-data.1bc6dd39cd.png)

Here, we've decrypted `fieldB` but not `fieldA`. This is because the logged-in user has been assigned the capability `cribl_keyclass_2`, but not `cribl_keyclass_1`.

![One field decrypted](decrypted-data.e9bd9fe10a.png)
