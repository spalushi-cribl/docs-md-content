# Securing Cribl.Cloud with TLS and Mutual TLS


Cribl.Cloud prioritizes secure data transfer with pre-enabled TLS on many sources.  This guide details configuring TLS and enabling mutual TLS (mTLS) for additional sources.

> For detailed instructions on configuring certificates to send Palo Alto logs to Cribl.Cloud via Syslog, refer to the guide: [Palo Alto Syslog to Cribl.Cloud](/stream/usecase-syslog-cloud).
{.box .success}

## TLS in Cribl.Cloud {#tls-cloud}

TLS encryption is pre-enabled on several Sources in Cribl.Cloud, as indicated on the **Data Sources** tab of the Cribl.Cloud portal.
All TLS is terminated by individual Nodes. 

1. Log in to your Cribl.Cloud portal.
2. From the top nav, select **Workspace**. 
3. In the sidebar, select **Data Sources**.

While TLS encryption ends at individual Nodes upon data arrival in Cribl.Cloud, data at rest is protected by other robust security measures within the platform.

### Add TLS to Additional Sources

When configuring TLS for Sources that don't have encryption enabled by default, use these [environment variables](environment-variables#criblcloud-certificate-variables) to populate the **Private key path** and **Certificate path** fields in the Source configuration modal.

| Name | Purpose |
|------|---------|
| `CRIBL_CLOUD_KEY` | Path to the default private key on Cribl.Cloud. |
| `CRIBL_CLOUD_CRT` | Path to the default certificate on Cribl.Cloud. |

## Enable mTLS Authentication on Cribl.Cloud {#mutual-tls-cloud}

In Cribl.Cloud, you configure mTLS authentication separately for each Source.

This requires a CA certificate chain that can validate the client certificate used for authentication. For secure communication using mTLS authentication in Cribl.Cloud, you need to provide the root CA certificate of your client certificate. If you don't have the root CA certificate, you can also provide a CA certificate chain that includes all intermediate certificates leading back to a trusted root certificate.

You can add your CA certificate by creating a new certificate entry in Cribl.Cloud. 

1. Prepare the CA certificate chain PEM file.
2. Go to the Worker Node: **Settings** > **Global** > **Security** > **Certificates** and select **Add Certificate**.
3. Populate the **Certificate** field with a valid PEM-formatted content (from your trusted CA certificate chain):

   ```
   -----BEGIN CERTIFICATE-----
   CERTIFICATE CONTENT
   -----END CERTIFICATE-----
   ```

4. Populate the **Private key** with the key in PEM format:

   ```
   -----BEGIN RSA PRIVATE KEY-----
   HIDDEN PRIVATE KEY
   -----END RSA PRIVATE KEY-----
   ```

5. In the **CA certificate** field, enter your PEM-formatted certificate and save.
   This will generate the CA certificate path.

> While the **Add Certificate** form requires you to provide a Private Key and Certificate, these fields are not necessary for
> client certificate validation. This is a design limitation that requires you to provide some properly formatted values to proceed. You can use a self-signed certificate for this purpose.
>
{.box .info}

1. Edit the certificate again to view the certificate path and copy it.
2. Go to the Source where you want to enable mTLS and paste the path in the **CA certificate path** field.
3. Save, commit, and deploy to finish the process.