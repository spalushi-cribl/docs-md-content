# Securing Communications


This page outlines how to protect Cribl Edge Leader to Edge Nodes communications, using an existing TLS/SSL certificate and key. 

Cribl Edge expects certificates and keys to be formatted in privacy-enhanced mail (`.pem`) format. To generate a self-signed certificate and corresponding key, see [Securing Cribl Edge](securing-and-monitoring#ssl).

## Importing Certificate and Key {#upload-certificate}

To use your certificate and key to prepare secure communications between Workers/Edge Nodes and the Leader:

1. Navigate to the Leader's **Settings** > **Global Settings** > **Security** > **Certificates** > **New Certificates** modal. 
2. Open your TLS/SSL certificate file. (The self-signed certificate [example](securing-and-monitoring#ssl) used the placeholder name `myCert.pem`.)
3. Copy the file's contents to your clipboard.
4. Paste the file's contents into the modal's **Certificate** field.

    > You can skip the preceding three steps: Just drag/drop your `.pem` file from your filesystem into the **Certificate** field, or upload it using the button at the field's upper right.
    {.box .success}

5. Open your private key file. (The self-signed certificate [example](securing-and-monitoring#ssl) used the placeholder name `myKey.pem`.)
6. Copy the file's contents to your clipboard.
7. Paste the clipboard contents into the same Leader modal's **Private key** field.

    > Here again, you can skip the preceding three steps by dragging/dropping or uploading the `.pem` file from your filesystem.
    {.box .success}

8. Fill in the **Name** and **Description** fields. 
9. If you've uploaded a self-signed certificate, just **Save** it now.
10. If your private key is encrypted, fill in the modal's **Passphrase** with the corresponding key.  
(You can paste the key's contents, or you can drag/drop or upload the key file.)
11. If you're uploading a certificate signed by an external certificate authority – e.g., a downloaded Splunk Cloud certificate – import the chain into the **CA certificate** field before saving the cert. For details, see [Obtain the Certificate Chain (TLS/SSL)](/stream/git-certificate-errors#chain).



![Leader's certificate modal, populated](tls-leader-new-cert.7f15b79d64.png)
{border="true"}

## Connecting to the Leader Securely {#connect-workers}

> For Cribl.Cloud Leader deployments, TLS is enabled by default, so you don't need to configure it manually — the Cribl.Cloud platform handles this automatically. The TLS configurations described below are only for on-prem and self-managed deployments.
{.box .cloud}

You can configure secure communication between your Leader and Edge Nodes using the UI, the `instance.yml` config file, or environment variables.

### Using the UI 

To set up secure communication via the UI, you configure first the [Fleet](#group-tls), then the [Edge Node](#node-tls), then the [Leader](#tls-leader).

#### Fleet Setup {#group-tls}

For each Fleet whose Edge Nodes you want to secure:

1. Open your TLS/SSL certificate file, and copy its contents to your clipboard. (This can be the same certificate you [uploaded to the Leader](#upload-certificate), or a different cert.)
1. Select **Manage**, then select the Fleet you want to configure.
1. Select **Group Settings** or **Fleet Settings** (upper right). 
1. From the left nav, select **Security** > **Certificates** > **TLS**.
1. Click **Add Certificate**.
1. In the resulting **New Certificates** modal, add the cert's contents to the **Certificate** field.  
(You can paste the cert file's contents, or you can drag/drop or upload the `.pem` file.)
1. As you [did on the Leader](#upload-certificate), also insert your **Private key**, and (as needed) your **Passphrase** and **CA certificate**.
1. Click **Save**.
1. Commit and Deploy the Fleet's new configuration, including the new cert.
1. Repeat the preceding steps on each Fleet.

![Group-level certificates are configured like the Leader's cert](tls-group-new-cert.df6085a3c1.png)
{border="true"}

#### Edge Node Setup {#node-tls}

For each Edge Node that you want to secure:

1. Enable the Leader's UI access to each desired Edge Node ([Stream](/stream/deploy-distributed#worker-access), [Edge](/edge/setting-up-leader-and-edge-nodes#edge-access)). 
1. Tunnel through from the Leader to a Edge Node's UI.
1. Navigate to this Edge Node's **Worker Settings** (upper right) > **System** > **Distributed Settings** > **TLS Settings**.
1. Toggle **Enable Server TLS** to `Yes`.  
This will expose the remaining TLS settings.
1. From the **Certificate name** drop-down, select the certificate you [uploaded](#group-tls) to the parent Fleet.  
This will prefill all the required fields. (See all deployed certificates at the left nav's **Security** > **Certificates** link.)
1. Click **Save**.  
The Worker will be unavailable during a short lag, while it restarts with the new configuration.
1. Repeat the preceding steps on each Edge Node.

![Configuring TLS on Worker's/Edge Node's UI, from the Leader](worker-to-leader-tls-cert-enabling.96e79b9506.png)
{border="true"}

#### Leader Setup {#tls-leader}

Next, return to the Leader's UI:

1. Select **Settings** > **Global Settings**> **System** > **Distributed Settings** > **TLS Settings**.
2. Toggle **Enable server TLS** to `Yes`.
3. In the **Certificate name** drop-down, select an existing Certificate. This will auto-populate the corresponding cert fields.
4. Click **Save**.

> After you've enabled TLS on the Leader, generating bootstrap scripts to [add](/stream/deploy-workers#add-bootstrap) or [update](/stream/deploy-workers#update-existing) Edge Nodes will automatically prepend `https://` to the Leader's URL.
>
{.box .info}

### Using YAML Config File 

You can also configure the Leader's `$CRIBL_HOME/local/_system/instance.yml` [file](instanceyml) to ensure that TLS is enabled. Here's the relevant section:

```yaml 
distributed:
  mode: managed-edge
  master:
    host: <hostname>
    port: 4200
    authToken: criblmaster
    tls:
      disabled: false
      rejectUnauthorized: false
      requestCert: false
    resiliency: none
  group: default_fleet
```

> After you've enabled TLS on the Leader, generating bootstrap scripts to [add](/stream/deploy-workers#add-bootstrap) or [update](/stream/deploy-workers#update-existing) Edge Nodes will automatically prepend `https://` to the Leader's URL.
>
{.box .info}

### Using Environment Variables 

Another way to set up secure communications between Edge Nodes and the Leader is via environment variables ([Stream](/stream/deploy-distributed#environment-variables), [Edge](/edge/setting-up-leader-and-edge-nodes#using-environment-variables)).

If you deploy your Edge Nodes in a container, you can enable encrypted TLS communications with the Leader by configuring the  `CRIBL_DIST_LEADER_URL` with the `tls:` protocol. This will override the default setting in `instance.yml`. Here’s the format:

`CRIBL_DIST_LEADER_URL=tls://<authToken>@leader:4200`

## Configuring TLS Mutual Authentication {#mutual}

Once you have configured the Leader for encrypted TLS communication, you can implement client certificate exchange to enable mutual authentication. This allows Cribl Edge to permit only explicitly authorized clients, which hold valid certificates, to connect to the Leader.

When a client certificate is presented to a Leader, two things happen:

1. The Leader validates the client certificate presented to Cribl Edge. The `Validate Client Certs` setting is optional, but highly recommended. When enabled, the Leader checks the certificate against the trust store, to see if it has been signed by a valid certificate authority (CA). 

    The Leader checks the list of certificates in the `CA Certificates Path` box first (if populated), then against the list of built-in system certificates.

1. The Leader checks whether the `Common Name` (CN) matches the regular expression in the configuration. Cribl Edge's default is to accept any value in the `Common Name` field. You can customize this as needed. 

    Within the `Common Name`, we validate against the value after the `CN=string`. If your `Common Name` is `CN=logstream.worker`, you would enter `logstream\.worker` in the `Common Name` field – including the backslash, because the value entered is a regular expression.

### Limitations on TLS Mutual Auth

When configuring TLS mutual authentication on Edge Nodes, make sure you place your certificates into a separate directory outside of `$CRIBL_HOME`. If you place the certificates inside `$CRIBL_HOME`, they’ll be removed when the next config bundle is deployed from the Leader.

Similarly, you can't bootstrap Edge Nodes with mutual authentication already populated. To bootstrap Edge Nodes, you supply only the shared authentication token. Certificates should be viewed as two-factor authentication; so placing the certificates in the config bundle defeats the purpose of two-factor authentication. 

### Using the UI 

Below is a sample configuration on the Leader: 

![Configuring Mutual Authentication](worker-to-leader-mutual-tls.7110dea6fa.png)

In the above configuration, note:

- You can set both the **Minimum TLS version** and **Maximum TLS version**.

- The `Common Name` regex contains an additional check for `\d+`. This allows checking for the format: `logstream1.worker`, `logstream2.worker`, `logstream3.worker`, etc.

### Using YAML Config File 

To set up TLS **mutual** authentication via the Edge Node's  `instance.yml` config file, you need to change some values and also add a few keys, as shown in this example:

```yaml 
distributed:
  mode: managed-edge
  master:
    host: <hostname>
    port: 4200
    authToken: criblmaster
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

### Using Environment Variables 

You can set up TLS mutual authentication by configuring this environment variable, using the format shown in this example:

```shell
CRIBL_DIST_LEADER_URL="tls://<authToken>@leader.cribl:4200?tls.privKeyPath=/path/to/certs/worker.key&tls.certPath=/path/to/certs/workers.pem&tls.caPath=/path/to/certs/root.pem&tls.requestCert=true&tls.rejectUnauthorized=true"
```

Once you've set this variable, restart the Edge Node. You should see the Edge Node successfully reconnect to the Leader.

If the Edge Node doesn't connect, check `cribl.log` on both the Edge Node and Leader for more context about the problem. You should see errors related to `dist leader communications`.

> To build your own Certificate Authority (a self-signed CA), see our blog post on [How to Secure Cribl Edge Worker-to-Leader Communications](https://cribl.io/blog/how-to-secure-logstream-worker-to-leader-communications/). 
>
{.box .info}

## Setting Authentication on Sources/Destinations {#worker-group-certs}

You can use certificates to authenticate Cribl Edge to external data senders and receivers. You configure this at the Group level, as follows:

1. Select a Group.

    > As an alternative to the preconfiguration in steps 2–6, you can import a certificate on the fly, using the Source's or Destination's **Create** button. See this section's final steps below.
    {.box .success}

2. Open **Group Settings** (top right) > **Security** > **Certificates**.
3. Select **New Certificates**.
4. Paste or upload `.pem` files, as in [Setting Up the Encrypted Channel](#upload-certificate).
5. Supply a **Passphrase** and/or **CA certificate**, if required by your integration partner's certificate.
7. Click **Save**.
8. Open your relevant Source's or Destination's config modal.
9. Select **TLS Settings (Client Side)** or **TLS Settings (Server Side)**, depending on the integration.
10. Slide **Enabled** on.
11. In the **Certificate name** drop-down, select a cert that you've preconfigured for this integration. This will auto-populate the corresponding fields here.
12. If you're creating a certificate on the fly, click the **Create** button beside **Certificate name**. 
12. Click **Save**.

    > Cribl Edge will create new certificates at the same Group level that you're configuring. You can verify this at the **Create new certificate** modal's bottom, by making that the **Referenced** table includes rows populated for the appropriate **Sources** and **Destinations**.
    {.box .success}

  ![Group-level certificate modal](certificate-modal-group-level.f642a1e6e5.png)
  {border="true"}

## Securing the Leader Node {#securing-leader}

This is a best practice that enables the Leader to validate itself to clients. We can secure it using the self-signed cert we created in [Securing Cribl Edge](securing-and-monitoring#ssl):

1. Navigate to **Settings** > **Global Settings** > **General Settings** > **API Server Settings** > **TLS**.
2. Slide the toggle to **Enabled**.
3. From the **Certificate Name** drop-down, select a cert you've previously imported. This will populate the corresponding fields here.
4. Click **Save**.

    > After this save, you must prepend `https://` to all Cribl Edge URLs on the Leader Node. E.g., to get back to the Settings page you just configured, you'll now need to use `https://<hostname>:<port>/settings/system`)`.
    {.box .warning}
