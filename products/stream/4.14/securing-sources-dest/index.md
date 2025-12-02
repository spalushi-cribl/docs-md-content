# Secure Sources and Destinations with Certificates


You can secure connections between Cribl Stream and external systems by using TLS (Transport Layer Security) certificates or custom HTTP headers. These extra security measures are optional, but TLS certificates or custom HTTP headers help authenticate data senders and receivers, protect data in transit, and prevent unauthorized access.

## Certificate-Based Authentication {#worker-group-certs}

Cribl Stream stores and manages certificates at the Worker Group level. When you add a certificate to a Source or Destination, Cribl Stream creates it within the same Worker Group where you configured the integration.

> For information about how certificate references work in Packs, see [Use Named TLS Certificates](packs#named-certificates).
>
{.box .info}

### Add a TLS Certificate to a Worker Group {#add-certificate-to-group}

To add a TLS certificate that will be available to all Sources and Destinations at the Worker Group level:

1. On the top bar, select **Products**, and then select **Cribl Stream**. Under **Worker Groups**, select a Worker Group.

1. Navigate to **Group Settings**. On the side menu, expand **Security** and select **Certificates**.

1. Select **Add Certificate**. In the **New Certificate** modal:

   - **Name:** Enter a unique name to identify the certificate.
   - **Description:** Optionally, add a short description of the certificate's scope or purpose.
   - **Certificate:** Upload or paste the full contents of your certificate file. Ensure you paste the entire block, including the `BEGIN` and `END` lines:

     ```
     -----BEGIN CERTIFICATE-----
     ... (certificate data) ...
     -----END CERTIFICATE-----
     ```
   - **Private key:** Upload or paste the full contents of your private key (`.pem`). Ensure you paste the entire block, including the `BEGIN` and `END` lines. Some private keys may use labels like `RSA PRIVATE KEY` or `EC PRIVATE KEY`. Be sure to include the correct `BEGIN` and `END` lines that match your key type.
   - **Passphrase:** Optionally include the certificate's passphrase if it uses passphrase encryption.
   - **CA certificate:** Optionally upload or paste a certificate signed by an external certificate authority by importing the chain. See [Obtain the Certificate Chain (TLS/SSL)](/stream/git-certificate-errors#chain) for more information.
   - **Referenced:** This table shows which Sources and Destinations are using this certificate. If this table is empty, you haven't assigned this certificate to any components yet.

1. Select **Save**. The certificate now appears in the list of certificates for the Worker Group.

### Add a TLS Certificate to a Source or Destination

To add a TLS certificate directly to an integration:

1. Open the configuration settings for your Source or Destination.

1. Select **TLS Settings (Client Side)** or **TLS Settings (Server Side)**, depending on the integration.

1. Toggle **Enabled** on.

1. Use the **Certificate name** drop-down to add a certificate:

   - **To add a new certificate:** Select the **Create** button next to the field. See [Add a TLS Certificate to a Worker Group](#add-certificate-to-group) for field descriptions.
   - **To add an existing certificate:** Select the name of the certificate from the drop-down. This will auto-populate the related fields.

1. Select **Save**.

### Maintain TLS Certificates

Certificate revocation can lead to unexpected service interruptions. To mitigate risks:

- Regularly review and renew certificates.
- Implement robust monitoring and alerting systems to detect potential issues.
- Ensure secure storage of certificates and private keys.
- In case of emergency, have a well-defined recovery plan to restore service quickly.

## Custom HTTP Headers {#custom}

You can also encode custom, security-related HTTP headers as needed. In a Distributed deployment, navigate to Worker Group Settings, then **API Server Settings** > **Advanced** > **HTTP Headers**. (In a Single-instance deployment, the **API Server Settings** are within **Global Settings** > **General Settings**.) Define HTTP headers as key-value pairs, selecting **Add Header** as needed.

### Implement a Custom Content Security Policy (CSP) {#csp}

Content Security Policy (CSP) helps protect Cribl Stream from Cross-Site Scripting (XSS) and other web injection attacks by controlling which resources the browser is allowed to load. You can implement CSP using the [Custom HTTP Headers](#custom) setting described above.

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
