# Decryption of Data in Splunk


Currently, Cribl Stream supports decryption only when Splunk is the end system. In Splunk, decryption is available to users of any role with permissions to run the `cribldecrypt` command that ships with [Cribl App for Splunk](https://cribl.io/download). Further restrictions can be applied with Splunk **capabilities**. This page provides details.

For clarity, Cribl introduced the `cribldecrypt` as an alias to the original`decrypt` command. Use this alias to avoid conflicts with Splunk's internal commands. (We show it in the examples below.) Both are, in fact, aliases to the actual command: `/path/2/cribl ‑‑splunk‑decrypt`. You can use both aliases. 


## Usage

The `cribldecrypt` command is used to display Cribl-encrypted fields in cleartext. It is an alias to the `decrypt` command. This command decrypts fields only for the encryption keys that the user can access. 

The example below retrieves data from a Splunk index with Cribl-encrypted data, and pipes it to the `cribldecrypt` command: 

```shell
index=index_with_encrypted_fields | cribldecrypt
```

## Decrypting in Splunk 

Decryption in Splunk is implemented via a custom command called `cribldecrypt`. To use the command, users must belong to a Splunk [role](https://docs.splunk.com/Documentation/Splunk/latest/Admin/Aboutusersandroles) that has permissions to execute it. [Capabilities](https://docs.splunk.com/Documentation/Splunk/latest/Security/Rolesandcapabilities), which are aligned to [Cribl Key Classes](securing-data-encryption#section-key-classes), can be associated with a particular role to further control the scope of `cribldecrypt`. 

> ##### Decrypt Command Is Search Head ONLY
>
> To ensure that keys don't get distributed to all search peers – including peers that your search head can search, but you don't have full control over – `cribldecrypt` is scoped to run locally on the installed search head.
>
{.box .info}

## Restricting Access with Splunk Capabilities 

In Splunk, capability names should follow the format `cribl_keyclass_N`, where `N` is the Cribl Key Class. For example, a role with capability `cribl_keyclass_1` has access to all key IDs associated with key class `1`.   

Capability Name | Corresponding Cribl Key Class
--- | ---
`cribl_keyclass_1`<br/>`cribl_keyclass_2`<br/>...<br/>`cribl_keyclass_N` | `1`<br/>`2`<br/>...<br/>`N`

## Configuring Splunk Search Head to Decrypt Data

You set up decryption in Splunk according to this schematic:

![](cribl-encryption-marchitecture.b369cc033a.png)

1. Download the Cribl Stream App for Splunk from Cribl's [Download Cribl Stream](https://cribl.io/download) page: In the **Select Release Version** dropdown, select the Cribl App for Splunk option.




2. To install the Cribl Stream App for Splunk on your search head, untar the package into your `$SPLUNK_HOME/etc/apps` directory. <br/>
  The app will run in search head mode by default. If the app has previously been installed and later modified, you can convert it to search head mode with the command: `$CRIBL_HOME/bin/cribld mode-searchhead`. (When installed as a Splunk app, `$CRIBL_HOME` is `$SPLUNK_HOME/etc/apps/cribl`.)

3. Assign [permissions](https://docs.splunk.com/Documentation/Splunk/latest/Search/Controlaccesstothecustomcommand) to the `cribldecrypt` command, per your requirements. 

4. Assign [capabilities](https://docs.splunk.com/Documentation/Splunk/latest/Security/Rolesandcapabilities) to your roles, per your requirements. If you'd like to create more capabilities, ensure that they follow the naming convention defined above.

5. In the `$SPLUNK_HOME/etc/apps/cribl/local/cribl/auth/` directory, sync `cribl.secret`|`keys.json`. (To successfully decrypt data, the `cribldecrypt` command will need access to the same keys that were used to encrypt, **in the Cribl instance where encryption happened**.) 

     - In a single-instance deployment, the `cribl.secret` and `keys.json` files reside in: `$CRIBL_HOME/local/cribl/auth/`.

     - In a distributed deployment, these files reside on the Leader Node in: `$CRIBL_HOME/groups/<group‑name>/local/cribl/auth/`.

     - When using Cribl Stream's UI, you can download these files by clicking the **Get Key Bundle **button.

  Sync/copy these files over to their counterparts on the search head (decryption side). In a non-Splunk integration, you would copy these assets to wherever decryption will take place.

> ##### Modifying Keys
>
> When you update keys by editing the `keys.json` file, you must add them back to the directories above (respectively, on a single instance or on a distributed deployment's Leader Node).
>
{.box .info}
