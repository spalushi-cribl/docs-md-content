# SSO with Okta and OIDC (On-Prem)


Cribl Edge supports setting up SSO using OIDC to provide user authentication (login/password)
and authorization (by mapping SSO users to Cribl [Roles](authentication#role-mapping)).

This page presents a walkthrough of setting up an OIDC SSO, using Okta as the example.

> If you'd prefer to learn this information in the form of a video course,
> take a look at Cribl University's Troubleshooting Criblet on [SSO Integration for Stream On-Prem - Okta](https://university.cribl.io/#/online-courses/6230f17a-3672-4d54-9039-a85687f82690).
> The course walks you through a configuration flow of SSO wth Okta and OIDC.
> (To follow the direct course link, first log into your Cribl University account - it's free!) 
> This page is a guide for configuring SSO for an on-prem installation.
> For Cribl.Cloud, see [SSO with Okta and OIDC (Cribl.Cloud)](oidc-okta-setup).
{.box .course}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IdP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IdP instance by [creating a chiclet](#oidc-chiclet).

[Snippet not found: content/shared/4.14/snippets/_sso-fallback-access-on-prem.md]

## Create OIDC App Integration {#create-app-oidc}

To create your app integration:

1. In Okta, navigate to the **Applications** section and select **Create App Integration**. 
2. Configure the app integration with the options below:
   
   - **Sign-in method**: `OIDC - OpenID Connect`
   - **Application type**: `Web Application`

3. Proceed with **Next**.

### General Settings

4. Configure the app integration's **General Settings** with the options below:
   
| Setting | Description |
| ------- | ----------- |
| **App integration name** | Your application name.
| **Logo** | (Optional) Upload the Cribl logo. You can use a logo from the Cribl [Press Kit](https://cribl.io/press-kit/).
| **Sign-in redirect URIs** | Your Leader base URL, and with `/api/v1/auth/authorization-code/callback` as the path.
| **Sign-out redirect URIs** | (Optional) Append `/login` to the pre-filled path.

### Assignments

5. In the **Assignments** pane, configure the access granted to Okta users:
   
   - To grant access to Cribl Edge to all Okta users, select **Allow everyone in your organization to access**.
   - To grant access to Cribl Edge to specific Okta groups, select **Limit access to selected groups**.
     Then, in the field below, add the groups you want to include.
     
     After you finish creating the app, if you need to add or remove groups, do that in the **Applications** > **Assignments** tab. 

6. **Save** your application.

In the **Client Credentials** panel, note down **Client ID** and **Client Secret**.
You will need them in the next step.

## Submit Your App Info to Cribl

Next, provide Cribl with essential details about your application to implement the SSO setup on the Cribl side. 

1. In Cribl Edge, in the sidebar, select **Settings**, then **Global**.
1. In **Access Management**, select **Authentication**.
1. From the **Type** dropdown, choose `OpenID Connect`.
1. In the **Provider name** dropdown, select `Okta`.
1. In the **Audience (Relying Party ID)** field, enter your Cribl Edge UI base URL.

   > If you have a Distributed deployment with a [fallback Leader](deploy-add-second-leader) configured,
   > modify the **Audience (Relying Party ID)** field to point to the load balancer instead of the Leader Node.
   {.box .info}

1. In the **Client ID** and **Client secret** fields, enter the respective values
   that you copied from the Okta **Client Credentials** in the previous step.
1. If your Cribl Edge is in Enterprise Distributed mode:

   In the **Scope** field, add the scope `groups` to the default space-separated list of scopes,
   so that it reads: `openid profile email groups`.

### Get Okta App URLs

Next, get the `authentication`, `token`, `userinfo`, and `logout` URLs for your Okta app.

You can get them in JSON format from the `/.well-known/openid-configuration` endpoint:
`https://<tenant>.okta.com/.well-known/openid-configuration` 
(`<tenant>` is your Okta tenant name). For example:
`https://dev-12345678.okta.com/.well-known/openid-configuration`.

> You can use a tool like [jq](https://stedolan.github.io/jq/)
> to automatically extract the URLs from the endpoint's response with the following `curl` command:
> 
> ```
> curl -s https://<tenant>.okta.com/.well-known/openid-configuration | jq '. | {"auth": (.authorization_endpoint), "token":(.token_endpoint), "userinfo":(.userinfo_endpoint), "logout": (.end_session_endpoint)}'
> ```
{.box .success}

Once you have the values, enter them in the respective fields in your Cribl Edge SSO configuration:

| Cribl Edge field | Okta URL |
| ---------------- | -------- |
| **Authentication URL** | `authentication`
| **Token URL** | `token`
| **User info URL** | `userinfo`. Fill it in if you configured Okta to use groups.
| **Logout URL** | `logout`. Fill it in if you want **Account** > **Log out** in Cribl Edge to log the user out globally.<br> This means that when a user selects the **Accounts** > **Log out** link in Cribl Edge, they are logged out of both Cribl Edge and Okta.

## Configure Response to Okta `/userinfo` Endpoint {#userinfo-endpoint}

An Okta tenant's user groups can be mastered either inside Okta, outside Okta, or both.

When Cribl Edge queries the `/userinfo` endpoint, Okta returns the appropriate groups membership of the user.

> See the Okta documentation on [dynamic allow lists](https://developer.okta.com/docs/guides/customize-tokens-dynamic/add-groups-claim-dynamic/)
> and using Okta [together](https://support.okta.com/help/s/article/Can-we-retrieve-both-Active-Directory-and-Okta-groups-in-OpenID-Connect-claims?language=en_US) with Active Directory.
{.box .success}

To configure this behavior, you need to set **Groups claim type** correctly, depending on where user groups are mastered.

1. In Okta, go to your app and open the **Sign On** tab.
2. In the **OpenID Connect ID Token** panel, select **Edit**

{{% tabs "inside" "inside" "Inside Okta" "outside" "Outside Okta" %}}
{{% tab-item "inside" %}}

3. Set **Groups claim type** to `Filter.`
4. Choose **Matches regex** from the dropdown, and enter `.*` as the regex.

{{% /tab-item %}}
{{% tab-item "outside" %}}

3. Set **Groups claim type** to `Expression.`
4. In the **Groups claim expression**, enter an expression field that matches the groups you want passed to Cribl Edge.

   For example, to match on Active Directory groups that contain the string `cribl`, use the following expression:

   ```
   Groups.contains("active_directory", "cribl", 10)
   ```

{{% /tab-item %}}
{{% /tabs %}}

5. Click **Save**.

## Configure ID Token to Include Groups Claim {#id-token}

For Okta to recognize your groups, you must configure the ID token to include your groups claim:

1. In Okta, open the **Security** > **API** page.
2. In the **Authorization Servers** tab, click the edit (pencil) button for the desired Authorization Server. 
3. In the resulting page, select the **Claims** tab.
4. If your groups claim already exists, click the edit (pencil) button. Otherwise, click **Add Claim**.
5. In the **Include in token type** drop-downs, choose `ID Token` and `Always`, respectively.

![Including the groups claim in the token ID](st-usecase-sso-okta_05.2f70d1ce5c.png)
{border="true" scale="50%"}

6. Configure the remaining settings in the way that suits your groups claim.
7. Click **Save** (or **Create** if you're adding the claim for the first time).

## Map Okta Groups to Cribl Edge Roles

Mapping groups to Roles is possible only for Cribl Edge deployments that are in Distributed mode, with an Enterprise license.
With a Standard license, all your external users will be imported to Cribl Edge in the `admin` role.

If you are running Cribl Edge in Single-instance mode, you cannot map Okta groups to Cribl Edge Roles, although you can still set up SSO with Okta.

As you think through how best to map your Okta groups to Cribl Edge Roles, keep these principles in mind:
- An Okta group can map to more than one Cribl Edge Role.
- A Cribl Edge Role can map to more than one Okta group. 
- If a user has multiple Roles, Cribl Edge applies the union of the most permissive levels of access.
- Cribl Edge automatically assigns the `default` Role to any user who has no mapped Roles.

For details on mapping your external identity provider's configured groups to corresponding Cribl Edge user access Roles, see [External Groups and Roles](roles#groups).

You can assign a Cribl Edge Role to each Okta group name, and you can specify a `default` Role for users who are not in any groups.

1. In Cribl Edge, in the sidebar, select **Settings**, go to **Access Management** and then select **Authentication**.
1. Scroll down to **Role Mapping**.

   Cribl recommends that you set the `default` Role to `user`, meaning that this Role will be assigned to users who are not in any groups.

1. Add mappings as needed.

   The Okta group names in the left column are case-sensitive, and must match the values returned by Okta (those you saw earlier when configuring Okta and OIDC).

![Role mapping section in Cribl Edge with sample mappings.](st-usecase-sso-okta_06.9892fa386f.png)
{border="true" caption="Example Role mapping"}

## Verify that SSO with Okta Is Working {#verify}

1. Log out of Cribl Edge, and verify that Okta is now an option on the login page.
1. Select **Log in with Okta**.
1. You should be redirected to Okta to authenticate yourself.
1. The OpenID connect flow should complete the authentication process.

## Get Temporary Access Credentials for AWS S3 Buckets {#temp-access}

You can use your SSO/OIDC IdP to issue temporary access credentials so your on-prem Edge Node can access AWS S3 buckets.

Set the `AWS_WEB_IDENTITY_TOKEN_FILE` environment variable. This variable defines a path to a file that contains the OAuth/OIDC provided by the SSO IdP. You'll also need to define `AWS_ROLE_ARN` and `AWS_ROLE_SESSION_NAME`.

You can use [curl/Postman](https://support.okta.com/help/s/article/How-to-get-tokens-for-an-OIDC-application-without-a-browser-using-curlPostman) to make the required API calls.

## OIDC/Okta Chiclet Setup (Optional) {#oidc-chiclet}

If you want to initiate login from an Okta instance on which you have configured OIDC authentication,
an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.
2. Find the OIDC application created earlier in the [OIDC/Okta Setup Example](#create-app-oidc).
3. Select that application, and in the **General** tab's **General Settings** section, select **Edit**.
4. In the **Initiate login URI** field, enter the `<host>:<port>` of your Cribl Leader Node.
5. Confirm with **Save** to complete the chiclet.

## Troubleshooting

> If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-on-prem).
{.box .info}
