# Cribl.Cloud Government vs. Commercial

> The following draft provides early access to the Cribl.Cloud Government product release. Features or functionality described are not considered binding commitments and are subject to change at the discretion of Cribl at any time for any reason without notice. This information should not be relied upon in making purchasing decisions.
{.box .info}

This document compares the Cribl.Cloud commercial and government offerings, highlighting key differences in features, security, and administrative considerations.

## Feature Comparison: Cribl.Cloud Government vs. Commercial

| Feature/Topic | Cribl.Cloud Government | Cribl.Cloud Commercial |
| :--- | :--- | :--- |
| **Available products** | Full Cribl.Cloud Product Suite, **excluding** all AI features ([AI functionality embedded within Cribl Guard](/fed_features#guard) and the Cribl Copilot AI feature). | Full Cribl.Cloud Product Suite, **including** Cribl Guard AI features and Cribl Copilot.|
| **Authentication** | Two-factor authentication (2FA) is **required for all logins**. You can use your own single sign-on (SSO) as you would with the commercial version. | Two-factor authentication (2FA) and SSO are available. |
| **Email notifications** | **Via agency-controlled SMTP.** | **Via commercial email delivery service.** |
| **Deployment & data residency** | Services are hosted within FedRAMP-approved **AWS US East/West Regions.** All data actively being processed or stored remains securely within these regions. | **Global regional deployment options** are available. |
| [**Worker Groups**](/stream/deploy-distributed/#concepts) | Only Cribl-managed Worker Groups on **AWS** are supported. Cribl-managed Worker Groups on **Azure are not available.** | Cribl-managed Worker Groups on both **AWS and Azure** are available. |
| **Cribl Stream Sources** | **Require explicit activation** for enhanced security. | **Enabled by default.** |

## Security and Compliance

| Feature/Topic | Cribl.Cloud Government | Cribl.Cloud Commercial |
| :--- | :--- | :--- |
| **Compliance level** | Authorized at the **FedRAMP Moderate Control** level. | Standard commercial security. |
| **Cryptographic standards** | **Enforces FIPS 140-3** cryptographic standards. | **Does not enforce FIPS 140-3.** |
| **U.S. persons** | Access to the production environment and Federal customer support is limited to **U.S. persons** as defined by NIST. | **No such limitations on personnel.** |
| **Technical support** | Provided by Cribl technical support engineers based in the U.S. Business hours are 9am PST - 9pm ET.| Standard commercial support. |

## Administrative Considerations

This section provides additional details for Federal administrators to keep in mind when working with Cribl.Cloud Government.

* **Procurement**: You cannot purchase Cribl.Cloud Government **directly through the AWS Marketplace.** For procurement inquiries, contact your dedicated Cribl Federal account team.
* **Support channels**: While we use a support process that meets compliance standards, certain risk-managed choices allow us to use the [standard support channels](/support/).
* **Public status page**: We continue to use the **public status page** to provide real-time service updates for both commercial and government customers.