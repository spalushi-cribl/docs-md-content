# SSO with Microsoft Entra ID and OIDC (On-Prem)


Cribl Edge supports setting up SSO using OIDC to provide user authentication (login/password)
and authorization (by mapping SSO users to Cribl [Roles](authentication#role-mapping)).

This page presents a walkthrough of setting up an OIDC SSO, using Microsoft Entra ID (formerly Azure AD) as the example.



[Snippet not found: content/shared/4.8/snippets/_sso-fallback-on-prem.md]

## Register Your Microsoft Entra ID App

1. Open the [**Microsoft Entra ID**](https://portal.azure.com/) portal and log in.
1. In the left nav's **Manage** section, select **App registrations**.
1. Select **New registration** and provide a name for your app.
1. In **Supported account types**, select **Accounts in this organizational directory only (Default Directory only - Single tenant)**.
1. In **Redirect URI (optional)**, select **Web** and provide the appropriate callback URL for your own Cribl Edge Leader instance, for example: `https://yourDomain.com:9000/api/v1/auth/authorization-code/callback`

### Create and Copy a Client Secret

Next, create a client secret:

1. From your app's left nav, select **Manage** > **Certificate & secrets**. Then select **New client secret**.
1. Add a new client secret with a descriptive name, and an expiration timeframe.
1. Confirm with **Add**. 
1. Immediately copy the **Value** and **Secret ID** from the resulting page. You'll need to paste the **Value** into Cribl Stream's **Authentication** > **Client secret** field [below](#logstream-auth).

> This is the only time the secret is shown! Make sure you copy it while it’s visible. (If you missed your chance, you can start over by creating a new secret.)
>
{.box .warning}

### Configure Token and Claims

Here, you'll add the groups claim to the OIDC ID token.

1. From your app's left nav, select **Manage** > **Token configuration**, then select **Add groups claim**.
1. Under **Select group types to include in Access, ID, and SAML tokens**, select **Groups assigned to the application**.
1. Confirm with **Add**.

> You need to synchronize Microsoft Entra ID with your on-prem Active Directory
> to be able to configure returning `sAMAccountName` for group names.
> Otherwise, Entra ID will return only GUIDs.
> 
> In the **Group claims** modal check **Emit group name for cloud-only groups** to ensure that `sAMAccountName` is added to the **Group** attribute.
>
{.box .info}

## Submit Your App Info to Cribl

Next, provide Cribl with essential details about your application to implement the SSO setup on the Cribl side. 

1. In Cribl Edge, select **Settings** in the top nav, then in **Access Management** choose **Authentication**.
2. From the **Type** dropdown, choose `OpenID Connect`.
3. In **Provider name**, enter an arbitrary identifier for this integration.
4. In the **Audience (Relying Party ID)** field, enter your Cribl Edge Leader's base URL. Do **not** append a trailing slash.

   > If you have a Distributed deployment with a [fallback Leader](deploy-add-second-leader) configured,
   > modify the **Audience (Relying Party ID)** field to point to the load balancer instead of the Leader Node.
   {.box .info}

5. Fill in the remaining fields:

| Cribl Edge field       | Entra ID field |
| ---------------------- | -------------- |
| **Client ID**          | **Application (client) ID** from Entra ID app overview. |
| **Client secret**      | The secret value your copied earlier. |
| **Authentication URL** | **OAuth 2.0 authorization endpoint (v2)** from Entra ID app overview > **Endpoints**. |
| **Token URL**          | **OAuth 2.0 token endpoint (v2)** from Entra ID app overview > **Endpoints**. |

6. If your Cribl Edge is in Enterprise Distributed mode:

   In the **Scope** field, add the scope `groups` to the default space-separated list of scopes,
   so that it reads: `openid profile email groups`.

7. Change the **Filter type** to `User info filter`. 

### v1 Endpoints

You can alternatively use v1 endpoints instead of v2.
In that case, replace the values for **Authentication URL** and **Token URL** with their v1 equivalents from Entra ID.
When using v1 endpoints, you also need to modify the default value for **User identifier**.

In v1 only the `name` field is included in the token by default,
so an acceptable entry here might be: `` `${unique_name || upn || username || name}` ``.
You can check the token fields returned by [enabling](monitoring#change-logging-levels) debug-level logging on Cribl Stream's `auth:sso` channel.

###  Map Microsoft Entra ID Groups to Cribl Stream Roles

Next, map your Microsoft Entra ID groups to Cribl Stream [Roles](roles).

Unless you [synchronize](#configure-token-and-claims) Entra ID with your on-prem Active Directory,
the group names will appear as GUIDs.
You can view the groups and their GUIDs on the Entra ID **Groups** page.

To map the groups:

1. In Entra ID, in the left nav's **Manage** section, select **Groups**.
1. Find the group you want to configure and copy its **Object Id**.
1. In Cribl Edge, go to **Global Settings**.
1. Under **Access Management**, select **Authentication**.
1. In the **Role Mapping** section, under **Mapping**, paste the group's object ID.
1. In the right column, select the Cribl Role you want to assign to this group.
1. Repeat this for all groups you want to configure.
1. Confirm with **Save**.

