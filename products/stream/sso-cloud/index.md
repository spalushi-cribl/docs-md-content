# SSO on Cribl.Cloud


With a Cribl.Cloud account on [certain plan/license tiers](https://cribl.io/pricing/), you can use an identity provider (IdP) to configure Single Sign-On (SSO) using OpenID Connect (OIDC) or Security Assertion Markup Language (SAML) in Cribl.Cloud.

Configuration details vary for different IdPs, but the general steps for setting up SSO between an IdP and Cribl.Cloud deployment are:

1. Set up [fallback access](#fallback-access-cloud).
1. Configure [OIDC](#configure-oidc-cloud) or [SAML](#configure-saml-cloud) SSO and [configure SSO Groups](sso-initial-steps) in the IdP that map to Cribl.Cloud Teams or Roles.
1. [Verify that SSO is working](#verify-cloud).

> The following guides describe how to configure SSO with a specific IdP:
>
> - [OIDC with Okta](oidc-okta-setup)
> - [SAML with Okta](saml-okta-setup)
> - [SSO with Microsoft Entra ID](sso-microsoft-entra-id-cloud)
> - [SSO with Ping Identity](sso-ping-identity-cloud)
{.box .success}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IdP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IdP instance by creating a chiclet.
Follow the guides for [Okta/SAML](saml-okta-setup#saml-chiclet-okta), [Okta/OIDC](usecase-sso-okta#oidc-chiclet), or [Microsoft Entra ID](sso-microsoft-entra-id-cloud#my-apps-chiclet) to create a chiclet.


 
## Set Up Fallback Access {#fallback-access-cloud}

Before you configure SSO, create a fallback user so that you aren't locked out of your Organization if you have issues with SSO. In your Cribl.Cloud Organization, [invite a new Member](members#invite-members) using an email domain that’s different from the corporate domain on which you’re configuring SSO. Assign the [Owner](permissions#organization) Permission for the Member. You can use this account to log in with a username and password and fix SSO issues if needed.

> After you confirm that your SSO integration is working, you can remove the fallback user. If you do so, do not disable the SSO integration without first re-creating a fallback user. Otherwise, you might get locked out of your Organization.
>
{.box .warning}


## Bypass SSO with Multiple Organizations {#bypass-sso-multi-orgs-cloud}

> For Cribl.Cloud Government, SSO is mandatory once configured. It does not support bypassing SSO for local accounts or for multi-organization access. For details, see [SSO in Cribl.Cloud Government](/fedramp/fed_access_sso).
{.box .cloud}

If you have SSO configured and you want to sign up for an additional Cribl.Cloud Organization, you need to bypass SSO. Otherwise, you will be forced to log into your existing Organization, because SSO does Home Realm Discovery and recognizes your email address.

In that case, edit your login URL and delete the word identifier. For example:

* Original URL: `https://login.cribl.cloud/u/login/identifier?state=<long_string_of_characters>`

* Edited URL: `https://login.cribl.cloud/u/login/?state=<long_string_of_characters>`

When you use this URL, instead of forcing you through SSO, Cribl.Cloud will ask for a username and password.

If you try to log in with an account that has a bad Permissions state, bypassing SSO with multiple Organizations might not work. In that case, you might be able to resolve the Permission issue using a [fallback user](#fallback-access-cloud).

## General SSO Configuration in Cribl.Cloud {#general-sso-cloud}

> The registration process uses the email domain of the authenticated user who configures SSO in Cribl.Cloud, regardless of the domain that is specified in the IdP configuration.
{.box .info}

The procedures in this section generally describe how to configure [OIDC SSO](#configure-oidc-cloud) and [SAML SSO](#configure-saml-cloud) in Cribl.Cloud using any IdP.



### Configure OIDC SSO {#configure-oidc-cloud} 

1. In Cribl.Cloud, in the sidebar, select **Organization > SSO Management**.

1. Scroll down to the end of the **Product-Level Mappings** and select **OIDC**.

1. In the IdP, create the OIDC application. Use the information from Cribl.Cloud under **Web Application Settings**:

   * **App integration name**: The name to use for the OIDC application you configure in the IdP.
   
   * **Application type**: The kind of OIDC application to integrate (**Web**).

   * **Sign-in redirect URIs** lists two URLs:
      * `https://login.cribl.cloud/login/callback` is the primary OIDC redirect URI, also called the callback URL. After a user authenticates with the IdP, the IdP sends an authorization code to this endpoint. Cribl.Cloud exchanges the authorization code for tokens and completes the login. Register this URI in the OIDC application in the IdP as an allowed redirect/callback URI.
      * `https://manage.cribl.cloud/organizations/<organizationId>/sso` is a testing URL to use during setup. After you successfully test the connection, remove this URL from the list of allowed redirect URIs in the IdP.

   * **Sign-out redirect URIs**: The endpoint where Cribl.Cloud redirects users after they log out. Register this URI in the OIDC application in the IdP settings to allow Cribl.Cloud to complete the logout flow.

   * **Groups map key value**: The key value to use to map groups from the IdP to Cribl.Cloud. Read [Configuring SSO Groups](sso-initial-steps) for information about valid IdP group names and Permission mapping.
        
   * **Scopes**: The set of user attributes that the IdP should return to Cribl.Cloud in its authentication response. For example, if you omit the `group` scope in the OIDC application, IdP group membership won't be available to Cribl.Cloud.

   > For OIDC applications, you must use backchannel authentication. Cribl.Cloud does not support front-channel authentication via OIDC.
   {.box .info}

2. After you create and save the OIDC application in the IdP, return to Cribl.Cloud to finish OIDC SSO setup. Scroll down to **Cribl Cloud SSO settings** and enter the following information from the OIDC application in the IdP:

   * **Client ID**: The unique identifier that the IdP assigned to the OIDC application. Cribl.Cloud uses the Client ID to identify itself to the IdP during authentication flows .

   * **Client secret**: The confidential string that the IdP generated for the OIDC application. Cribl.Cloud uses the Client secret to authenticate to the IdP when exchanging authorization codes for tokens. Keep the Client secret secure and do not expose it publicly.

   * **Issuer URL**: The unique URL that identifies the IdP as an OIDC authority. Cribl.Cloud uses the Issuer URL to discover the IdP metadata and to validate tokens. Provide the exact Issuer URL from the OIDC configuration in the IdP.

3. Select **Save**.


> In Cribl.Cloud Government, the The SAML setup process involves a specific user workflow and distinct security
requirements for compliance. For details, see [SAML Configuration](/fedramp/fed_access_sso/#saml-configuration).
 {.box .warning}



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

> In Cribl.Cloud Government,the Test Connection feature provides fewer diagnostic details due to regulatory restrictions.  For details, see [SSO in Cribl.Cloud Government](/fedramp/fed_access_sso).
 {.box .info}

## Update the SAML SSO Certificate {#update-cert-saml-cloud}

Follow these steps to update the public certificate for your SAML SSO configuration:

1. In Cribl.Cloud, in the sidebar, select **Organization > SSO Management**.

1. Scroll down to the end of the **Product-Level Mappings** and select **SAML**.

1. Under **SAML configuration**, select **Update Certificate**. 

1. Enter the public certificate that the IdP uses to sign SAML responses. Paste the entire PEM-encoded certificate, including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines.

1. Select **Save**.

## Troubleshooting {#troubleshooting-cloud}

If you encounter issues when setting up SSO, refer to [SSO Troubleshooting in Cribl.Cloud](sso-troubleshooting-cloud).
