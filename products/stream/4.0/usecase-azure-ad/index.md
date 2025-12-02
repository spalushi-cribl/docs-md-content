# Microsoft Entra ID + OpenID Configuration


This page outlines how to integrate Azure Active Directory with Cribl Stream's SSO/OpenID Connect [authentication](authentication#sso).

## Configure Microsoft Entra ID App

Start at the Azure portal to configure an OpenID Connect provider: [https://portal.azure.com/](https://portal.azure.com/).

### Register Your Microsoft Entra ID App

1. Open the **Azure Active Directory** Service.

2. In the left nav's **Manage** section, select **App registrations**.

3. Add a new registration. For details, see Microsoft's [Quickstart: Register an Application](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app#register-an-application) topic. <br/>
  In the example below, substitute the appropriate callback URL for your own Cribl Stream Leader instance:
  `https://leader.cribl.io:9000/api/v1/auth/authorization-code/callback`

![Registering a Microsoft Entra ID app](azure-ad-register-app.592bda4cad.png)
{border="true"}

###  Get the Microsoft Entra ID App's Basic Credentials  {#credentials}

You'll need to copy and paste these credentials into Cribl Stream's **Authentication** page [below](#logstream-auth).

1. You can find the OIDC Client ID on the new app's **Overview** page, as the **Application (client) ID**.

![Finding the OIDC Client ID](azure-ad-oidc-id.010ce360e0.png)
{border="true"}

2. Click the **Endpoints** button at the page top to display the OAuth endpoints. You can use either the v2 or the v1 endpoints.

![Copying OAuth 2 v2 endpoints](azure-ad-endpoints.c5c2c720dd.png)
{border="true"}

###  Create and Copy a Client Secret  {#secret}

1. To create a client secret: From the Azure portal's left nav, select **Certificate & secrets**. Then select **New client secret**.

![Accessing client secrets](azure-ad-certs.9749de8ef1.png)
{border="true"}

2. Add a new client secret with a descriptive name, and an expiration timeframe.

![Adding a client secret](azure-ad-client-secret.5f3cb34909.png)
{border="true"}

3. Click **Add**. 

4. Immediately copy the **Value** and **Secret ID** from the resulting page. You'll need to paste the **Value** into Cribl Stream's **Authentication** > **Client secret** field [below](#logstream-auth).

![Copy that secret!](azure-id-secret.4b942e7898.png)
{border="true"}



> This is the only time the secret is shown! Make sure you copy it while it’s visible. (If you missed your chance, you can start over by creating a new secret.)
>
{.box .warning}

### Configure Token and Claims

Here, you'll add the groups claim to the OIDC ID token.

1. From the Azure portal's left nav, select **Token configuration**, then select **Add groups claim**.

![Configuring a token](azure-id-token-config.939bc9b3f7.png)
{border="true"}

2. Configure the groups claim as necessary, then click **Add**.

![Editing the groups claim](azure-id-groups-claim.5c14a23366.png)
{border="true"}

> Unless you synchronize Azure AD with your on-prem Active Directory, AD will return only GUIDs for your group names. If you've synchronized, you'll then be able to configure returning the `sAMAccountName` instead.
>
{.box .info}

3. Your token is now configured, and you're all done on the Azure side.

![Microsoft Entra ID token configuration complete](azure-ad-token-complete.0801d509bc.png)
{border="true"}

##  Configure Cribl Stream Authentication  {#logstream-auth}

Switch to Cribl Stream, and navigate to its **Settings** > [**Global Settings** >] **Access Management** > **Authentication** page. Configure this as indicated below (with reactions):

- **Type**: **OpenID Connect**. This will expose relevant fields, setting several default values and placeholders.

- **Provider name**: Enter an arbitrary identifier for this Azure AD integration.

- **Audience**: Enter your Cribl Stream Leader instance's base URL. Use the format: `https://<your‑domain.ext>:9000` (do not use a trailing slash).

- **Client ID**: Enter your Microsoft Entra ID **Application (client) ID**. (In the Azure portal, see [above](#credentials) to copy this from your app's **Overview** page.)

- **Client secret**: Enter the **Client secret** > **Value** that you [earlier](#secret) generated and copied from the Azure app's **Certificates & secrets** page.

- **Scope**: Accept the default `openid profile email` scopes.

- **Authentication URL**: Paste the **OAuth 2.0 authorization endpoint** that you copied [above](#credentials) from the Azure app's **Overview** > **Endpoints** drawer.

- **Token URL**: Paste the **OAuth 2.0 token endpoint** that you copied [above](#credentials) from the Azure app's **Overview** > **Endpoints** drawer.

- **User Info URL**, **Logout URL**: Leave both fields blank.

- **User identifier**: Adjust this based on the endpoint you choose (v1 or v2) [above](#credentials). In v2, the `preferred_username`, `name`, and `email` fields are set, matching this field's default values. <br/>
  In v1, only the `name` field is included in the token by default, so an acceptable entry here might be: `` `${unique_name || upn || username || name}` ``. You can check the token fields returned by [enabling](monitoring#change-logging-levels) debug-level logging on Cribl Stream's `auth:sso` channel.

- Change the **Filter type** to `User info filter`. 

- Optionally, enable **Allow local auth** as a fallback login method.

![Sample Cribl Stream Authentication Settings for Azure AD (v2 endpoints)](azure-ad-logstream-auth-config.10ba779a39.png)
{border="true"}

![Sample User identifier entry for v1 endpoints](azure-ad-logstream-auth-config-v1-endpoint.a5eb3a7f44.png)
{border="true"}

###  Map Microsoft Entra ID Groups to Cribl Stream Roles  {#map}

Next, map your Microsoft Entra ID groups to Cribl Stream [Roles](roles). The group names might appear as GUIDs. You can translate these on the Microsoft Entra ID **Groups** page.

Unless you synchronize Azure AD with your on-prem Active Directory, you will need to obtain the Group GUIDs from the Azure AD **Groups** page. Place these GUIDs in the mappings box and then choose the appropriate Cribl Stream Role. Here is a simple example.

![Microsoft Entra ID groups...](azure-ad-groups.40528e6f5e.png)
{border="true"}

![...mapped to Cribl Stream Roles](azure-ad-logstream-roles.7d58df271f.png)
