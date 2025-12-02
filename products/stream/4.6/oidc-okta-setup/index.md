# OIDC/Okta Setup Example


This page expands on the overview for OIDC, offering a detailed walkthrough with Okta as the example IDP. 

> Cribl.Cloud supports only OIDC backchannel authentication, not front-channel.
>
{.box .cloud}

## Create OIDC App Integration {#create-app-oidc}

To create your app integration within Okta, navigate to the **Applications** section of your Okta environment and click **Create App Integration**. 

Next, configure the app integration with the options below.
- **Sign-in method**: `OIDC - OpenID Connect`
- **Application type**: `Web Application`

### General Settings

Configure the app integration's **General Settings** with the options below.
- **App integration name**: `Cribl.Cloud (OIDC)`
- **Grant type**: Select `Authorization Code` and `Refresh Token`.
- **Refresh token behavior**: Select `Use persistent token`.
- **Sign-in redirect URIs**: 
  - [https://login.cribl.cloud/login/callback](https://login.cribl.cloud/login/callback)
  - `https://manage.cribl.cloud/<organizationID>/organization/sso`
- **Sign-out redirect URI**s: [https://login.cribl.cloud/v2/logout](https://login.cribl.cloud/v2/logout)


> If your IDP is PingOne, you must also configure this (non-Okta) option:
> - **Authentication options**: `Allow Client Secret`
>
{.box .info}

### Assignments

Configure the **Assignments** pane with the following options:
- **Controlled access**: `Limited access to selected groups`
- **Selected groups**: The groups you mapped in [Configure Groups](sso-initial-steps#configuring-groups)

### Sign-On Tab

In the **OpenID Connect ID Token** section, set the **Groups claim filter** to: `groups` : `Starts with` : `Cribl`.

To obtain the **Issuer URL** you'll need to provide to Cribl in the next section, change the value in the **Issuer** field from `Dynamic` to `Okta URL`.

This step concludes the setup procedure for Okta (or other IDP). 

## Submit Your App Info to Cribl {#tell-cribl}

Next, provide Cribl essential details about your application, to implement SSO setup on the Cribl side. 

On your Cribl.Cloud portal's [Organization page](cloud-portal#organization) > **SSO** tab, select the **OIDC** lower tab. 

The **Web Application Settings** are prefilled for you, so you only need to fill in the **Cribl Cloud SSO Settings** section with the following details from your IDP client configuration:
   - Client ID
   - Client Secret
   - Issuer URL. Copy this value from the **API** > **Settings** section of your Okta environment.
     Make sure **Issuer URL** does not contain a trailing space.

## OIDC/Okta Chiclet Setup (Optional) {#oidc-chiclet}

If you want to initiate login from your Okta instance with OIDC authentication configured, an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.

2. Find the OIDC application created earlier in the [OIDC/Okta Setup Example](#oidc).

3. Click that application, and select **General Settings** > **Edit**.

4. In the **Initiate login URI** field, enter `https://manage.cribl.cloud/login?connection=<organizationID>` (where `<organizationID>` is your Cribl.Cloud Organization's ID).

5. Click **Save** to complete the chiclet.

## Link Existing Users

To ensure that your Cribl.Cloud Organization's local users have a smooth transition to SSO, see [Final SSO Steps & Troubleshooting](sso-final-steps).
