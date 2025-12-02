# SSO with Okta and OIDC (Cribl.Cloud)


This page presents a walkthrough of setting up an OIDC SSO, using Okta as the example.

> Cribl.Cloud supports only OIDC backchannel authentication, not front-channel.
>
{.box .cloud}

> This page is a guide for configuring SSO for a Cribl.Cloud deployment.
> For information about an on-prem installation, see [SSO with Okta and OIDC (on-prem)](usecase-sso-okta).
{.box .info}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IdP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IdP instance by [creating a chiclet](#oidc-chiclet).

[Snippet not found: content/shared/4.14/snippets/_sso-fallback-access-cloud.md]

## Create OIDC App Integration {#create-app-oidc}

To create your app integration:

1. In Okta, navigate to the **Applications** section and select **Create App Integration**. 
2. Configure the app integration with the options below:
   
   - **Sign-in method**: `OIDC - OpenID Connect`
   - **Application type**: `Web Application`

### General Settings

3. Configure the app integration's **General Settings** with the options below:
   
   - **App integration name**: `Cribl.Cloud (OIDC)`
   - **Grant type**: Select `Authorization Code` and `Refresh Token`.
   - **Sign-in redirect URIs**: 
     - `https://login.cribl.cloud/login/callback` is the URL you will use for the connection.
     - `https://manage.cribl.cloud/organizations/<organizationId>/sso` is used during setup to test the connection.
       After you have successfully tested the connection, save the configuration and replace the second URL with the first one.
   - **Sign-out redirect URIs**: [https://login.cribl.cloud/v2/logout](https://login.cribl.cloud/v2/logout)

> If your IdP is PingOne, you must also configure this (non-Okta) option:
> - **Authentication options**: `Allow Client Secret`
>
{.box .info}

### Assignments

4. Configure the **Assignments** pane with the following options:
   
   - **Controlled access**: `Limited access to selected groups`
   - **Selected groups**: The groups you mapped in [Configure Groups](sso-initial-steps).

5. **Save** your application.
6. Now return to the **General** tab's **General Settings** section and in **Refresh token behavior**, select `Use persistent token`. 

### Sign On Tab

7. If you are **not** [mapping Teams](sso-initial-steps#link) to IdP groups,
   you need to specify a groups claim filter.
   In the **OpenID Connect ID Token** section, select **Edit**, and set the **Groups claim filter** to: `groups` : `Starts with` : `Cribl`.

8. To obtain the **Issuer URL** that you'll need to provide to Cribl in the next section, change the value in the **Issuer** field from `Dynamic` to `Okta URL`.

This step concludes the setup procedure for Okta (or other IdP). 

## Submit Your App Info to Cribl {#tell-cribl}

Next, provide Cribl with essential details about your application to implement the SSO setup on the Cribl side. 

> The registration process uses the email domain of the authenticated user who configures SSO in Cribl.Cloud, regardless of the domain that is specified in the IdP configuration.
{.box .info}

1. In Cribl Stream, on the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, then **SSO Management**.
1. Above **Web Application Settings**, select **OIDC**. 
1. The **Web Application Settings** are prefilled for you, so you only need to fill in the **Cribl Cloud SSO Settings** section with the following details from your IdP client configuration:
   - Client ID
   - Client Secret
   - Issuer URL. Copy the **Issuer** URL from the **Sign On** > **OpenID Connect ID Token** section of your Okta environment.

## OIDC/Okta Chiclet Setup (Optional) {#oidc-chiclet}

If you want to initiate login from your Okta instance with OIDC authentication configured, an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.
2. Find the OIDC application created earlier in the [OIDC/Okta Setup Example](#create-app-oidc).
3. Select that application, and in the **General** tab's **General Settings** section, select **Edit**.
4. In the **Initiate login URI** field, enter `https://manage.cribl.cloud/login?connection=<organizationId>` (where `<organizationId>` is your Cribl.Cloud Organization's ID).
5. Confirm with **Save** to complete the chiclet.

## Troubleshooting

If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-cloud).
