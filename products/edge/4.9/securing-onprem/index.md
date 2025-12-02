# Secure Your On-Prem/Hybrid Deployment


This document outlines essential security considerations to safeguard your on-prem/hybrid Cribl deployment.

## Secure Your Installation

Secure your Cribl Edge deployment by ensuring the following during installation.

### Choose a Secure Environment

Protect your Cribl deployment by: 

* **Isolating your deployment** – deploy Cribl Edge on a dedicated network segment to minimize exposure.  
* **Restricting physical access** – control who can physically access the deployment hardware.

### Configure a System Proxy 

Enhance the security and reliability of your Cribl deployment by leveraging proxy servers.

- To configure a system proxy for outbound HTTP/S traffic: Set the HTTP\_PROXY and HTTPS\_PROXY environment variables before starting Cribl Edge, specifying the proxy's IP address or FQDN and optionally the port number. For details, see [System Proxy Configuration](proxy-config).

- To configure a SOCKS proxy for secure communication between Leader and managed Nodes: Follow the steps in [Configure a SOCKS proxy](proxy-socks-config). This will allow you to bypass network restrictions and ensure uninterrupted data flow.

### Create a Dedicated User

To enhance security, avoid running Cribl Edge  with root privileges. Instead, create a dedicated user account with limited permissions.

