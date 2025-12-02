# SSO with Microsoft Entra ID and SAML (On-Prem)


Cribl Stream supports setting up SSO using SAML to provide user authentication (login/password)
and authorization (by mapping SSO users to Cribl [Roles](authentication#role-mapping)).

This page presents a walkthrough of setting up a SAML SSO, using Microsoft Entra ID (formerly Azure AD) as the example.

> This page is a guide for configuring SSO for an on-prem installation.
> For Cribl.Cloud, see [SSO with Microsoft Entra ID and SAML (Cribl.Cloud)](saml-azure-setup).
{.box .info}

[Snippet not found: content/shared/4.9/snippets/_sso-fallback-on-prem.md]

## Create an Enterprise Application {#create-app-saml-azure-ad}

To create your app integration:

1. In Microsoft Entra ID's top nav, select **Add**, then **Enterprise application**, then **Create your own application**.
1. Name your new app `Cribl` (or any name you prefer).
1. Select **Integrate any other application you don't find in the gallery (Non‑gallery)**.
1. Confirm with **Create**.

## Submit Your App Info to Cribl {#tell-cribl-saml-azure-ad}

Next, provide Cribl with essential details about your application to implement the SSO setup on the Cribl side. 

1. In Cribl Stream, in the sidebar, select **Settings**, then **Global**.
1. In **Access Management**, select **Authentication**.
2. From the **Type** dropdown, choose `SAML 2.0`.
3. In the **Audience (SP entity ID)** field, enter the base URL of your Cribl Stream instance, for example, `https://yourDomain.com:9000`. Do **not** append a trailing slash.

   > If you have a Distributed deployment with a [fallback Leader](deploy-add-second-leader) configured,
   > modify the **Audience (SP entity ID)** field to point to the load balancer instead of the Leader Node.
   {.box .info}

4. Then fill in remaining information from the Entra ID's **SAML-based Sign-on** page:

| Cribl Stream field                    | Entra ID field                                   |
| ----------------------------------- | ------------------------------------------------ |
| **Issuer (IDP entity ID)**          | Microsoft Entra Identifier                       |
| **Single sign-on (SSO) URL**        | Login URL                                        |
| **Single logout (SLO) URL**         | Logout URL                                       |
| **Response validation certificate** | Certificate (Base64) under **SAML Certificates** |
| **Group name field**                | `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` |

5. Select **Save** to complete your submission.

## Configure Single Sign-On {#config-sso-azure-ad}

From Microsoft Entra ID's left nav, under **Manage**, select **Single sign‑on**, then **SAML**, to open the **SAML-based Sign-on** page.
Then, edit the fields below with the information from the Cribl **Authentication** page:

1. In **Identifier (Entity ID)**, select **Add identifier** and enter the **Audience URI** value from the Cribl **Authentication** page.
1. In **Reply URL (Assertion Consumer Service URL)**, select **Add reply URL** and enter the **Sign-on callback URL** value from the Cribl **Authentication** page.
1. In **Logout Url (Optional)**, enter the  **Logout callback URL** value from the Cribl **Authentication** page.

## Map Entra ID Groups to Cribl Stream Roles

Mapping groups to Roles is possible only for Cribl Stream deployments that are in Distributed mode, with an Enterprise license.
With a Standard license, all your external users will be imported to Cribl Stream in the `admin` role.

If you are running Cribl Stream in Single-instance mode, you cannot map Entra ID groups to Cribl Stream Roles, although you can still set up SSO with Entra ID.

As you think through how best to map your Entra ID groups to Cribl Stream Roles, keep these principles in mind:
- An Entra ID group can map to more than one Cribl Stream Role.
- A Cribl Stream Role can map to more than one Entra ID group. 
- If a user has multiple Roles, Cribl Stream applies the union of the most permissive levels of access.
- Cribl Stream automatically assigns the `default` Role to any user who has no mapped Roles.

For details on mapping your external identity provider's configured groups to corresponding Cribl Stream user access Roles, see [External Groups and Roles](roles#groups).

You can assign a Cribl Stream Role to each Entra ID group name, and you can specify a `default` Role for users who are not in any groups.

1. In Cribl Stream, in the sidebar, select **Settings**, go to **Access Management** and then select **Authentication**.
1. Scroll down to **Role Mapping**.

   Cribl recommends that you set the `default` Role to `user`, meaning that this Role will be assigned to users who are not in any groups.

1. Add mappings as needed.

   The Entra ID group names in the left column are case-sensitive, and must match the values returned by Entra ID .

## Assign Groups {#assign-groups-azure-ad}

Now, map your Cribl groups to your Entra ID groups.
From Microsoft Entra ID's left nav: 

1. Select **Manage**, then **Users and groups**, then select **Add user/group**.
2. Add the Cribl groups you created above

> You need to synchronize Microsoft Entra ID with your on-prem Active Directory
> to be able to configure returning `sAMAccountName` for group names.
> Otherwise, Entra ID will return only GUIDs.
>
> For the groups claim configuration, select `Groups assigned to the application` for association
> and set **Source attribute** to `sAMAccountName`.
> Enable **Emit group name for cloud-only groups** to also return the `sAMAccountName` attribute for cloud-only groups.
>
{.box .warning}

## Verify that SSO with Entra ID Is Working {#verify}

1. Log out of Cribl Stream, and verify that Entra ID is now an option on the login page.
1. Select **Log in with Saml 2.0**.
1. You should be redirected to Entra ID to authenticate yourself.
1. The SAML connect flow should complete the authentication process.

