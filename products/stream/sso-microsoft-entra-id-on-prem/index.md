# SSO with Microsoft Entra ID (On-Prem)


This topic provides details to help you configure Single Sign-On (SSO) with Microsoft Entra ID as the identity provider (IdP).

To configure Microsoft Entra ID as an IdP, refer to the [Microsoft Entra ID documentation](https://learn.microsoft.com/en-us/entra/identity/).

> This page describes how to configure SSO for on-prem installations. For Cribl.Cloud, see [SSO with Microsoft Entra ID (Cribl.Cloud)](sso-microsoft-entra-id-cloud).
{.box .info}


## Set Up Fallback Access {#fallback-access-on-prem}

When you configure OIDC or SAML SSO, enable local authenticaton to ensure that users aren't locked out if you have issues with SSO. Local authentication provides fallback access so that users can log in with a username and password.

1. In the sidebar, select **Settings** > **Global**.

1. Under **Access Management**, select **Authentication**.

1. In the **Type** drop-down menu, select **OpenID Connect** or **SAML 2.0**. 

1. In the configuration options, enable **Allow login as Local User**. Enabling this option means that the login page will include the **Log in as Local User** button so that users can log in with a username and password.

> After you confirm that your SSO integration is working, you can disable **Allow login as Local User** in the SSO configuration. If you do get locked out, see [Manual Password Replacement](authentication#manual-pwd).
{.box .warning}


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



### Configure OIDC SSO {#configure-oidc-on-prem}

1. In the IdP, create and save the OIDC application.

   - If you can configure the application username, make sure that its value is `NameID`.
   - If the application requires a callback or redirect URI, provide the callback URL for the Leader Node of your Cribl deployment (for example, `https://{$hostname}:9000/api/v1/auth/authorization-code/callback`).

1. In Cribl, in the sidebar, select **Settings** > **Global**.

1. Under **Access Management**, select **Authentication**.

1. From the **Type** dropdown, choose `OpenID Connect`.

1. In the **Provider name** field, enter the provider name or select the provider from the list.  

1. In the **Audience (Relying Party ID)** field, enter your Cribl Stream Leader’s base URL. Do not append a trailing slash.

   > If you have a Distributed deployment with a fallback Leader configured, modify the **Audience (Relying Party ID)** field to point to the load balancer instead of the Leader Node.
   {.box .info}
   
1. Enter the following required information from the OIDC application in the IdP:

   * **Client ID**: The unique identifier of the OIDC application, which is generated when you register the application with the IdP. Cribl uses the Client ID to authenticate to the IdP during token exchange.

   * **Client secret**: The secret string that the IdP generates when you register the OIDC application. Cribl uses the Client secret to authenticate to the IdP during token exchange.
   
   * **Authentication URL**: The OIDC authorization endpoint for the IdP. Cribl redirects users to this URL to authenticate.

   * **Token URL**: The OIDC token endpoint for the IdP. Cribl uses the Token URL to exchange the authorization code for tokens.

1. In the **Scope** field, if Cribl is in Enterprise Distributed mode, add `groups` so that the list reads `openid profile email groups`.

   - In the OIDC application in the IdP, configure the ID token to include the `groups` claim.

1. If the Cribl deployment is in Distributed mode with an Enterprise license, configure **Role Mapping** settings. With a Standard license, all external users are imported to Cribl in the `admin` role. If Cribl is running in Single-instance mode, you cannot map IdP groups to Cribl Roles.

   - Select a **Default role**. Cribl recommends selecting `user` so that the default Role will be assigned to users who are not in any groups.
   
   - In the **Mapping** fields, add mappings as needed. The IdP group names in the left column are case-sensitive and must match the values returned by IdP.

   > Keep these principles in mind when you map groups to Roles:
   > 
   > - An IdP group can map to more than one Cribl Role. Likewise, a Cribl Role can map to more than one IdP group.
   > - If a user has multiple Roles, Cribl applies the union of the most permissive levels of access.
   > - If a user has no mapped Roles, Cribl automatically assigns the default Role that you specify.
   > - The value used to identify groups in the IdP (such as GUID, Object ID, or group name) is case-sensitive and must exactly match the value you enter in Cribl’s role mapping configuration.
   {.box .info}

   ![Role mapping in Cribl with sample mappings.](st-usecase-sso-okta_06.9892fa386f.png)
   {border="true" caption="Example Role mapping"}

1. Review the other fields on the **Authentication** page and update their values as needed depending on the IdP and your desired configuration.

   > To prevent lockout, we recommend enabling **Allow login as Local User** until you’re certain that external authentication is working as intended. If you do get locked out, see [Manual Password Replacement](authentication#manual-pwd).
   {.box .warning}
   
1. Select **Save**.




### Configure SAML SSO {#configure-saml-on-prem}

1. In Cribl, in the sidebar, select **Settings** > **Global**.

1. Under **Access Management**, select **Authentication**.

1. From the **Type** dropdown, choose `SAML 2.0`.

1. In the **Audience (SP entity ID)** field, enter the base URL of your Cribl instance (for example, `https://{$hostname}:9000`). Do not append a trailing slash, and make sure that the URL uses the HTTPS protocol.

   > If you have a Distributed deployment with a fallback Leader configured, modify the **Audience (SP entity ID)** field to point to the load balancer instead of the Leader Node.
   {.box .info}
   <br />
   
   The following URLs automatically populate using the URL that you enter as the **Audience (SP entity ID)**. Depending on the IdP, you might use some or all of these URLs, in addition to the **Audience (SP entity ID)**, to create the SAML application in the next step:

   * **Sign-on callback URL**: The Assertion Consumer Service (ACS) URL. After a user authenticates with the IdP, this URL receives and processes the SAML assertion (authentication response) from the IdP to complete the login.
   
   * **Logout callback URL**: The Single Logout (SLO) endpoint. If you enable SLO in the SAML application, the IdP sends logout requests and responses to this URL so that Cribl can process SAML logout flows, ensuring that users are logged out from both Cribl and the IdP.

   * **Metadata URL**: The endpoint that serves the SAML Service Provider (SP) metadata in XML format. This metadata describes Cribl’s SAML endpoints, certificates, and configuration. You can include the **Metadata URL** in the SAML application settings if the IdP supports importing metadata.<br /><br />

   Leave the **Authentication** page open in Cribl. You will complete setup in Cribl with information from the SAML application in a later step.

1. In the IdP, create and save the SAML application.

1. Return to the **Authentication** page in Cribl to complete SSO setup.

1. In the **Authentication** page, enter the following required information from the SAML application:

   * **Issuer (IDP entity ID)**: The SAML entity ID for the IdP. The IdP issuer is a unique string (often a URI or URL) that identifies the IdP in SAML assertions. Cribl uses the IdP issuer to validate the authenticity of SAML responses.

   * **Single sign-on (SSO) URL**: The SAML SSO endpoint URL for the IdP, where Cribl should send SAML authentication requests. If the IdP supports SLO at the same endpoint, Cribl will use this URL for both login and logout flows.
   
   * **Response validation certificate**: The public certificate that the IdP uses to sign SAML responses. Paste the entire PEM-encoded certificate, including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines.

1. If the Cribl deployment is in Distributed mode with an Enterprise license, configure **Role Mapping** settings. With a Standard license, all external users are imported to Cribl in the `admin` role. If Cribl is running in Single-instance mode, you cannot map IdP groups to Cribl Roles.

   - Select a **Default role**. Cribl recommends selecting `user` so that the default Role will be assigned to users who are not in any groups.
   
   - In the **Mapping** fields, add mappings as needed. The IdP group names in the left column are case-sensitive and must match the values returned by IdP.

   > Keep these principles in mind when you map groups to Roles:
   > 
   > - An IdP group can map to more than one Cribl Role. Likewise, a Cribl Role can map to more than one IdP group.
   > - If a user has multiple Roles, Cribl applies the union of the most permissive levels of access.
   > - If a user has no mapped Roles, Cribl automatically assigns the default Role that you specify.
   > - The value used to identify groups in the IdP (such as GUID, Object ID, or group name) is case-sensitive and must exactly match the value you enter in Cribl’s role mapping configuration.
   {.box .info}

   ![Role mapping in Cribl with sample mappings.](st-usecase-sso-okta_06.9892fa386f.png)
   {border="true" caption="Example Role mapping"}

1. Review the other fields on the **Authentication** page and update their values as needed depending on the IdP and your desired configuration.

   > To prevent lockout, we recommend enabling **Allow login as Local User** until you’re certain that external authentication is working as intended. If you do get locked out, see [Manual Password Replacement](authentication#manual-pwd).
   {.box .warning}
   
1. Select **Save**.




## Verify that SSO Is Working {#verify-on-prem}

1. Log out of Cribl and verify that login page includes a button for the IdP.

1. Select **Log in with ...** (the button should include the name of the IdP that you configured).

1. Cribl should redirect you to the IdP to authenticate.


## Troubleshooting

If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-on-prem).
