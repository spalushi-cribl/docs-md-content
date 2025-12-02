# TLS Defaults and System-wide Settings


Cribl Edge prioritizes secure communication by default. This ensures the integrity and confidentiality of data transmitted between your Cribl Edge instance and various Sources and Destinations.

## TLS Version Support and Defaults {#defaults}

The minimum supported TLS version in Cribl Edge is `TLS 1.2` by default. This default applies to all secure connections, including supported Sources and Destinations, Worker/Leader communications, and Cribl.Cloud. 

Cribl.Cloud and on-prem deployments use the same TLS settings, with `TLS 1.2` being the default minimum version.

> You can check whether a Source or Destination supports TLS by reviewing the information box at the beginning of its corresponding documentation page.
>
{.box .info}

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

## System-wide TLS Settings Including Ciphers {#ciphers}

You can configure advanced, system-wide TLS settings – minimum and maximum TLS versions, default cipher lists, and ECDH curve names. Select **Settings** > **Global** > **System > General Settings > Default TLS Settings**.

In the **Default cipher list** field, you can specify one or more ciphers from the following list:

- `ECDHE-RSA-AES128-GCM-SHA256`
- `ECDHE-ECDSA-AES128-GCM-SHA256`
- `ECDHE-RSA-AES256-GCM-SHA384` 
- `ECDHE-ECDSA-AES256-GCM-SHA384`
- `DHE-RSA-AES128-GCM-SHA256`
- `ECDHE-RSA-AES128-SHA256`
- `DHE-RSA-AES128-SHA256` 
- `ECDHE-RSA-AES256-SHA384` 
- `DHE-RSA-AES256-SHA384`
- `ECDHE-RSA-AES256-SHA256`
- `DHE-RSA-AES256-SHA256` 
- `HIGH` 
- `!aNULL` 
- `!eNULL` 
- `!EXPORT`
- `!DES`
- `!RC4`
- `!MD5`
- `!PSK`
- `!SRP`
- `!CAMELLIA`


## CA Certificates and Environment Variables {#env-vars}

For any Cribl Edge [Source](sources) or [Destination](destinations) that supports TLS, you can configure a **CA Certificate Path** field that points to a Certificate Authority (CA) `.pem` file(s). However, you can also use environment variables to manage CAs globally. Here are some common scenarios:

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

> As a result of a [Known Issue](/results/?searchOption=known-issues&tickets=CRIBL-33283),
> passphrases in TLS certificates configured for Sources or Destinations in a Cribl Edge Fleet
> will not be decrypted correctly when a Subfleet inherits the configuration.
> 
> Avoid using passphrases with TLS certificates if you intend to have Subfleets
> inherit Source or Destination configurations.
{.box .warning}

### Global CA Certificates and Sources

While Cribl Edge supports using environment variables like `NODE_EXTRA_CA_CERTS` to add trusted root CAs globally, this approach is not applicable to all Sources. Some Sources, such as the [Windows Events Fowarder](sources-wef) (WEF), require a specific CA certificate configured at the Source level to function correctly.

Additionally, OS certificate stores accessed through Node.js boot flags (such as  `--use-bundled-ca`, `--use-openssl-ca`) are not supported by Cribl Edge. These flags are stripped during application startup.

In summary, certain Sources, like the WEF, exclusively rely on the specific CA certificate configured within the Source itself.