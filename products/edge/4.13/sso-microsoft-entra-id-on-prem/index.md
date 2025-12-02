# SSO with Microsoft Entra ID (On-Prem)


This topic provides details to help you configure Single Sign-On (SSO) with Microsoft Entra ID as the identity provider (IdP).

To configure Microsoft Entra ID as an IdP, refer to the [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/).

> This page describes how to configure SSO for on-prem installations. For Cribl.Cloud, see [SSO with Microsoft Entra ID (Cribl.Cloud)](sso-microsoft-entra-id-cloud).
{.box .info}

[Snippet not found: content/shared/4.13/snippets/_sso-fallback-access-on-prem.md]

## OIDC SSO with Microsoft Entra ID {#oidc-entra-id-on-prem}

> This section provides OIDC SSO configuration details that are specific to using Microsoft Entra ID as the IdP. For general step-by-step procedures, read [Configure OIDC SSO](#configure-oidc-on-prem).
{.box .info}

Configuring SSO in Cribl requires creating an OIDC application and a client secret and adding a groups claim in Microsoft Entra ID. Follow the instructions in these pages in the Microsoft Entra ID documentation:

- [Register an application in Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app)
- [Create a new client secret](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal#option-3-create-a-new-client-secret)
- [Configure groups optional claims](https://learn.microsoft.com/en-us/entra/identity-platform/optional-claims?tabs=appui#configure-groups-optional-claims)
 
Make sure that the OIDC application includes at least one user so that you can test the configuration.

When you register your app in Microsoft Entra ID:

- Under **Supported account types**, select **Accounts in this organizational directory only**.
- Under **Redirect URI (optional)**, select the platform **Web** and provide the callback URL for the Leader Node of your Cribl deployment (such as `https://{$hostname}:9000/api/v1/auth/authorization-code/callback`).

When you create the client secret in Microsoft Entra ID, copy the **Value**. You will need it to configure SSO in Cribl. This is the only time the Value is available to copy, so make sure to copy it while it is visible. If you missed your chance, start over by creating a new secret. The Value is sensitive information and should be kept private.

> Synchronize Microsoft Entra ID with your on-prem Active Directory to configure returning `sAMAccountName` for group names. Otherwise, Microsoft Entra ID will return only Globally Unique Identifiers (GUIDs).
> 
> For the groups claim configuration, select **Groups assigned to the application** and set the **Source attribute** to `sAMAccountName`. Enable [**Emit group name for cloud-only groups**](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-fed-group-claims?WT.mc_id=AZ-MVP-5003833#emit-cloud-only-group-display-name-in-token) to return the `sAMAccountName` attribute for cloud-only groups.
{.box .warning}

As you [configure OIDC SSO](#configure-oidc-on-prem), you will need the following values from Microsoft Entra ID to finish SSO setup in Cribl:

| Field in Microsoft Entra ID | Field in Cribl |
| --------------------------- | -------------- |
| **Application (client) ID** | **Client ID** |
| **Value** for the client secret | **Client secret** |
| **OAuth 2.0 authorization endpoint (v2)** | **Authentication URL** |
| **OAuth 2.0 token endpoint (v2)** | **Token URL** |

To use v1 endpoints instead of v2, replace the values for **Authentication URL** and **Token URL** with their v1 equivalents from Microsoft Entra ID. Also, in v1, only the `name` field is included in the token by default, so you must modify the **User identifier** field (for example, the value might be `${unique_name || upn || username || name}`). Check the token fields returned by enabling debug-level logging on Cribl's `auth:sso` channel.

Also, change the **Filter type** selection to `User info filter` in the Cribl OIDC configuration.

> The field names and documentation for adding an OIDC application might change without notice due to product changes in Microsoft Entra ID. Refer to the [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/) for the latest information.
{.box .info}

## SAML SSO with Microsoft Entra ID {#saml-entra-id-on-prem}

> This section provides SAML SSO configuration details that are specific to using Microsoft Entra ID as the IdP. For general step-by-step procedures, read [Configure SAML SSO](#configure-saml-on-prem).
{.box .info}

Configuring SSO in Cribl requires a SAML 2.0 application in Microsoft Entra ID. Read [Quickstart: Add an enterprise application](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/add-application-portal) and [Overview of the Microsoft Entra application gallery](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/overview-application-gallery) in the Microsoft Entra ID documentation for more information. Make sure that the SAML application includes at least one user so that you can test the configuration.

> Synchronize Microsoft Entra ID with your on-prem Active Directory to configure returning `sAMAccountName` for group names. Otherwise, Microsoft Entra ID will return only Globally Unique Identifiers (GUIDs).
> 
> For the [groups claim configuration](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-fed-group-claims?WT.mc_id=AZ-MVP-5003833#add-group-claims-to-tokens-for-saml-applications-using-sso-configuration), select **Groups assigned to the application** and set the **Source attribute** to `sAMAccountName`. Enable [**Emit group name for cloud-only groups**](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-fed-group-claims?WT.mc_id=AZ-MVP-5003833#emit-cloud-only-group-display-name-in-token) to return the `sAMAccountName` attribute for cloud-only groups.
{.box .warning}

As you [configure SAML SSO](#configure-saml-on-prem), provide the values from the following fields in Cribl:

| Field in Cribl | Field in Microsoft Entra ID |
| -------------- | --------------------------- |
| **Audience (SP entity ID)** | **Identifier (Entity ID)** |
| **Sign-on callback URL** | **Reply URL (Assertion Consumer Service URL)** |
| **Logout callback URL** | **Logout Url (Optional)** |

You will need the values from the following fields in the SAML application to finish SSO setup in Cribl:

| Field in Microsoft Entra ID | Field in Cribl |
| --------------------------- | -------------- |
| **Login URL** | **Single sign-on (SSO) URL** |
| **Logout URL** | **Single logout (SLO) URL** |
| **Certificate (Base64)** under **SAML Certificates** | **Response validation certificate** |

In **Group name field** in Cribl, enter `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups`. 

> The field names and documentation for adding a SAML application might change without notice due to product changes in Microsoft Entra ID. Refer to the [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/) for the latest information.
{.box .info}

## General SSO Configuration in Cribl {#general-sso-on-prem}

The procedures in this section generally describe how to configure [OIDC SSO](#configure-oidc-on-prem) and [SAML SSO](#configure-saml-on-prem) in Cribl using any IdP.

[Snippet not found: content/shared/4.13/snippets/_sso-oidc-on-prem.md]

[Snippet not found: content/shared/4.13/snippets/_sso-saml-on-prem.md]

[Snippet not found: content/shared/4.13/snippets/_sso-verify-on-prem.md]

## Troubleshooting

If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-on-prem).
