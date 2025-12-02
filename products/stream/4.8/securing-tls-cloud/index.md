# Securing Cribl.Cloud with TLS and Mutual TLS


Cribl.Cloud prioritizes secure data transfer with pre-enabled TLS on many sources.  This guide details configuring TLS and enabling mutual TLS (mTLS) for additional sources.

## TLS in Cribl.Cloud {#tls-cloud}

TLS encryption is pre-enabled on several Sources in Cribl.Cloud, as indicated on the **Data Sources** tab of the Cribl.Cloud portal.
All TLS is terminated by individual Nodes. 

## Enable mTLS Authentication on Cribl.Cloud {#mutual-tls-cloud}

In Cribl.Cloud, you configure mTLS authentication separately for each Source.

This requires a CA certificate chain that can validate the client certificate used for authentication.
You add your CA certificate by creating a new certificate entry in Cribl.Cloud. 

1. Prepare the CA certificate chain PEM file.
2. Go to the Worker Node: **Settings** > **Security** > **Certificates** and select **Add Certificate**.
3. Populate the **Certificate** field with any valid PEM-formatted content:

   ```
   -----BEGIN CERTIFICATE-----
   CERTIFICATE CONTENT
   -----END CERTIFICATE-----
   ```

   The certificate and key are required only for UI validation and are not used otherwise.

4. Populate the **Private key** with the key in PEM format:

   ```
   -----BEGIN RSA PRIVATE KEY-----
   HIDDEN PRIVATE KEY
   -----END RSA PRIVATE KEY-----
   ```

5. In the **CA certificate** field, enter your PEM-formatted certificate and save.
   This will generate the CA certificate path.

6. Edit the certificate again to view the certificate path and copy it.
7. Go to the Source where you want to enable mTLS and paste the path in the **CA certificate path** field.
8. Save, commit, and deploy to finish the process.