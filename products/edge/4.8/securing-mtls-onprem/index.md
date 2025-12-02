# Configure mTLS Authentication On-Prem

Once you have configured the Leader for encrypted TLS communication, you can implement client certificate exchange to enable mutual authentication (mTLS). This allows Cribl Edge to permit only explicitly authorized clients, which hold valid certificates, to connect to the Leader.

When a client certificate is presented to a Leader, two things happen:

1. The Leader validates the client certificate presented to Cribl Edge. The `Validate Client Certs` setting is optional, but highly recommended. When enabled, the Leader checks the certificate against the trust store, to see if it has been signed by a valid certificate authority (CA). 

  The Leader checks the list of certificates in the `CA Certificates Path` box first (if populated), then against the list of built-in system certificates.

2. The Leader checks whether the `Common Name` (CN) matches the regular expression in the configuration. Cribl Edge's default is to accept any value in the `Common Name` field. You can customize this as needed. 

  Within the `Common Name`, we validate against the value after the `CN=string`. If your `Common Name` is `CN=logstream.worker`, you would enter `logstream\.worker` in the `Common Name` field – including the backslash, because the value entered is a regular expression.

  > The Cribl.Cloud Leader connections on port `4200/tcp` do not currently support mutual TLS (mTLS).
  {.box .info}

## Limitations on mTLS Authentication

When configuring mTLS authentication on Edge Nodes, make sure you place your certificates into a separate directory outside of `$CRIBL_HOME`. If you place the certificates inside `$CRIBL_HOME`, they’ll be removed when the next config bundle is deployed from the Leader.

Similarly, you can't bootstrap Edge Nodes with mutual authentication already populated. To bootstrap Edge Nodes, you supply only the shared authentication token. Certificates should be viewed as two-factor authentication; so placing the certificates in the config bundle defeats the purpose of two-factor authentication. 

## Configure via the UI

> mTLS authentication in Cribl.Cloud is [configured](securing-tls-cloud) separately for each Source.
>
{.box .cloud}

Below is a sample configuration on the Leader: 

![Configuring Mutual Authentication](worker-to-leader-mutual-tls.763b4b4403.png)
{border="true"}

In the above configuration, note:

- You can set both the **Minimum TLS version** and **Maximum TLS version**.

- The `Common Name` regex contains an additional check for `\d+`. This allows checking for the format: `logstream1.worker`, `logstream2.worker`, `logstream3.worker`, etc.

## Using YAML Config File 

To set up mTLS authentication via the Edge Node's  `instance.yml` config file, you need to change some values and also add a few keys, as shown in this example:

```yaml 
distributed:
  mode: managed-edge
  master:
    host: <hostname>
    port: 4200
    authToken: <token>
    tls:
      disabled: false
      rejectUnauthorized: true # false if ignoring untrusted certs
      requestCert: true
      privKeyPath: /path/to/certs/worker.key
      certPath: /path/to/certs/worker.pem
      caPath: /path/to/certs/root.pem
    resiliency: none
  group: default_fleet     
```

## Using Environment Variables 

You can set up mTLS authentication by configuring this environment variable, using the format shown in this example:

```shell
CRIBL_DIST_LEADER_URL="tls://<authToken>@leader.cribl:4200?tls.privKeyPath=/path/to/certs/worker.key&tls.certPath=/path/to/certs/workers.pem&tls.caPath=/path/to/certs/root.pem&tls.requestCert=true&tls.rejectUnauthorized=true"
```

Once you've set this variable, restart the Edge Node. You should see the Edge Node successfully reconnect to the Leader.

If the Edge Node doesn't connect, check `cribl.log` on both the Edge Node and Leader for more context about the problem. You should see errors related to `dist leader communications`.

> To build your own Certificate Authority (a self-signed CA), see our blog post on [How to Secure Cribl Stream Worker-to-Leader Communications](https://cribl.io/blog/how-to-secure-logstream-worker-to-leader-communications/). 
>
{.box .info}

