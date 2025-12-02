# Configure SSL Certificates


You can secure access to the Cribl Stream API and UI by configuring Secure Sockets Layer (SSL). Do this on the Leader, to secure inbound communications to the Worker Nodes.

You can use your own certificates and private keys, or you can generate a pair with [OpenSSL](https://www.openssl.org/), as shown here:

`openssl req -nodes -new -x509 -newkey rsa:2048 -keyout myKey.pem -out myCert.pem -days 420`

This example command will generate both a self-signed cert `myCert.pem` (certified for 420 days), and an unencrypted, 2048-bit RSA private key `myKey.pem`. (Change the filename arguments to modify these placeholder names.) 

> As indicated by these examples, Cribl Stream expects certificates and keys to be formatted in privacy-enhanced mail (`.pem`) format.
>
{.box .info}

In the Cribl Stream UI, you configure the certificate and its associated private key differently depending on your deployment type:

- For Single-instance deployments, navigate to **Settings** > **Global Settings** > **Security** > **Certificates** ([Edge](/edge/deploy-single-instance), [Stream](/stream/deploy-single-instance)).

- For Distributed deployments, navigate to **Manage** > **Groups** > `<group‑name>` > **Group Settings** > **Security** > **Certificates** ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)).

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

> See [Secure Leader/Worker Nodes Communication](securing-communications) for details about using this certificate and key to secure communications on, and among, your Cribl Stream Leader and Worker Nodes.
>
{.box .success}
