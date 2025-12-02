# SSO in On-Prem Deployments


With a Cribl account on [certain plan tiers](https://cribl.io/pricing/), you can use an identity provider (IdP) to configure Single Sign-On (SSO) using OpenID Connect (OIDC) or Security Assertion Markup Language (SAML) in Cribl.

Configuration details vary for different IdPs, but the general steps for setting up SSO between an IdP and an on-prem Cribl deployment are:

1. Set up [fallback access](#fallback-access-on-prem).
1. Configure [OIDC](#configure-oidc-on-prem) or [SAML](#configure-saml-on-prem) SSO.
1. [Verify that SSO is working](#verify-cloud).

> The following guides describe how to configure SSO with a specific IdPs:
> 
> - [OIDC with Okta](usecase-sso-okta)
> - [SAML with Okta](usecase-sso-saml)
> - [SSO with Microsoft Entra ID](sso-microsoft-entra-id-on-prem)
> - [SSO with Ping Identity](sso-ping-identity-on-prem)
> 
> If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-on-prem).
{.box .success}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IdP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IdP instance by creating a chiclet.
Follow the guides for [Okta/SAML](usecase-sso-okta#oidc-chiclet) or [Okta/OIDC](usecase-sso-saml#saml-chiclet-okta) to create a chiclet.


## Set Up Fallback Access {#fallback-access-on-prem}

When you configure OIDC or SAML SSO, enable local authenticaton to ensure that users aren't locked out if you have issues with SSO. Local authentication provides fallback access so that users can log in with a username and password.

1. In the sidebar, select **Settings** > **Global**.

1. Under **Access Management**, select **Authentication**.

1. In the **Type** drop-down menu, select **OpenID Connect** or **SAML 2.0**. 

1. In the configuration options, enable **Allow login as Local User**. Enabling this option means that the login page will include the **Log in as Local User** button so that users can log in with a username and password.

> After you confirm that your SSO integration is working, you can disable **Allow login as Local User** in the SSO configuration. If you do get locked out, see [Manual Password Replacement](authentication#manual-pwd).
{.box .warning}


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
