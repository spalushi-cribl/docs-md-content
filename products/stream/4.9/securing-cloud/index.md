# Secure Your Cribl.Cloud Deployment


Cribl.Cloud is SOC 2 Type II–certified and GDPR-compliant, simplifying compliance with industry standards and regulatory requirements.
To secure your Cribl.Cloud deployment, consider implementing the following.

## Secure Your Cribl.Cloud Account

Create a strong password during your initial setup.

To create a secure password, ensure it is at least 12 characters long and includes a combination of lowercase letters, uppercase letters, numbers, and special characters (!@#$%^&*). Follow the on-screen instructions carefully to meet these requirements.
 
 (For Cribl.Cloud setup instructions, see [Initial Cribl.Cloud Setup](cloud-initial-setup)).

## Review Your Network Settings
When you log into your Cribl.Cloud Organization, navigate to the **Workspace** link. Here you can check and manage connectivity details – [Data Sources](workspaces#view-data-sources), [Access Control](workspaces#set-up-acl), and [Trust](workspaces#get-arn) relationships – for your Cribl-managed Workers in Cribl.Cloud.

On the **Data Sources** tab, you can review the available ports and TLS configurations. 

Note that TLS encryption is pre-enabled for you on some, but not all Sources. For more information, see [TLS in Cribl.Cloud](securing-tls-cloud). 

## Configure TLS Mutual Authentication on Cribl.Cloud

Within Cribl.Cloud, enabling mutual TLS authentication for individual data sources requires a trusted Certificate Authority (CA) certificate chain. This CA certificate verifies the legitimacy of the client certificate presented by the source during connection. For details, see [TLS Mutual Authentication in Cribl.Cloud](securing-tls-cloud#mutual-tls-cloud).

## Set Up Role-Based Access Control for Members 

Cribl.Cloud offers detailed role-based access control (RBAC) to ensure appropriate permissions and security. For details on how to invite and manage the roles for Members of your Organization, see [Managing Cribl.Cloud](members#invite-members).

## Set Up Your Preferred Authentication Method

Cribl.Cloud with [certain plan tiers](https://cribl.io/pricing/) offers a range of secure and convenient login options to fit your organization's needs. Options include: 

- Local Accounts – Create traditional usernames and passwords for direct login. For details, see [Local Authentication](authentication#local-authentication).
- Federated Authentication (OIDC and SAML): Integrate with existing identity providers (IdPs) like Okta or Entra ID for centralized user management and single sign-on (SSO). For details, see [Cribl.Cloud SSO Setup](cloud-sso). 

## Secure Data in Motion

Cribl.Cloud provides built-in encryption for data at rest, ensuring that your information remains secure within its infrastructure. For an extra layer of protection, you can enrypt individual fields within events. For details, see [Data Encryption](/stream/securing-data-encryption/).

Currently, Cribl Stream supports decryption only when Splunk is the end system. For details, see [Data Decryption](/stream/securing-data-decryption).

For a comprehensive solution, we recommend configuring TLS. TLS encrypts the entire data stream, including all event fields, regardless of Source or Destination type. This provides a more robust security posture for your data in motion. For details, see [Secure Cribl Stream Sources and Destinations with Certificates](securing-sources-dest).

## Configure a Key Management Service (KMS)

Cribl.Cloud with [certain plan tiers](https://cribl.io/pricing/) offers a Key Management Service (KMS) that strengthens the security of your data. KMS securely manages the encryption keys used to protect sensitive information on Worker Groups and Nodes within Cribl.
For details on how to activate and configure KMS, see [KMS Configuration](securing-kms-config). 

## (Optional) Segment Your Organization with Workspaces

Segment  your organization's data and user access using Workspaces. This multi-tenancy feature allows you to create isolated instances within your Cribl.Cloud Organization, addressing critical security, compliance, and data isolation requirements tailored for diverse business units.

Each Workspace boasts a dedicated virtual private cloud (VPC). This essentially creates a separate environment within your Organization for each Workspace, ensuring the complete isolation of data and users in different Workspaces.

For details, see [Configuring Workspaces](workspaces-configuring).

## (Optional) Create Isolated Data Environments Using Stream Projects 

Within Cribl Stream, Projects offer a more granular level of isolation allowing teams and users to manage and share data flows without impacting others. Projects enable you to define the data that the Project’s users can consume, where that data can be sent, and who has access to the Project.

Projects are ideal for teams and users who need to share and manage specific data, with accelerated access to relevant data and minimal configuration requirements. Data can be forked into full-fidelity versus redacted views to match the authorzation requirements and needs of different teams or users.

For details, see [Configuring Projects](/stream/configuring-projects/).
