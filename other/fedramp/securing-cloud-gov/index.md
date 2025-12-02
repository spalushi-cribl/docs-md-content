# Secure Your Cribl.Cloud Government Deployment

> The following draft provides early access to the Cribl.Cloud Government product release. Features or functionality described are not considered binding commitments and are subject to change at the discretion of Cribl at any time for any reason without notice. This information should not be relied upon in making purchasing decisions.
{.box .info}

To ensure the robust security of your Cribl.Cloud Government deployment and maximize its benefits, consider implementing the following best practices and features.

For a foundational understanding, start with our [Cribl.Cloud Security Practices](/reference-architectures/arch-cloud-security-practices/), and specifically, [Cribl.Cloud/Customer Responsibilities](/reference-architectures/arch-cloud-security-responsibility/).

> The linked tutorials and guides will help orient you to our Cribl Product Suite. Cribl.Cloud Government offers comparable features to our commercial platform, and these guides are applicable to both environments.
{.box .info}

## Secure Your Cribl.Cloud Account

Create a strong password during your initial setup.

To create a secure password, ensure it is at least 15 characters long and includes a combination of lowercase letters, uppercase letters, numbers, and special characters (!@#$%^&*). Follow the on-screen instructions carefully to meet these requirements.

##  Use Secure Naming Conventions for DNS Resources

DNS names are exposed in network traffic and logs, so avoid including sensitive information in resource names. When creating Workspaces and Worker Groups/Fleets in Cribl.Cloud Government, be mindful that these resources generate public DNS names. To maintain security and prevent inadvertent disclosure of sensitive information:

- Avoid using sensitive internal data in Workspace names, Worker Group names, or Fleet names.
- Do not include confidential project names, internal system identifiers, or sensitive information in resource names.
- Use generic, non-descriptive naming conventions that do not reveal organizational structure or sensitive operations.

## Review Your Network Settings
When you log in to your Cribl.Cloud Organization, navigate to the **Workspace** link. Here you can check and manage connectivity details – [Data Sources](/stream/workspaces#view-data-sources), [Access Control](/stream/workspaces#set-up-acl), and [Trust](/stream/workspaces#get-arn) relationships – for your Cribl-managed Workers in Cribl.Cloud.

On the **Data Sources** tab, you can review the available ports and TLS configurations. 

Note that TLS encryption is pre-enabled for you on some Sources, but not all. For more information, see [Securing Cribl.Cloud with TLS and Mutual TLS](/stream/securing-tls-cloud). 

## Configure Mutual TLS Authentication on Cribl.Cloud

Within Cribl.Cloud, enabling mutual TLS authentication for individual data sources requires a trusted Certificate Authority (CA) certificate chain. This CA certificate verifies the legitimacy of the client certificate presented by the Source during connection. For details, see [Enable mTLS Authentication on Cribl.Cloud](/stream/securing-tls-cloud#mutual-tls-cloud).

## Set Up Role-Based Access Control for Members 

Cribl.Cloud offers detailed role-based access control (RBAC) to ensure appropriate permissions and security. To set up Permissions and invite and manage the Roles for Members of your Organization, see [Permissions](/stream/permissions/) and [Members](/stream/members/).


## Set Up Your Authentication Method

Integrate with existing identity providers (IdPs) for centralized user management and single sign-on (SSO). For details, see [SSO on Cribl.Cloud](/stream/cloud-sso). 

## Data at Rest

Cribl.Cloud Government provides built-in encryption for data at rest, ensuring that your information remains secure within its infrastructure. 

## Secure Data in Motion

For an extra layer of protection, you can enrypt individual fields within events. For details, see [Encryption of Data in Motion](/stream/securing-data-encryption/).

Currently, Cribl Stream/Cribl Edge supports decryption only when Splunk is the end system. For details, see [Decryption of Data in Splunk](/stream/securing-data-decryption).

For a comprehensive solution, we recommend configuring TLS. TLS encrypts the entire data stream, including all event fields, regardless of Source or Destination type. This provides a more robust security posture for your data in motion. For details, see [Secure Sources and Destinations with Certificates](/stream/securing-sources-dest).

## Configure a Key Management Service (KMS)

Cribl.Cloud Government offers a Key Management Service (KMS) that strengthens the security of your data. KMS securely manages the encryption keys used to protect sensitive information on Worker Groups and Nodes within Cribl.
To activate and configure KMS, see [Configure KMS Providers](/stream/securing-kms-config).  

## (Optional) Segment Your Organization with Workspaces

Segment  your Organization's data and user access using Workspaces. This multi-tenancy feature allows you to create isolated instances within your Cribl.Cloud Organization, addressing critical security, compliance, and data isolation requirements tailored for diverse business units.

Each Workspace boasts a dedicated virtual private cloud (VPC). This essentially creates a separate environment within your Organization for each Workspace, ensuring the complete isolation of data and users in different Workspaces.

For details, see [Configuring Workspaces](/stream/workspaces-configuring).

## (Optional) Create Isolated Data Environments Using Stream Projects 

Within Cribl Stream, Projects offer a more granular level of isolation allowing teams and users to manage and share data flows without impacting others. Projects enable you to define the data that the Project’s users can consume, where that data can be sent, and who has access to the Project.

Projects are ideal for teams and users who need to share and manage specific data, with accelerated access to relevant data and minimal configuration requirements. Data can be forked into full-fidelity versus redacted views to match the authorization requirements and needs of different teams or users.

For details, see [Configure Projects](/stream/configuring-projects/).