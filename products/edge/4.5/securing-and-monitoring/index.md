# Securing Cribl Edge (TLS/SSL, Certs, Keys)

 
You can secure Cribl Edge access and traffic using various combinations of SSL (Secure Sockets Layer), TLS (Transport Layer Security), custom HTTP headers, and internal or external [KMS](kms-config) (Key Management Service) options. An additional option for Cribl Stream is to [deploy in FIPS mode](/stream/fips-mode).

> In a single-instance deployment ([Stream](/stream/deploy-single-instance), [Edge](/edge/deploy-single-instance)), wherever this page refers to a Fleet, just follow the Cribl Edge top nav's **Manage** link.
>
{.box .success}

## Secure Access to Edge Nodes' UI {#ui-access}

A best practice in enterprise distributed deployments, this prevents direct browser access to Edge Nodes' UI.

> Cribl recommends that you first enable the Leader's UI access to Edge Nodes ([Stream](/stream/deploy-distributed#worker-access), [Edge](/edge/setting-up-leader-and-edge-nodes#edge-access)). This way, admins will still be able to tunnel through from the Leader to any Edge Node's UI. This is also a prerequisite for [Connecting Workers to the Leader Securely](securing-communications#connect-workers).
>
{.box .warning}

1. Select a Fleet.
2. Open **Fleet Settings** (top right).
3. Under **General Settings** > **API Server Settings**, click **Advanced**.
4. Toggle **Local UI Access** to `No`.
5. Click **Save**.

## SSL Certificate Configuration {#ssl}

You can secure Cribl Edge's API and UI access by configuring SSL. Do this on the Leader, to secure Edge Nodes' inbound communications.

You can use your own certs and private keys, or you can generate a pair with [OpenSSL](https://www.openssl.org/), as shown here:

`openssl req -nodes -new -x509 -newkey rsa:2048 -keyout myKey.pem -out myCert.pem -days 420`

This example command will generate both a self-signed cert `myCert.pem` (certified for 420 days), and an unencrypted, 2048-bit RSA private key `myKey.pem`. (Change the filename arguments to modify these placeholder names.) 

> As indicated by these examples, Cribl Edge expects certificates and keys to be formatted in privacy-enhanced mail (`.pem`) format.
>
{.box .info}

In the Cribl Edge UI, you can configure the cert via **Settings** > **Global Settings** > **Security > Certificates**. You can configure the **key** via: 

- **Settings** > **Global Settings** > **Security > Encryption Keys** single-instance deployments ([Edge](/edge/deploy-single-instance), [Stream](/stream/deploy-single-instance)), or
- **Manage** > **Groups** > `<group‑name>` > **Group Settings** > **Security** > **Encryption Keys** distributed deployments ([Edge](/edge/setting-up-leader-and-edge-nodes),[Stream](/stream/deploy-distributed)).

Alternatively, you can edit the `local/cribl.yml` file's `api` section to directly set the `privKeyPath` and `certPath` attributes. For example:

```yaml {title="cribl.yml"}
api:
  host: 0.0.0.0
  port: 9000
  disabled : false
  ssl:
    disabled: false
    privKeyPath: /path/to/myKey.pem
    certPath: /path/to/myCert.pem
...
```

> See [Securing Communications](securing-communications) for details about using this certificate and key to secure communications on, and among, your Cribl Edge Leader and Edge Nodes.
>
{.box .success}

## Custom HTTP Headers {#http}

You can encode custom, security-related HTTP headers, as needed. As shown in the examples below, you specify these at **Settings** > **Global Settings** > **General Settings** > **API Server Settings** > **Advanced** > **HTTP Headers**. Click **Add Header** to display extra rows for new key-value pairs.

![Custom HTTP headers](http-custom-headers.76873e112d.png)
{border="true"}

## TLS Settings and Traffic Types {#tls}

This table shows TLS client/server pairs, and encryption defaults, per traffic type.

Traffic Type | TLS Client | TLS Server | Encryption | Cert Auth | CN* Check
--- | --- | --- | --- | --- | ---
UI | Browser | Cribl Edge | Default disabled | Default disabled | Default disabled
API | Worker/Edge Node| Leader | Default disabled | Default disabled | Default disabled
Worker-to-Leader | Worker/Edge Node | Leader | Default disabled | Default disabled | Default disabled
Data | Any data sender | Cribl Edge (Source) | Default disabled | Default disabled | Default disabled
Data | Cribl Edge (Destination) | Any data receiver | Default disabled | Default disabled | Default disabled
**Authentication**  | ———— | ———— | ———— | ———— | ————
Local* | Browser | Cribl Edge| Default Disabled | N/A | N/A
LDAP* | Cribl Edge | LDAP Provider | Custom | N/A | Default Disabled
Splunk* | Cribl Edge | Splunk Search Head | Default Enabled | N/A | Default Disabled
OIDC†/​Okta* | Browser and Cribl Edge | Okta | Default Enabled | N/A | Enabled (Browser)
OIDC†/​Google* | Browser and Cribl Edge | Google | Default Enabled | N/A | Enabled (Browser)

_\* Common name_  
_† OpenID Connect_

## Default TLS Settings (Cyphers, Etc.) {#cyphers}

You can configure advanced, system-wide TLS settings – minimum and maximum TLS versions, default cypher lists, and ECDH curve names. Select **Settings** > **Global Settings** > **System > General Settings > Default TLS Settings**.

Here, in Cribl Edge's **Default cypher list** field, you can specify between one and all of the following supported cyphers:
- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`

### TLS in Cribl.Cloud {#tls-cloud}

TLS encryption is pre-enabled on several Sources in Cribl.Cloud, indicated on the Cribl.Cloud portal's **Data Sources** tab.
All TLS is terminated by individual Nodes. 

To enable TLS settings for additional Sources, use these configuration settings:
- **Private key path:** `/opt/criblcerts/criblcloud.key`
- **CA certificate path**: `/opt/criblcerts/criblcloud.crt`
- **Minimum TLS version**: `TLSv1.2`

For steps to enable TLS mutual authentication in Cribl.Cloud, see [Securing Communications](securing-communications#mutual-tls-cloud).

## Encryption Keys {#keys}

You can create and manage keys that Cribl Edge will use for real-time encryption of fields and patterns within events. For details on applying the keys that you define here, see [Encryption](/stream/securing-data-encryption#encryption-keys).

> In Cribl Edge 4.1 and later, for enhanced security, Cribl encrypts TLS certificate private keys in configuration files when you add or modify them. Before upgrading to v.4.1.0 (or later), back up your existing unencrypted key files – see [Safeguarding Unencrypted Private Keys for Rollback](upgrading#safeguarding). If you need to roll back to a pre-4.1 version, see [Restoring Unencrypted Private Keys](#restoring) below.
>
{.box .warning}

### Accessing Keys {#keys-access}

- In a single-instance deployment, select **Settings** > **Security > Encryption Keys**.
- In a distributed deployment with one Fleet, select **Settings > Security > Encryption Keys**.
- In a distributed deployment with multiple Fleets, keys are managed per Fleet. Select **Manage** > **Groups** > `<group-name>` **Group Settings > Security > Encryption Keys**.

On the resulting **Manage Encryption Keys** page, you can configure existing keys, and/or use the following options to add new keys.

### Get Key Bundle {#keys-import}

To import existing keys, click **Get Key Bundle**. You'll be prompted to supply a login and password to proceed.

### Add New Key {#keys-add}

To define a new key, click **New Key**. The resulting **New Key** modal provides the following controls:

**Key ID**: Cribl Edge will automatically generate this unique identifier.

**Description**: Optionally, enter a description summarizing this key's purpose.

**Encryption algorithm**: Currently, Cribl Edge supports the `aes-256-cbc` (default) and `aes-256-gcm` algorithms.

**KMS for this key**: Currently, the only option supported here is `local` (Cribl Edge's internal Key Management Service).

**Key class**: Classes are arbitrary collections of keys that you can map to different levels of access control. For details, see [Encryption](/stream/securing-data-encryption#key-classes). This value defaults to `0`; you can assign more classes, as needed. 

**Expiration time**: Optionally, assign the key an expiration date. Directly enter the date or select it from the date picker.

**Use initialization vector**: If enabled, Cribl Edge will seed encryption with a [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce) to make the key more random and unique. Optional (and defaults to disabled) with the `aes‑256‑cbc` algorithm; automatically enabled (and cannot be disabled) with the `aes‑256‑gcm` algorithm.

**Initialization vector size**: Length of the initialization vector (IV), in bytes. This option is displayed only with the `aes‑256‑gcm` algorithm. Defaults to `12` bytes to optimize interoperability, but you can use the drop-down to set this anywhere between `12`–`16` bytes.

### Restoring Unencrypted Private Keys {#restoring}

If you need to roll back from Cribl Edge 4.1 or later to an earlier version, follow this procedure to restore the unencrypted private key files that you earlier [backed up](upgrading#safeguarding).

1. Stop Cribl Edge 4.1 (or later) on hosts – the Leader and all Edge Nodes.
2. Untar the older Cribl Edge version (e.g., 4.0.4) onto the Leader and all Edge Nodes.
3. Manually edit each Cribl Edge config file that contains an encrypted private key. Replace each key with its prior unencrypted version.
4. Start up Cribl Edge, commit and deploy, and resume normal operations.

## Secrets {#secrets}

With Cribl Edge's secrets store, you can centrally manage secrets that Cribl Edge instances use to authenticate on integrated services. Use this UI section to create and update authorization tokens, username/password combinations, and API‑key/secret‑key combinations for reuse across the application.

### Accessing Secrets

- In a single-instance deployment, select **Settings** > **Security > Secrets**.
- In a distributed deployment with one Fleet, select **Configure > Settings > Security > Secrets**.
- In a distributed deployment with multiple Fleets, secrets are managed on each Fleet. Select **Groups >** `<group-name>` **Settings > Security > Secrets**.

On the resulting **Manage Secrets** page, you can configure existing secrets, and/or click **New Secret** to define new secrets.

### Add New Secret

The **New Secret** modal provides the following controls:

**Secret name**: Enter an arbitrary, unique name for this secret.

**Secret type**: See [below](#ssshhh) for this second field's options, some of which expose additional controls.

**Description**: Optionally, enter a description summarizing this secret's purpose.

**Tags**: Optionally, enter one or multiple tags related to this secret.

####  Secret Type  {#ssshhh}

This drop-down offers the following types:

**Text**: This default type exposes a **Value** field where you directly enter the secret.

**API key and secret key**: Exposes **API key** and **Secret key** fields, used to retrieve the secret from a secure endpoint. This is the only secret type supported on Cribl Edge's AWS-based Sources, Collectors, and Destinations, and on our Google Cloud Storage Destination.

**Username with password**: Exposes **Username** and **Password** fields, which you fill to retrieve the secret using Basic Authentication.

## CA Certificates and Environment Variables {#env-vars}

Where Cribl Edge [Sources](sources) and [Destinations](destinations) support TLS, each Source's or Destination's configuration provides a **CA Certificate Path** field where you can point to corresponding Certificate Authority (CA) `.pem` file(s). However, you can also use environment variables to manage CAs globally. Here are some common scenarios:

- **Add a set of trusted root CAs to the list of trusted CAs that Cribl Edge trusts**. Set the `NODE_EXTRA_CA_CERTS` environment variable for each Edge Node. For example, if you are using systemd, add the following line in each Edge Node's systemd unit file (replace `/<path>/<to>/<the>/<directory>/<containing>/<certs>/ca.pem` with the path to your CA `.pem` file):

  ```shell
  ...
  [Service]
  Environment="NODE_EXTRA_CA_CERTS=/<path>/<to>/<the>/<directory>/<containing>/<certs>/ca.pem"
  ...
  ```
  > When configuring TLS authentication on Edge Nodes, make sure you place your certificates into a separate directory outside of $CRIBL_HOME. If you place the certificates inside $CRIBL_HOME, they’ll be removed when the next config bundle is deployed from the Leader.
  >
  {.box .warning}
  
  For details about `NODE_EXTRA_CA_CERTS`, see the [node.js documentation](https://nodejs.org/docs/latest-v12.x/api/cli.html#cli_node_extra_ca_certs_file).

- **Configure Cribl Edge to accept all TLS certificates, regardless of their validity**. Set the `NODE_TLS_REJECT_UNAUTHORIZED` environment variable for each Edge Node. For example, if you are using systemd, add the following line in each Edge Node's systemd unit file:

  ```shell
  ...
  [Service]
  Environment="NODE_TLS_REJECT_UNAUTHORIZED=0"
  ...
  ```
  
  > `NODE_TLS_REJECT_UNAUTHORIZED=0` disables TLS certificate validation, which can decrease the security posture of your Cribl installation. For this reason, we recommend avoiding `NODE_TLS_REJECT_UNAUTHORIZED=0` in production environments. Instead, use the `NODE_EXTRA_CA_CERTS` environment variable to explicity trust the necessary certificates.
  >
  > For details about `NODE_TLS_REJECT_UNAUTHORIZED`, see the [node.js documentation](https://nodejs.org/docs/latest-v12.x/api/cli.html#cli_node_tls_reject_unauthorized_value).
  >
  {.box .warning}


