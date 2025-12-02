# OIDC/Okta Setup Example


This page expands on the overview for OIDC, offering a detailed walkthrough with Okta as the example IDP. 

> Cribl.Cloud supports only OIDC backchannel authentication, not front-channel.
>
{.box .cloud}

## Create OIDC App Integration {#create-app-oidc}

To create your app integration within Okta, start here:

![Creating an app integration](cloud-sso-okta-app-4.fd1fa1579d.png)
{border="true"}

Next, configure the app integration with the options below.
- **Sign-in method**: `OIDC - OpenID Connect`
- **Application type**: `Web Application`

![Configuring new Okta app integration](cloud-sso-okta-integration-5.884b8caae1.png)
{border="true"}

## General Settings

Configure the app integration's **General Settings** with the options below.
- **App Integration Name**: `Cribl.Cloud`
- **Sign-In Redirect URIs**: [https://login.cribl.cloud/login/callback](https://login.cribl.cloud/login/callback)
- **Sign-Out Redirect URI**s: [https://login.cribl.cloud/v2/logout](https://login.cribl.cloud/v2/logout)
- **Grant Type**: `Authorization Code`, `Refresh Token`
- **Scopes**: `openid profile email groups`

![App integration name and URIs](cloud-sso-okta-uris-6a.bb28070e4c.png)
{border="true"}

> If your IDP is PingOne, you must also configure this (non-Okta) option:
> - **Authentication Options**: `Allow Client Secret`
>
{.box .info}

## Assignments

Configure the **Assignments** pane with the options below.
- **Controlled access**: `Limited access to selected groups`
- **Selected groups**: <The groups you mapped in [Configure Groups](sso-initial-steps#configuring-groups)>

![Assignments configuration](cloud-sso-okta-assignments-7.a6dab94c2a.png)
{border="true"}

## Sign-On Tab

Edit the **OpenID Connect ID Token** section, as shown here:

![Sign-On tab’s Settings/Tokens sections](cloud-sso-okta-sign-on-9.5a5cb2bf1a.png)
{border="true"}

Set the **Groups claim filter** to: `groups` : `Starts with` : `Cribl`, as shown below:

![OIDC token: configuring Groups claim filter](cloud-sso-okta-groups-claim-10.4ea8d49397.png)
{border="true"}

Change the **Issuer** from `Dynamic` to `Okta URL`, to get the **Issuer URL** to provide to Cribl in the next section:

![OIDC token: obtaining Issuer URL](cloud-sso-okta-issuer-url.5350098b87.png)
{scale="70%"}

## Submit Your App Info to Cribl {#tell-cribl}

This concludes the setup you need to do in Okta (or other IDP). Next, provide Cribl essential details about your application, to implement SSO setup on the Cribl side. 

On your Cribl.Cloud portal's [Organization page](cloud-portal#organization) > **SSO** tab, select the **OIDC** lower tab. 

![SSO > OIDC lower tab](cloud-sso-oidc-tab.2b1b96ef18.png)
{border="true"}

The **Web Application Settings** are prefilled for you, so you only need to fill in the **Cribl Cloud SSO Settings** section with the following details from your IDP client configuration:
   - Client ID
   - Client Secret
   - Issuer URL.
     Make sure **Issuer URL** does not contain a trailing space.

Continuing with our Okta example, here's where you'd copy the Issuer URL from your Okta app:

![Obtaining Issuer URL from Okta](cloud-sso-okta-metadata-11.70345f8f9d.png)
{border="true"}



## OIDC/Okta Chiclet Setup (Optional) {#oidc-chiclet}

If you want to initiate login from your Okta instance with OIDC authentication configured, an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.

2. Find the OIDC application created earlier in the [OIDC/Okta Setup Example](#oidc).

3. Click that application, and select **General Settings** > **Edit**.

4. In the **Initiate login URI** field, enter `https://portal.cribl.cloud/login?connection=<organizationID>` (where `<organizationID>` is your Cribl.Cloud Organization's ID).

![Adding the Initiate Login URI value](cloud-sso-oidc-app.d5e5aca928.png)
{border="true"}

5. Click **Save** to complete the chiclet.

## Link Existing Users

To ensure that your Cribl.Cloud Organization's local users have a smooth transition to SSO, see [Final SSO Steps & Troubleshooting](sso-final-steps).
