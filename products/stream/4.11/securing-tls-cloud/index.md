# Securing Cribl.Cloud with TLS and Mutual TLS


Secure data transmission is essential for protecting sensitive information as it travels across networks. Cribl.Cloud offers two primary methods for securing data in transit:

- **Transport Layer Security (TLS)** - Encrypts data during transmission and authenticates the server.
- **Mutual TLS (mTLS)** - Adds client authentication on top of standard TLS.

For encrypting data in transit and simple server verification, Standard TLS is sufficient. Opt for mTLS when security policies mandate additional client verification.

This guide explains both security methods and walks you through configuration options in Cribl.Cloud.

## TLS in Cribl.Cloud {#tls-cloud}

TLS encryption is pre-enabled on several Sources in Cribl.Cloud. The Sources include Cribl TCP, Cribl HTTP, Syslog, HTTP, Splunk TCP, and others. To view the pre-enabled Sources with their corresponding ports and ingress addresses: 

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

### Use Cribl-Provided Certificates (Recommended)

In Cribl.Cloud, we provide Cribl certificates for several Sources by default to ensure your data is protected. These certificates are automatically generated based on your Cloud instance's name, making them unique to your environment.

#### Benefits of Using Cribl-Provided Certificates

- **Pre-configured security**: TLS encryption is ready to use without additional setup.
- **Zero maintenance**: Certificates are automatically managed and renewed.
- **Simplified deployment**: No need to generate or install custom certificates.
- **Reliable security**: Ensures data in transit is always encrypted.

> You only need to use your own private key or configure mTLS if specifically required by your security team or company policies.
>
{.box .info}

#### Example: Enable TLS for a Syslog Source

Follow these steps to enable TLS for your Syslog Source in Cribl.Cloud. These steps are applicable for other Sources with pre-enabled TLS encryption.

1. Navigate to **Sources** > **Syslog**.
2. Select **Add New** or select an existing Syslog Source to edit.
3. In the sidebar, select **TLS Settings**.
4. Toggle **Enable TLS** to `Yes`. 
5. Accept the default certificate values provided by Cribl.
6. Select **Save**, then **Commit** & **Deploy**.

![Default TLS Settings in Syslog Source](syslog-tls-settings.png)
{scale="90%" border="true"}

Syslog TLS in Cribl.Cloud is enabled on port `6514`. 

 For details on how to securely forward syslog data to your Cribl.Cloud instance, see [Syslog TLS to Cribl.Cloud (Palo Alto Example)](/stream/usecase-syslog-cloud).This topic demonstrates a vendor-specific configuration (Palo Alto) for secure data ingestion.

### Use Custom Certificates (Optional)

For organizations needing custom certificates, see [Setting Authentication on Sources/Destinations](securing-sources-dest#worker-group-certs).

## Enable mTLS Authentication on Cribl.Cloud {#mutual-tls-cloud}

When standard TLS isn't sufficient and you need client authentication, you can enable mTLS. 

### Prerequisites for mTLS

mTLS requires:
- A CA certificate chain that can validate the client certificates.
- Client certificates issued to each client that will connect to Cribl.Cloud.
- Client systems configured to present their certificates during connection.

In Cribl.Cloud, you configure mTLS authentication separately for each Source.

This requires a CA certificate chain that can validate the client certificate used for authentication. For secure communication using mTLS authentication in Cribl.Cloud, you need to provide the root CA certificate of your client certificate. If you don't have the root CA certificate, you can also provide a CA certificate chain that includes all intermediate certificates leading back to a trusted root certificate.

### Configure mTLS on a Source

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

### Apply the CA Certificate to Your Source:

1. Edit the certificate again to view the certificate path and copy it.
2. Navigate to the Source where you want to enable mTLS and open the configuration modal. In the **TLS Settings** tab:
   1. Toggle **Enable TLS** to `Yes`.
   2. Set **Authenticate Client** to `Yes`.
   3. Paste the path in the **CA certificate path** field.
   4. Select **Save**, then **Commit** & **Deploy**.
