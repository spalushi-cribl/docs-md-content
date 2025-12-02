# Secure Leader/Edge Nodes Communication


You can configure secure communication between your Leader and Edge Nodes. As an additional security measure, we recommend you prevent direct browser access to the UI for all Edge Nodes. 

## Secure Browser Access to Edge Node {#ui-access}

Cribl recommends that in enterprise distributed deployments, you prevent direct browser access to the UI for all Edge Nodes.

> On the Leader Node, start by enabling UI access to Edge Nodes ([Stream](/stream/setting-up-leader-and-worker-nodes#teleporting), [Edge](/edge/setting-up-leader-and-edge-nodes#edge-access)). This way, admins will still be able to tunnel through from the Leader to the UI on any Edge Node. This is also a prerequisite for [Connect to the Leader Securely](#connect-workers) described below.
>
{.box .warning}

1. Select a Fleet.
2. Open **Fleet Settings** (top right).
3. Under **General Settings** > **API Server Settings**, select **Advanced**.
4. Toggle **Local UI Access** to `No`.
5. Select **Save**.

## Connect to the Leader Securely {#connect-workers}

> For Cribl.Cloud Leader deployments, TLS is enabled by default, so you don't need to configure it manually — the Cribl.Cloud platform handles this automatically. The TLS configurations described below are only for on-prem and self-managed deployments.
{.box .cloud}

You can configure secure communication between your Leader and Edge Nodes using the UI, the `instance.yml` config file, or environment variables. 

## Secure via the UI

To set up secure communication via the UI, you configure first the [Fleet](#group-tls), then the [Edge Node](#node-tls), then the [Leader](#tls-leader).

### Fleet Setup {#group-tls}

For each Fleet whose Edge Nodes you want to secure:

1. Open your TLS/SSL certificate file, and copy its contents to your clipboard. (This can be the same certificate you [uploaded to the Leader](securing-import-certs#upload-certificate)), or a different cert.)
1. Select **Manage**, then select the Fleet you want to configure.
1. Select **Group Settings** or **Fleet Settings** (upper right). 
1. From the left nav, select **Security** > **Certificates** > **TLS**.
1. Click **Add Certificate**.
1. In the resulting **New Certificates** modal, add the cert's contents to the **Certificate** field.  
(You can paste the cert file's contents, or you can drag/drop or upload the `.pem` file.)
1. As you [did on the Leader](securing-import-certs#upload-certificate), also insert your **Private key**, and (as needed) your **Passphrase** and **CA certificate**.
1. Click **Save**.
1. Commit and Deploy the Fleet's new configuration, including the new cert.
1. Repeat the preceding steps on each Fleet.

![Group-level certificates are configured like the Leader's cert](tls-group-new-cert.df6085a3c1.png)
{border="true"}

### Edge Node Setup {#node-tls}

For each Edge Node that you want to secure:

1. Enable the Leader's UI access to each desired Edge Node ([Stream](/stream/setting-up-leader-and-worker-nodes#teleporting), [Edge](/edge/setting-up-leader-and-edge-nodes#edge-access)). 
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

### Leader Setup {#tls-leader}

Next, return to the Leader's UI:

1. Select **Settings** > **Global Settings**> **System** > **Distributed Settings** > **TLS Settings**.
2. Toggle **Enable server TLS** to `Yes`.
3. In the **Certificate name** drop-down, select an existing Certificate. This will auto-populate the corresponding cert fields.
4. Click **Save**.

> After you've enabled TLS on the Leader, generating bootstrap scripts to [add](/stream/deploy-workers#add-bootstrap) or [update](/stream/deploy-workers#update-existing) Edge Nodes will automatically prepend `https://` to the Leader's URL.
>
{.box .info}

## Secure via the YAML Config File

You can also configure the Leader's `$CRIBL_HOME/local/_system/instance.yml` [file](instanceyml) to ensure that TLS is enabled. Here's the relevant section:

```yaml 
distributed:
  mode: managed-edge
  master:
    host: <hostname>
    port: 4200
    authToken: <token>
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

## Secure via Environment Variables 

Another way to set up secure communications between Edge Nodes and the Leader is via environment variables ([Stream](/stream/setting-up-leader-and-worker-nodes#environment-variables), [Edge](/edge/setting-up-leader-and-edge-nodes#using-environment-variables)).

If you deploy your Edge Nodes in a container, you can enable encrypted TLS communications with the Leader by configuring the  `CRIBL_DIST_LEADER_URL` with the `tls:` protocol. This will override the default setting in `instance.yml`. Here’s the format:

`CRIBL_DIST_LEADER_URL=tls://<authToken>@leader:4200`


## Secure the Leader Node {#securing-leader}

This is a best practice that enables the Leader to validate itself to clients. We can secure it using the self-signed certificate we created in [Configure SSL Certificates](securing-ssl):

1. Navigate to **Settings** > **Global Settings** > **General Settings** > **API Server Settings** > **TLS**.
2. Slide the toggle to **Enabled**.
3. From the **Certificate Name** drop-down, select a cert you've previously imported. This will populate the corresponding fields here.
4. Click **Save**.

  > After this save, you must prepend `https://` to all Cribl Edge URLs on the Leader Node. E.g., to get back to the Settings page you just configured, you'll now need to use `https://<hostname>:<port>/settings/system`)`.
  {.box .warning}

