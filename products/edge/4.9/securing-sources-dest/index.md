# Secure Cribl Edge Sources and Destinations with Certificates

You can use [certificates](#worker-group-certs) and [custom HTTP headers](#http) to authenticate Cribl Edge with external Sources and Destinations, enhancing the security of your data pipelines.

## Setting Authentication on Sources/Destinations {#worker-group-certs}

You can use certificates to authenticate Cribl Edge to external data senders and receivers. In the Cribl Edge UI, you configure the certificate and its associated private key differently depending on your deployment type:

- On Cribl.Cloud, navigate to **Settings** > **Global** > **Security** > **Certificates**.

- For Single-instance deployments, navigate to **Settings** > **Security** > **Certificates** ([Edge](/edge/deploy-single-instance/), [Stream](/stream/deploy-single-instance/)). Next, select **Add Certificate**.

- For Distributed deployments, navigate to a Fleet and select **Fleet Settings** > **Security** > **Certificates** ([Edge](/edge/setting-up-leader-and-edge-nodes/), [Stream](/stream/deploy-distributed/)). Next, select **Add Certificate**.

> As an alternative to the preconfiguration in steps 1–4 below, you can import a certificate on the fly, using the **Create** button for certificates in the TLS Settings for the Source or Destination. 
{.box .success}

In the **New Cerfificate** modal: 
1. Paste or upload `.pem` files, according to the instructions in [Import Certificate and Key](securing-import-certs#upload-certificate).
2. Supply a **Passphrase** and/or **CA certificate**, if required by your integration partner's certificate.
3. Click **Save**.
4. Open the configuration modal for the Source or Destination.
5. Select **TLS Settings (Client Side)** or **TLS Settings (Server Side)**, depending on the integration.
6.  Toggle **Enabled** on.
7.   In the **Certificate name** drop-down, select a certificate that you've preconfigured for this integration. (This will auto-populate the corresponding fields here.)  
> Or, if you're creating a new certificate, instead click the adjacent **Create** button. 
1.  Click **Save**.

  > Cribl Edge will create new certificates at the same Fleet level that you're configuring. You can verify this at the bottom of the **Create new certificate** modal by making sure that the **Referenced** table includes rows populated for the appropriate **Sources** and **Destinations**.
  {.box .success}

  ![Group-level certificate modal](certificate-modal-group-level.f642a1e6e5.png)
  {border="true"}

> Certificate revocation can lead to unexpected service interruptions. To mitigate risks:
>   *  Regularly review and renew certificates. 
>   *  Implement robust monitoring and alerting systems to detect potential issues. 
>   *  Ensure secure storage of certificates and private keys. 
>   *  In case of emergency, have a well-defined recovery plan to restore service quickly.
>
{.box .warning}

## Custom HTTP Headers {#http}

You can also encode custom, security-related HTTP headers as needed. In a Distributed deployment, navigate to [ Fleet Settings](settings-group-fleet), then **API Server Settings** > **Advanced** > **HTTP Headers**. (In a Single-instance deployment, the **API Server Settings** are within **Global Settings** > **General Settings**.) Define HTTP headers as key-value pairs, clicking **Add Header** as needed.

### Implement a Custom Content Security Policy (CSP) {#csp}

A Content Security Policy (CSP) helps protect Cribl Edge from Cross-Site Scripting (XSS) and other web injection attacks by controlling which resources the browser is allowed to load. You can implement CSP using the [Custom HTTP Headers](#custom) setting described above.

#### Basic Configuration

To configure a basic CSP for Cribl Stream, add the following header in your **HTTP Headers** settings:

**Header Name:** `Content-Security-Policy`

**Header Value:**
```
default-src 'self'; script-src 'self' 'unsafe-eval' 'sha256-ZaAN8+jAf3MdnSLRdmTuExUhAwcHulq3JnLJbyqInfc=' 'sha256-+aa8QH7Wjp/9wCcmtCveJhRIN9pSaWU+Ojoe1aIvLmI='; connect-src 'self' https://cdn.cribl.io https://ai.cribl.cloud; frame-src 'self' blob: https://cdn.cribl.io; style-src 'self' 'unsafe-inline'; img-src 'self' data:;
```

Test your CSP policy thoroughly in a development environment before deploying to production and monitor the browser console for CSP violations after implementation.

### SAML SSO Configuration

If your organization uses SAML Single Sign-On, you'll need a modified CSP policy to accommodate the dynamic JavaScript and form submissions required for SAML authentication:

**Header Name:** `Content-Security-Policy`

**Header Value for SAML:**
```
default-src 'self'; script-src 'self' 'unsafe-eval' 'unsafe-inline'; connect-src 'self' https://cdn.cribl.io https://ai.cribl.cloud; frame-src 'self' blob: https://cdn.cribl.io; style-src 'self' 'unsafe-inline'; img-src 'self' data:; form-action 'self' https://YOUR_IDP_DOMAIN;
```
> Replace `YOUR_IDP_DOMAIN` with your actual Identity Provider's domain (for example, your Okta domain).
>
{.box .info}

SAML configurations require additional permissions that reduce the restrictiveness of the CSP compared to the basic configuration.




