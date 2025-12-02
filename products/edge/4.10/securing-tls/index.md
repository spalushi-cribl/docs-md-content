# Configure TLS for API and UI Access


To safeguard inbound communications to your Edge Nodes, configure Transport Layer Security (TLS) on the Leader. This will enable secure access to the Cribl Edge API and UI.

> In Cribl Edge v.4.1 and newer, for enhanced security, Cribl encrypts TLS certificate private keys in configuration files when you add or modify them. Before upgrading to v.4.1.0 (or newer), back up your existing unencrypted key files – see [Safeguarding Unencrypted Private Keys for Rollback](/stream/upgrading#safeguarding/). If you need to roll back to a pre-4.1 version, see [Restore Unencrypted Private Keys](#restoring) below.
>
{.box .warning}

You can use your own certificates and private keys, or you can generate a pair with [OpenSSL](https://www.openssl.org/), as shown here:

`openssl req -nodes -new -x509 -newkey rsa:2048 -keyout myKey.pem -out myCert.pem -days 420`

This example command will generate both a self-signed cert `myCert.pem` (certified for 420 days), and an unencrypted, 2048-bit RSA private key `myKey.pem`. (Change the filename arguments to modify these placeholder names.) 

> As indicated by these examples, Cribl Edge expects certificates and keys to be formatted in privacy-enhanced mail (`.pem`) format.
>
{.box .info}

In the Cribl Edge UI, you configure the certificate and its associated private key differently depending on your deployment type:

- For Single-instance deployments, navigate to **Settings** > **Globals** > **Security** > **Certificates** ([Edge](/edge/deploy-single-instance/), [Stream](/stream/deploy-single-instance/)).

- For Distributed deployments, navigate to a Fleet and select **Fleet Settings** > **Security** > **Certificates** ([Edge](/edge/setting-up-leader-and-edge-nodes/), [Stream](/stream/deploy-distributed/)).

Alternatively, in the `local/cribl.yml` file, you can edit the `api` section to directly set the `privKeyPath` and `certPath` attributes. For example:

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

> See [Secure Leader/Edge Nodes Communication](securing-communications) for details about using this certificate and key to secure communications on, and among, your Cribl Edge Leader and Edge Nodes.
>
{.box .success}

## Restore Unencrypted Private Keys {#restoring}

If you need to roll back from Cribl Edge v.4.1 or later to an earlier version, follow this procedure to restore the unencrypted private key files that you [backed up](stream/upgrading#safeguarding) earlier. 

1. Stop Cribl Edge v.4.1 (or later) on hosts – the Leader and all Edge Nodes.
2. Untar the older Cribl Edge version (v.4.0.4, for example) onto the Leader and all Edge Nodes.
3. Manually edit each Cribl Edge config file that contains an encrypted private key. Replace each key with its prior unencrypted version.
4. Start up Cribl Edge, commit and deploy, and resume normal operations.