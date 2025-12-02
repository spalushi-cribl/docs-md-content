# SSO with Microsoft Entra ID (Cribl.Cloud)


This topic provides details to help you configure Single Sign-On (SSO) with Microsoft Entra ID as the identity provider (IdP).

To configure Microsoft Entra ID as an IdP, refer to the [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/).

> This page describes how to configure SSO for Cribl.Cloud. For on-prem installations, see [SSO with Microsoft Entra ID (On-Prem)](sso-microsoft-entra-id-on-prem).
{.box .info}

[Snippet not found: content/shared/4.14/snippets/_sso-fallback-access-cloud.md]

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

[Snippet not found: content/shared/4.14/snippets/_sso-saml-cloud.md]

[Snippet not found: content/shared/4.14/snippets/_sso-verify-cloud.md]

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

