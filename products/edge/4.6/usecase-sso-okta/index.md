# SSO with Okta and OIDC


Cribl Edge supports SSO/OIDC user authentication (login/password) and authorization (user's group membership, which you can map to Cribl [Roles](#role-mapping)). Using OIDC will change the default `Log in` button on the login page to a button labeled `Log in with OIDC` which redirects to the configured Identity Provider (IDP).

If you are a Cribl Edge admin and want to offer single sign-on (SSO) to your users, you can choose OpenID Connect (OIDC) as the authentication type, then configure an IDP to use OIDC within its single sign-on flow. Once configuration is complete (several steps later), the Cribl Edge login page will send users to the IDP's login UI. Besides the IDP, some settings will refer to the Service Provider (SP), which in this context is your Cribl Edge instance.

The IDP can be Okta or Google, among others. Configuring SSO requires going back and forth between Cribl Edge and the IDP's UI. In this page, we walk through the process for configuring SSO with OIDC, using Okta as the IDP.

The last part of the walkthrough is a [complete description](#finish-in-cribl) of the @{product **Authentication** settings (with OIDC selected as the authentication method). Skip directly to this section if you just need a UI reference, or if your IDP is not Okta. For non-Okta deployments, please join us on Cribl's Community Slack at [https://cribl‑community.slack.com/](https://cribl-community.slack.com/) and share your questions.

> Make sure you don't get locked out of Cribl Edge! Enable the **Allow login as Local User** toggle until you're certain that external auth is working as intended. If you do get locked out, refer to [Manual Password Replacement](authentication#manual-pwd) for the remedy.
> 
> Like other external auth methods, OpenID Connect requires either an Enterprise or a Standard license. It is not supported with a Free license.
>
{.box .warning}

## Plan Your Mapping of Okta Groups to Cribl Edge Roles {#map-groups-to-roles}

In Okta, admins organize their users in groups. In Cribl Edge, there are no user groups, but there are [Roles](roles). Your task includes **mapping** Okta groups to Cribl Edge Roles.
- Mapping groups to Roles is possible only for Cribl Edge deployments that are in Distributed mode, with an Enterprise license applied.
- If you are running Cribl Edge in Single-instance mode, you cannot map Okta groups to Cribl Edge Roles, although you can still set up SSO with Okta.

As you think through how best to map your Okta groups to Cribl Edge Roles, keep these principles in mind:
- An Okta group can map to more than one Cribl Edge Role.
- A Cribl Edge Role can map to more than one Okta group. 
- If a user has multiple Roles, Cribl Edge applies the union of the most permissive levels of access.

- Cribl Edge automatically assigns the `default` Role to any user who has no mapped Roles.

The example below illustrates how multiple mappings work: The groups in Mapping **b** and **c** each map to multiple Roles, while both the `reader_all` and `editor_cloud` Roles map to multiple groups.

Mapping | Okta Group | Cribl Edge Role(s)
--- | --- | ---
a. | Cribl Admins | `admin`
b. | Cloud Admins | `reader_all`, `editor_cloud`
c. | Security Team | `reader_all`, `editor_cloud`, `editor_firewall`

Cribl Edge Roles and [role mapping](authentication#role-mapping) are supported **only** with an Enterprise license. With a Standard license, all your external users will be imported to Cribl Edge in the `admin` role.

## Integrate Okta with Cribl Edge {#okta-app}

1. Log in to your Okta tenant admin console.

2. In the left nav, select **Applications** > **Applications**.

3. Click **Create App Integration**.

   - For **Sign-in method**, select `OIDC - OpenID Connect`.

   - For **Application type**, select `Web Application`.

4. Click **Next** to open the **New Web App Integration** page.

   - In the **App name** field, enter Cribl Edge.

   - (Optional) In the **Logo** field, upload the Cribl logo. You can use a logo from the Cribl [Media Kit](https://cribl.io/brand-assets/).
    - (Optional) If you wish to keep your OIDC app hidden, check the **App visibility** check box.

5. In the **Sign-in redirect URIs** field, replace the default with your Leader base URL, and with `/api/v1/auth/authorization-code/callback` as the path. This is the Cribl Edge callback API endpoint.

6. (Optional) In the **Sign-out redirect URIs** field, append `/login` to the pre-filled path.

7. In the **Assignments** > **Controlled access** area:
    - If all your Okta users need access to Cribl Edge, select **Allow everyone in your organization to access**.
    - To permit specific Okta groups to access Cribl Edge, select **Limit access to selected groups**. Then, in the field below, add the groups you want to include. After you finish creating the app, if you need to add or remove groups, do that in the **Applications** > **Assignments** tab. 

8. Click **Save**.

Okta should show an `Application Created Successfully` message.

![Completing the new app integration in Okta](st-usecase-sso-okta_01.1e7da2a82c.png)
{border="true"}

## Copy Your Okta App's Client ID and Client Secret {#okta-creds}

In the **Client Credentials** panel, copy both the **Client ID** and **Client Secret**, and temporarily store them locally. You will need them in the next step, when you configure Cribl Edge.

## Begin Configuring OIDC Auth in Cribl Edge {#config-ls}

> The only OAuth 2.0 flow that Cribl Edge supports is the [Authorization Code Grant](https://developer.okta.com/blog/2018/04/10/oauth-authorization-code-grant-type) flow.
> 
> In version 3.0 and higher, Cribl Edge's former "master" application components are renamed "leader." Above, while some legacy terminology remains within URLs, this document will reflect that.
>
{.box .info}

In Cribl Edge, select **Settings** > **Access Management** > **Authentication**.

1. Choose `OpenID Connect` from the **Type** dropdown.

1. Choose `Okta` from the **Provider** dropdown.

1. In the **Audience** field, enter your Cribl Edge UI base URL. Do **not** append a trailing slash.

1. In the **Client ID** and **Client secret** fields, enter the respective values that you copied from the Okta UI in the previous step.

1. If your Cribl Edge is in Enterprise Distributed mode:

   In the **Scope** field, add the scope `groups` to the default space-separated list of scopes, which is:  
   `openid profile email`. 

1. Obtain the authentication, token, user info, and logout URLs for your Okta app, by sending a request to the OpenID Connect Discovery endpoint. 

    * This endpoint has the URL:  
    `https://<tenant>.okta.com/.well-known/openid-configuration` 

    ...where `<tenant>` is your Okta tenant name. For example:  
    `https://dev-12345678.okta.com/.well-known/openid-configuration`

    * You can view the discovery document in your web browser, or use [jq](https://stedolan.github.io/jq/) to extract the needed values, as in the following example:

    `curl -s https://<tenant>.okta.com/.well-known/openid-configuration | jq '. | {"auth": 
   (.authorization_endpoint), "token":(.token_endpoint), "userinfo":(.userinfo_endpoint), "logout": 
   (.end_session_endpoint)}'`

    * Sample response:

    `{
      "auth": "https://dev-416897.oktapreview.com/oauth2/v1/authorize",
      "token": "https://dev-416897.oktapreview.com/oauth2/v1/token",
      "userinfo": "https://dev-416897.oktapreview.com/oauth2/v1/userinfo",
      "logout": "https://dev-416897.oktapreview.com/oauth2/v1/logout"
    }`

1. Populate the **Authentication URL** **Token URL** fields with the respective `auth` and `token` URLs.

1. If you configured Okta to use groups, populate the **User info URL** field with the `userinfo` URL.

    This is necessary because Okta does not send group information in the `id_token` passed to Cribl Edge.

1. If you want **Account** > **Log out** in Cribl Edge to log the user out **globally**, populate the **Logout URL** field with the `logout` URL. This means that when a user clicks the **Accounts** > **Log out** link in Cribl Edge, they are logged out of **both** Cribl Edge and Okta.

![Authentication settings in Cribl Edge](st-usecase-sso-okta_02.10e4d3ee4a.png)
{border="true"}

## Configure Response to Okta `/userinfo` Endpoint {#userinfo-endpoint}

An Okta tenant's user groups can be mastered either inside Okta, outside Okta, or both.

When the `/userinfo` endpoint is queried, Okta returns the appropriate groups membership of the user back to Cribl Edge:

* For groups mastered inside Okta only, the app should pass a `Filter` type groups claim to Cribl Edge.

* For groups mastered outside Okta (such as Active Directory), or both inside and outside, the app should pass an `Expression` type groups claim back to Cribl Edge.

See the Okta documentation on [dynamic allow lists](https://developer.okta.com/docs/guides/customize-tokens-dynamic/add-groups-claim-dynamic/) and 
using Okta [together](https://support.okta.com/help/s/article/Can-we-retrieve-both-Active-Directory-and-Okta-groups-in-OpenID-Connect-claims?language=en_US) with Active Directory.

In Okta, you should still be in the panel for the app you created. If not, you can get there by opening **Applications** > **Applications** and selecting the app.

* For groups mastered **inside** Okta only, complete [this](#groups-inside-okta) procedure.

* For groups mastered **outside** Okta, or both inside and outside, complete [this](#groups-outside-okta) procedure.

###  Configure Groups Inside of Okta {#groups-inside-okta}

Open the **Sign On** tab. Then, in the **OpenID Connect ID Token** panel:

1. Click **Edit** to change the value of **Groups claim filter** to `groups` and show filter options.

1. Leave **Groups claim type** set to `Filter.`

1. Choose **Matches regex** from the dropdown, and enter `.*` as the regex.

1. Click **Save**.

![Role mapping, beginning](st-usecase-sso-okta_03.8e97b82d48.png)
{border="true"}

###  Configure Groups Outside of Okta {#groups-outside-okta}

Open the **Sign On** tab, if necessary. Then, in the **OpenID Connect ID Token** panel:

1. Click **Edit** to change the value of **Groups claim filter** to `groups` and show filter options.

1. Set **Groups claim type** set to `Expression.`

1. In the **Groups claim expression**, enter an expression field that matches the groups you want passed to Cribl Edge. See the Okta documentation for more details.

    For example, to match on Active Directory groups that contain the string `okta`, use the following expression:

    ```
    Groups.contains("active_directory", "cribl", 10)
    ```
1. Click **Save**.

![Role mapping, continued](st-usecase-sso-okta_04.237c70a0ea.png)
{border="true"}

## Configure ID Token to Include Groups Claim {#id-token}

> Okta can recognize your groups **only** if your ID token includes your groups claim, as you'll configure here.
>
{.box .warning}

1. In Okta, open the **Security** > **API** page.

2. In the **Authorization Servers** tab, click the edit (pencil) button for the desired Authorization Server. 

3. In the resulting page, click the **Claims** tab.

4. If your groups claim already exists, click the edit (pencil) button. Otherwise, click **Add Claim**.

5. In the **Include in token type** drop-downs, choose `ID Token` and `Always`, respectively.

![Including the groups claim in the token ID](st-usecase-sso-okta_05.2f70d1ce5c.png)
{border="true"}

6. Configure the remaining settings in the way that suits your groups claim.

7. Click **Save** (or **Create** if you're adding the claim for the first time).

## Finish Configuring OIDC Auth in Cribl Edge {#finish-in-cribl}

> This section documents the entire Cribl Edge Authentication UI. If you have been working through the procedures from [earlier in this page](#begin-in-cribl), you will have completed some of the following steps already.
>
{.box .success}

Navigate to **Settings** > [**Global Settings** >] **Access Management > Authentication > Type** and select **OpenID Connect**. This exposes the following controls.

- **Provider name**: The name of the identity provider service. You can select **Google** or **Okta**, both supported natively. Manual entries are also allowed. 

- **Audience (SP entity ID)**: The base URL, for example: `https://leader.yourDomain.com:9000` for a distributed environment. For the IDP, this serves as a unique identifier for the SP (that is, your Cribl Edge instance). 

  > For distributed environments with a [second Leader](deploy-add-second-leader) configured, modify the **Audience** field to point to the load balancer instead of the Leader Node.
  {.box .info}

- **Client ID**: The `client_id` from provider configuration.

- **Client secret**: The `client_secret` from provider configuration.

- **Scope**: Space-separated list of authentication scopes. The default list is: `openid profile email`. If you populate the **User info URL** field, you must add `groups` to this list.

- **Authentication URL**: The full path to the provider's authentication endpoint. Be sure to configure the callback URL at the provider as `<masterServerFQDN>:9000/api/v1/auth/authorization-code/callback`, for example: `https://leader.yourDomain.com:9000/api/v1/auth/authorization-code/callback`.

- **Token URL**: The full path to the provider's access token URL.

- **User info URL**: The full path to the provider's user info URL. Optional; if not provided, Cribl Edge will attempt to gather user info from the ID token returned from the **Token URL**.

- **Logout URL**: The full path to the provider's logout URL. Leave blank if the provider does not support logout or token revocation.

- **User identifier**: JavaScript expression used to derive `userId` from the `id_token` returned by the OpenID provider.

- **Validate certs**: Whether to validate certificates. Defaults to `Yes`. Toggle to `No` to allow insecure self‑signed certificates.

- **Filter type**: Select either **Email allowlist** or **User info filter**. This selection displays one of the following fields:
  - **Email allowlist**: Wildcard list of emails/email patterns that are allowed access.
  - **User info filter**: JavaScript expression to filter against user profile attributes. For example: `name.startsWith("someUser") && email.endsWith("domain.com")`

- **Group name field**: Field in the **User info URL** response (if configured); otherwise, `id_token` that contains the user groups. Defaults to `groups`. 

- **Allow login as Local User**: Toggle to `Yes` to also users to log in using Cribl Edge's local authentication. This enables an extra button called `Log in as Local User` on the Cribl Edge login page. (This option ensures fallback access for local users if SSO/OpenID authentication fails.)

  > To prevent lockout, Cribl strongly recommends enabling **Allow login as Local User** until you're certain that external auth is working as intended. If you do get locked out, see [Manual Password Replacement](#manual-pwd).
  {.box .warning}

- **Email allowlist**: Wildcard list of emails/email patterns that are allowed access.

Note the following details when filling in the form – for example, when using Okta:

- `<Issuer URI>` is the account at the identity provider.

- `Audience` is the URL of the host that will be connecting to the Issuer (for example, `https://leader.yourDomain.com:9000` for a distributed environment). The issuer (Okta, in this example) will redirect back to this site upon authentication success or failure.

- `User info URL` is required, because Okta doesn't encode groups in `id_token`. Azure AD and Google also rely on this field.

### Role Mapping Settings {#role-mapping}

This section is displayed only on **distributed** deployments ([Edge](/edge/setting-up-leader-and-edge-nodes), [Stream](/stream/deploy-distributed)) with an Enterprise License. For details on mapping your external identity provider's configured groups to corresponding Cribl Edge user access Roles, see [External Groups and Roles](roles#groups). The controls here are:

- **Default role**: Default Cribl Edge Role to assign to all groups not explicitly mapped to a Role.

- **Mapping**: Add a mapping for each external user group that you want to map to one or more Cribl roles. Then enter the group and select the role(s).

See also the explanation of [role mapping](#map-groups-to-roles) above.

#### Role Mapping Example

Role mapping UIs will differ from one IDP to another. For this example, we'll look at Okta's. 

You can assign a Cribl Edge Role to each Okta group name, and you can specify a `default` Role for users who are not in any groups.

1. In Cribl Edge, select **Settings** > **Access Management** > **Authentication**.

2. Scroll down to the **ROLE MAPPING** section.

    Cribl recommends that you set the `default` Role to `user`, meaning that this Role will be assigned to users who are not in any groups.

3. Add mappings as needed.

    The Okta group names in the left column are case-sensitive, and must match the values returned by Okta (those you saw earlier when configuring Okta and OIDC).

![Role mapping, concluded](st-usecase-sso-okta_06.9892fa386f.png)
{border="true"}

##  Verify that SSO with Okta Is Working {#verify}

1. Log out of Cribl Edge, and verify that Okta is now an option on the login page.



2. Click **Log in with Okta**.

3. You should be redirected to Okta to authenticate yourself.

4. The OpenID connect flow should complete the authentication process.

## Get Temporary Access Credentials for AWS S3 Buckets {#temp-access}

You can use your SSO/OIDC IDP to issue temporary access credentials so your on-prem Edge Node can access AWS S3 buckets.

Set the `AWS_WEB_IDENTITY_TOKEN_FILE` environment variable. This variable defines a path to a file that contains the OAuth/OIDC provided by the SSO IDP. You'll also need to define `AWS_ROLE_ARN` and `AWS_ROLE_SESSION_NAME`.

You can use [curl/Postman](https://support.okta.com/help/s/article/How-to-get-tokens-for-an-OIDC-application-without-a-browser-using-curlPostman) to make the required API calls.

## Troubleshooting Resources {#troubleshooting}

[Cribl University](https://cribl.io/university/)'s Troubleshooting Criblet on [SSO Integration for Stream On-Prem - Okta](https://university.cribl.io/#/online-courses/6230f17a-3672-4d54-9039-a85687f82690) walks you through this whole configuration flow. To follow the direct course link, first log into your Cribl University account. (To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses – but Cribl’s training is always free of charge.) Once logged in, check out other useful [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0) and [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses.