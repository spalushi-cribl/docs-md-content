# SSO in Cribl.Cloud Government

> The following draft provides early access to the Cribl.Cloud Government product release. Features or functionality described are not considered binding commitments and are subject to change at the discretion of Cribl at any time for any reason without notice. This information should not be relied upon in making purchasing decisions.
{.box .info}

Setting up SSO (single sign-on) for Cribl.Cloud Government imposes specific requirements that differ from the commercial offering. This guide focuses on those key distinctions to help you configure your identity provider correctly.

For detailed, step-by-step instructions on setting up SSO in general, see [Cribl.Cloud SSO Documentation](/stream/cloud-sso/).

## General SSO Differences

Cribl.Cloud Government has stricter SSO controls to comply with Federal standards. A key difference is the handling of user authentication paths:

* **SSO bypass:** Unlike the commercial offering, Cribl.Cloud Government does not support the ability to bypass SSO to access a local user account or register additional organizations. Once SSO is configured, **all users are required to authenticate via the configured SSO provider**. This ensures a single, secure authentication path for all users.

For details, see [Bypass SSO with Multiple Organizations](/stream/sso-cloud/#bypass-sso-multi-orgs-cloud).


### SAML Configuration

The SAML setup process involves a specific user workflow and distinct security requirements for compliance. 

#### Assertion Consumer Service (ACS) URL / Audience URI

For Cribl.Cloud Government, you can't get the final **ACS URL** and **Audience URI** until after you create the SSO connection in the Cribl UI. 

1.  When you first create a SAML connection, the UI will display **temporary, placeholder values**. You must use these dummy values to initially set up the application in your IdP.
2.  After saving the connection, the Cribl.Cloud UI will automatically update with the **real, permanent URLs**. You must then copy these URLs and update your IdP's configuration.

#### SAML Assertion Mapping

The required assertion attributes have different naming conventions compared to the commercial offering. Ensure your IdP is configured to map the following attributes:

| Cribl.Cloud Government Attribute | Description |
| :--- | :--- |
| `email` | The user’s email address, which serves as the unique user identifier. |
| `firstName` | The user’s first name. |
| `lastName` | The user’s last name. |
| `groups` | The user’s group memberships. |

#### Encryption Certificate

Cribl.Cloud Government supports **encrypted SAML assertions** for enhanced security, a feature not available in the commercial offering.

1.  After saving the SSO connection in the Cribl.Cloud UI, an encryption certificate will be available for download.
2.  You must download this certificate and upload it to your IdP's configuration to enable encrypted assertions.

For details, see [Configure SAML SSO](/stream/sso-cloud/#configure-saml-cloud).

### Test Connection

The **Test Connection** feature is available to verify basic connectivity. Due to regulatory restrictions, the verification process may offer fewer diagnostic messages compared to the commercial offering, but the core workflow remains the same.

For details, see [Verify that SSO Is Working](/stream/sso-cloud/#verify-cloud).