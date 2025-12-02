# SSO with Microsoft Entra ID (Cribl.Cloud)


This topic provides details to help you configure Single Sign-On (SSO) with Microsoft Entra ID as the identity provider (IdP).

To configure Microsoft Entra ID as an IdP, refer to the [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/).

> This page describes how to configure SSO for Cribl.Cloud. For on-prem installations, see [SSO with Microsoft Entra ID (On-Prem)](sso-microsoft-entra-id-on-prem).
{.box .info}


 
## Set Up Fallback Access {#fallback-access-cloud}

Before you configure SSO, create a fallback user so that you aren't locked out of your Organization if you have issues with SSO. In your Cribl.Cloud Organization, [invite a new Member](members#invite-members) using an email domain that’s different from the corporate domain on which you’re configuring SSO. Assign the [Owner](permissions#organization) Permission for the Member. You can use this account to log in with a username and password and fix SSO issues if needed.

> After you confirm that your SSO integration is working, you can remove the fallback user. If you do so, do not disable the SSO integration without first re-creating a fallback user. Otherwise, you might get locked out of your Organization.
>
{.box .warning}


## Limitations

Cribl offers a service provider-initiated (Cribl-initiated) workflow, but does not support an IdP-initiated SSO flow. To allow users to initiate login from the IdP instance, [create a chiclet](#my-apps-chiclet).

## SAML SSO with Microsoft Entra ID {#saml-entra-id-cloud}

> This section provides SAML SSO configuration details that are specific to using Microsoft Entra ID as the IdP. For general step-by-step procedures, read [Configure SAML SSO](#configure-saml-cloud).
{.box .info}

Configuring SSO in Cribl.Cloud requires a SAML 2.0 application in Microsoft Entra ID. Read [Quickstart: Add an enterprise application](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/add-application-portal) and [Overview of the Microsoft Entra application gallery](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/overview-application-gallery) to learn how to create an application in Microsoft Entra ID. Make sure that the SAML application includes at least one user so that you can test the configuration.

As you [configure SAML SSO](#configure-saml-cloud), provide the values from the following fields in Cribl.Cloud:

| Field in Cribl.Cloud | Field in Microsoft Entra ID |
| -------------------- | --------------------------- |
| **Single Sign on URL** | **Reply URL (Assertion Consumer Service URL)** |
| **Audience URI** | **Identifier (Entity ID)** |

When you add the **Single Sign‑on URL** values from Cribl.Cloud to **Reply URL (Assertion Consumer Service URL)**, select the **Default** checkbox for the URL that includes `connection`.

You must also add Cribl.Cloud [groups](sso-initial-steps) to the SAML application according to the [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/users/groups-saasapps#assign-access-for-a-user-or-group-to-a-saas-application). Do not configure roles in Microsoft Entra ID. Instead, Cribl groups will manage access control.

[Customize the group claim names](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-fed-group-claims#customize-group-claim-name) in Microsoft Entra ID to make sure that they match the group names in Cribl.Cloud: 

| Name in Microsoft Entra ID | Name in Cribl.Cloud |
| -------------------------- | ------------------- |
| `surname` | `family_name` |
| `emailaddress` | `email` |
| `givenname` | `given_name` |

To configure the group claim to [include the group display name for the cloud-only groups](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-fed-group-claims?WT.mc_id=AZ-MVP-5003833#emit-cloud-only-group-display-name-in-token), select **Groups assigned to the application** and set the **Source attribute** to `Cloud‑only group display names`.

> If Microsoft Entra ID is synchronized with external Active Directory, select **Groups assigned to the application** and set the **Source attribute** to `sAMAccountName`. Otherwise, Microsoft Entra ID will return only Globally Unique Identifiers (GUIDs).
> 
> Enable **Emit group name for cloud-only groups** to return the group names if Microsoft Entra ID is defaulting to GUID or Object ID.
{.box .warning}

You will need the values from the following fields in the SAML application to finish SSO setup in Cribl.Cloud:

| Field in Microsoft Entra ID | Field in Cribl.Cloud |
| --------------------------- | -------------------- |
| **Login URL** | **IDP Login/Logout URL** |
| **Microsoft Entra ID Identifier** | **IDP issuer** |
| **Signing Certificate** | **Certificate (Base64)** under **SAML Certificates** |

> The field names and documentation for adding a SAML application and configuring groups might change without notice due to product changes in Microsoft Entra ID. Refer to the [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/) for the latest information.
{.box .info}

## General SSO Configuration in Cribl.Cloud {#general-sso-cloud}

The procedure in this section generally describes how to configure SAML SSO in Cribl.Cloud using any IdP.

> The registration process uses the email domain of the authenticated user who configures SSO in Cribl.Cloud, regardless of the domain that is specified in the IdP configuration.
{.box .info}



### Configure SAML SSO {#configure-saml-cloud}

1. In Cribl.Cloud, in the sidebar, select **Organization > SSO Management**.

1. Scroll down to the end of the **Product-Level Mappings** and select **SAML**.

1. In the IdP, create the SAML application. Use the information from Cribl.Cloud under **Web Application Settings** and **SAML Assertion Mappings**:

   * **Web Application Settings > Single Sign on URL** lists two URLs:
      * `https://login.cribl.cloud/login/callback?connection=<organizationId>` is the Assertion Consumer Service (ACS) URL. This is the endpoint that receives and processes authentication responses from the IdP.
      * `https://manage.cribl.cloud/api/assert` is a testing URL to use during setup. After you successfully test the connection, replace this URL with `https://login.cribl.cloud/login/callback?connection=<organizationId>` in the SAML application.

   * **Web Application Settings > Audience URI**: The SAML entity ID for Cribl.Cloud. The Audience URI is a unique string that identifies Cribl.Cloud in SAML assertions. The IdP uses the Audience URI to specify the intended recipient of the assertion and prevent replay attacks.

   * **SAML Assertion Mappings** define the attribute names that Cribl.Cloud must receive in the SAML assertions from the IdP to correctly provision and authorize users:
      * `email`: The user's email address (used as the user's unique identifier).
      * `given_name`: The user's first name.
      * `family_name`: The user's last name.
      * `groups`: The user's group memberships (used for role-based access control and Team assignments). Read [Configuring SSO Groups](sso-initial-steps) for information about valid IdP group names and Permission mapping.

1. After you create and save the SAML application in the IdP, return to Cribl.Cloud to finish setting up SAML SSO. Scroll down to **SAML configuration** and enter the following information from the SAML application in the IdP:

   * **IDP Login/Logout URL**: The SAML SSO endpoint URL for the IdP, where Cribl.Cloud should send SAML authentication requests. If the IdP supports SAML Single Logout (SLO) at the same endpoint, Cribl will use this URL for both login and logout flows.

   * **IDP issuer**: The SAML entity ID for the IdP. The IdP issuer is a unique string (often a URI or URL) that identifies the IdP in SAML assertions. Cribl uses the IdP issuer to validate the authenticity of SAML responses.

   * **X.509 certificate (base64-encoded)**: The public certificate that the IdP uses to sign SAML responses. Paste the entire PEM-encoded certificate, including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines.

1. Select **Save**.




## Verify that SSO Is Working {#verify-cloud}

1. In Cribl.Cloud, in the sidebar, select **Organization > SSO Management**.

1. Scroll to the bottom of the page and select **Test Connection**.

If the test encounters a configuration error, Crib.Cloud will display an error message.


## SAML/Entra ID Setup with My Apps Chiclet (Optional) {#my-apps-chiclet}

If you want to log into Cribl.Cloud via the [Microsoft My Apps](https://myapps.microsoft.com/) chiclet, complete the following procedure:
1. In Microsoft Entra ID, navigate to the enterprise application that you created to integrate SSO.
1. From the left nav, select **Single Sign-on**.
1. On the Enterprise Application's **Basic SAML Configurations** page, select **Edit**.
1. In the **Sign on URL (Optional)** section, enter the following URL:

    `https://manage.cribl.cloud/login?connection=<organizationId>` (where `<organizationId>` is your Cribl.Cloud Organization’s ID).

You also need to allow [self-service access](https://learn.microsoft.com/en-us/azure/active-directory/manage-apps/manage-self-service-access) to the Cribl App,
or [assign AD groups permissions](https://learn.microsoft.com/en-us/azure/active-directory/enterprise-users/groups-self-service-management) to access the application.

## Troubleshooting {#troubleshooting-cloud}

If you encounter issues when setting up SSO, refer to [SSO Troubleshooting in Cribl.Cloud](sso-troubleshooting-cloud).