For details, see [Setting Capabilities for Cribl Stream](/stream/deploy-distributed#setting-capabilities-for-cribl-stream/) or [Running Edge as an Unprivileged User](/edge/deploy-runtime-user/).

### Change Default Admin Password

After your initial login, immediately create a strong, unique password for the administrator account. This will help prevent unauthorized access.

**Note:** Edge Node passwords are managed by the Leader Node.


### Locate and Access Your Cribl Secrets

When you install Cribl Edge, the following security files are created:

* `cribl.secret` – This file stores a unique cryptographic key used for authentication, password encryption, and key encryption. It's essential for securing your Cribl Edge deployment when authenticating with integrated services.  
* `keys.json` – This file contains cryptographic keys used for internal operations within Cribl Edge.

These files are stored in: 

**Single-instance deployment** – both files are located in `$CRIBL_HOME/local/cribl/auth/`.

**Distributed deployment** – both files reside on the Leader Node, in `$CRIBL_HOME/groups/<group‑name>/local/cribl/auth/`.

You can use the **Settings** \> **Security \> Secrets** section to create and update authorization tokens, username/password combinations, and API‑key/secret‑key combinations for reuse across the application. For details, see [Secrets](securing-secrets). 

## Set Your Remote Git Repo to Private 

Cribl uses Git for efficient configuration management. In Distributed deployments, a local Git repository tracks configuration changes. For added security and disaster recovery, you can also connect to a remote Git repository. Here are some key considerations.

* **Private repositories**  – Always ensure that your remote Git repository is private, to prevent unauthorized access to your configuration files.  
* **Disaster recovery**  – Remote repositories provide a valuable backup for your Cribl configuration in case of system failures or data loss.

## FIPS Mode (Cribl Stream only)

Do not start Cribl Stream without FIPS mode enabled, or else you will be unable to run it in FIPS mode later. You must enable FIPS mode as described in the [FIPS Mode](/stream/fips-mode/) document, after installing **but before starting** Cribl Stream. 

## Secure the Leader Auth Token 

Cribl Edge generates a strong, random authentication token for new installations and when transitioning from Single-instance to Distributed mode.

If you need to change the default token:

* Create a unique, secure value.  
* Update the token in the UI or Docker Compose file.

For details, see [How to Secure the Auth Token for the Leader Node](securing-auth-token).

## Set up Role-Based Access Control for Members 

Cribl offers detailed role-based access control (RBAC) to ensure appropriate permissions and security. For details on how to define and manage fine-grained access control across Cribl products and resources, see [Members and Permissions](members).

## Enhance Security with an Identity Provider (IDP)

We strongly recommend using an external Identity Provider (IDP) to implement single sign-on (SSO) for Cribl. This enhances your organization's security, by centralizing user authentication and authorization.

For details, see:
- [SSO with Okta and OIDC (on-prem)](usecase-sso-okta)
- [SSO with Okta and SAML](saml-okta-setup)
- [SSO with Microsoft Entra ID and SAML (On-Prem)](/stream/usecase-sso-saml-entra/)
- [SSO with Microsoft Entra ID and OIDC (On-Prem)](/stream/usecase-azure-ad/)

## Secure Your Communication

Secure your Cribl communication channels by using the following options.

* **SSL/TLS**  – Encrypt Cribl access and traffic with SSL or TLS.    
* **Custom headers**  – Enhance security with custom HTTP headers.  
* **Encryption**  – Create and manage your own keys to encrypt events in real time.  
* **Secrets**  –  Centrally manage secrets that your instances use to authenticate on integrated services.  
* **KMS**  – Use internal or external key management service for advanced key management.

Choose a combination that best aligns with your organization’s security policies. 

### Secure Access to the Leader and Edge Nodes

You can secure access to the Cribl Edge API and UI by configuring  Secure Sockets Layer (SSL) or  Transport Layer Security (TLS). Do this on the Leader, to secure inbound communications to the Edge Nodes.  You can use your own certs and private keys, or you can generate a pair with OpenSSL. For details, see [Configure TLS for API and UI Access](securing-ssl).

For TLS details, see the following:

* [TLS version support and defaults](securing-tls-overview#defaults)  
* [TLS settings and Traffic Types](securing-tls-overview#tls)  
* [System-wide TLS Settings Including Ciphers​](securing-tls-overview#ciphers)

#### Import an Existing Certificate and Key

You can secure Leader-to-Edge Node communications by using an existing TLS/SSL certificate and key, in `\.pem\` format. For details, see [Import Certificate and Key](securing-import-certs#upload-certificate).

#### Create a Self-signed Certificate

You also have the option to create a self-signed certificate and key by following the steps outlined in [Configure SSL](securing-ssl).

### Connect to the Leader Securely 

Configure secure communication between your Leader and Edge Nodes using either the user interface, the `instance.yml` configuration file, or environment variables. For details, see [Secure Leader/Edge Nodes Communications](securing-communications).

### Secure Access to the Worker/Edge Nodes UI

Cribl recommends that in Enterprise Distributed deployments, you prevent direct browser access to the UI for all Worker Nodes. For details, see [Secure Access to Edge Node UIs](securing-communications#ui-access).

### Configure TLS Mutual Authentication (optional) 

Implement client certificate exchange to enhance security by allowing only authorized clients with valid certificates to connect to the Leader. Configure this feature after setting up encrypted TLS communication. For details, see [Configuring TLS Mutual Authentication](securing-mtls-onprem).

## Secure Communication Between Sources and Destinations

To authenticate Cribl Edge with external data senders and receivers, configure certificates at the Fleet level. For details, see [Secure Cribl Edge Sources and Destinations with Certificates](securing-sources-dest).

### Configure Custom HTTP  Headers 

Custom HTTP headers are metadata that can be sent along with HTTP requests and responses. They provide a way to convey extra data that is not part of the standard HTTP protocol. You can customize Cribl's security settings with custom HTTP headers. For details, see [Custom HTTP Headers](securing-sources-dest#http).

## Create Encryption Keys

You can create and manage keys that Cribl Edge will use for real-time encryption of fields and patterns within events. For details, see:

* [Accessing Keys](securing-encryption-keys#keys-access).  
* [Add New Key](securing-encryption-keys#keys-add).  
* [Get Key Bundle](securing-encryption-keys#keys-import).
* Safeguarding Unencrypted Private Key Files for Rollback ([Edge](/edge/upgrading-troubleshooting/#safeguarding)/[Stream](stream/upgrading#safeguarding)).
* [Restoring Unencrypted Private Keys](securing-tls#restoring).

### Configure a Key Management Service (KMS) 

Cribl offers a built-in Key Management Service (KMS) that strengthens the security of your data. With [certain license tiers](https://cribl.io/pricing/), you have the option to integrate an external KMS. A KMS securely manages the encryption keys used to protect sensitive information on Fleets and Edge Nodes within Cribl Edge.

For details, see [Configure KMS](securing-kms-config). 

### Create Secrets

Use the Cribl secrets store to isolate and  simplify the management of authentication tokens, usernames/passwords, and API keys.

* To find out more about secrets, see [The cribl.secret File](authentication#the-criblsecret-file).
* To access secrets, see [Accessing Secrets](securing-secrets#accessing-secrets#get-secret).  
* To add a new secret, see [Add New Secret.](securing-secrets#accessing-secrets#add-secret). 
* For secret types, see [Secret Type](securing-secrets#ssshhh).

## Secure Data in Motion

For an extra layer of protection, consider encrypting your data while it travels between Cribl Edge and your data Sources and Destinations. For details, see [Data Encryption](/stream/securing-data-encryption).

Currently, Cribl Edge supports decryption only when Splunk is the end system. For details, see [Data Decryption](/stream/securing-data-decryption).

For a comprehensive solution, we recommend configuring TLS. TLS encrypts the entire data stream, including all event fields, regardless of Source or Destination type. This provides a more robust security posture for your data in motion. For details, see [Secure Cribl Edge Sources and Destinations with Certificates](securing-sources-dest).


## Disable the Exec Source

The [Exec Source](sources-exec), while powerful, can pose security risks if not configured and used carefully. To mitigate these risks, you can disable the Exec source by setting the `CRIBL_NOEXEC` environment variable. This prevents the execution of arbitrary commands, reducing the attack surface. For details, see [Disable the Exec Source](sources-exec#disable).


