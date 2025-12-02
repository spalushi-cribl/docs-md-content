# Securing On-Prem and Hybrid Deployments

This document outlines high-level security best practices for deploying Cribl Stream and Cribl Edge in on-premises or hybrid environments. Following these guidelines helps safeguard your deployment and supports compliance with your organization’s security policies.

## Secure Installation and Environment

A secure deployment begins with a fortified foundation. This section outlines how to configure your Cribl environment to minimize your attack surface from the ground up.

| Best Practice | Description |
| :---- | :---- |
| **Isolate Your Deployment** | Deploy Cribl on a dedicated network segment to minimize exposure to unauthorized access. |
| **Restrict Physical Access** | Limit physical access to the hardware running Cribl to authorized personnel only. |
| **Create a Dedicated User** | Avoid running Cribl as a root user. Instead, create a dedicated user account with limited permissions. For details, see [Running Edge as an Unprivileged User](/edge/deploy-runtime-user). |
| **FIPS Mode** | If required, enable FIPS mode *before* starting Cribl Stream/Cribl Edge. You cannot enable it later. For details, see FIPS Mode for [Cribl Stream](/stream/fips-mode/) and [Cribl Edge](/edge/fips-mode/). |


## Authentication and Access Control

Protecting your data requires strict control over who can access your system and what they can do. These guidelines focus on managing user identities and permissions within your Cribl deployment.

| Best Practice | Description & Link |
| :---- | :---- |
| **Change Default Admin Password** | Immediately update the default administrator password to a strong, unique value after installation. |
| **Role-Based Access Control (RBAC)** | Use RBAC to define and enforce fine-grained permissions for users and groups. For details, see [Members](/stream/members/) and [Permissions](/stream/permissions/).|
| **Secure Leader Auth Token** | Use a strong, unique authentication token for the Leader Node. For details, see [How to Secure the Auth Token for the Leader Node](/stream/securing-auth-token/). |
| **Prevent Direct UI Access** | In distributed deployments, restrict direct browser access to Worker or Edge Node UIs. Use the Leader Node for centralized management. For details, see [Secure Access to Worker Node UIs](/stream/securing-communications/#ui-access). |


## Identity and Single Sign-On (SSO)

We strongly recommend using an external IDP to centralize user authentication and authorization. For details, see:

* [SSO with Okta and OIDC (on-prem)](/edge/usecase-sso-okta)  
* [SSO with Okta and SAML](/edge/usecase-sso-saml/)    
* [SSO with Microsoft Entra ID (On-Prem)](/edge/sso-microsoft-entra-id-on-prem)


## Communication and Data Security

Data is constantly in motion, and it needs protection every step of the way. These guidelines focus on encrypting data in transit and securing communication channels between all Cribl components and external systems.

| Best Practice | Description & Link |
| :---- | :---- |
| **Encrypt Inbound Communication** | To safeguard inbound communications to your Worker/Edge Nodes, configure Transport Layer Security (TLS) on the Leader. For details, see: <br> <ul><li> [Configure TLS for API and UI Access](/edge/securing-ssl) </li><li>  [TLS version support and defaults](/edge/securing-tls-overview#defaults) </li><li>  [TLS settings and Traffic Types](/edge/securing-tls-overview#tls)</li><li> [System-wide TLS Settings Including Ciphers​](/edge/securing-tls-overview#ciphers). |
| **Data in Motion** | For extra protection, you can encrypt data as it travels between Cribl and your data Sources and Destinations. TLS is the recommended method as it encrypts the entire data stream. For details, see  [Secure Sources and Destinations with Certificates](/stream/securing-sources-dest/) |
| **TLS Mutual Authentication** | Configure client certificate exchange to allow only authorized clients to connect to the Leader Node. For details, see [Configuring TLS Mutual Authentication](/stream/securing-mtls-onprem/). |
| **System Proxy** | Use HTTP/HTTPS or SOCKS proxies to secure and manage outbound and inter-node communication. For details, see  [System Proxy Configuration](/stream/proxy-config/). |
| **Data at Rest** | You are responsible for implementing encryption on the systems hosting Cribl. Consider using filesystem-level or hardware-level encryption. |
| **Filter Sensitive Information** | Use the sensitiveFields property in cribl.yml to filter sensitive information from API responses. For details, see  [API reference](/api-reference/) and [Custom HTTP Headers](/stream/securing-sources-dest/#custom). |

## Configuration and Secrets Management

Your Cribl configurations and credentials are the keys to your entire deployment. Learn how to manage them securely to prevent unauthorized access and enable robust disaster recovery.

| Best Practice | Description & Link |
| :---- | :---- |
| **Private Git Repositories** | Always use a private remote Git repository to back up and manage your configurations. For details, see  [GitOps](/stream/gitops/). |
| **Manage Secrets and Keys** | Use the Cribl secrets store to securely manage authentication tokens, keys, and API credentials. For details, see  [Secrets Management](/stream/manage-secrets-and-keys/).|
| **Key Management Service (KMS)** | Use Cribl’s built-in KMS or integrate an external KMS to securely manage encryption keys. For details, see [Configure KMS](/stream/securing-kms-config/). |


