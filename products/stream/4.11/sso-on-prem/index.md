# SSO in On-Prem Deployments


The general steps to set up a Single Sign-On (SSO) integration between your identity provider and your on-prem Cribl Stream deployment are:

1. In your IDP, [create an application](#create-app).
1. [Submit your app's configuration details](#submit-configuration) to Cribl.
1. In your IDP, [assign groups to your users](#map-groups), matching the Role that each group of users should have in Cribl Stream.
1. [Verify your connection](#verify).
1. Set up [fallback access](#set-up-fallback-access).

The details of specific steps can differ depending on the IDP that you are using.

> The following guides show how to configure SSO with different IDPs:
> 
> - [OIDC with Okta](usecase-sso-okta)
> - [OIDC with Entra ID](usecase-azure-ad)
> - [SAML with Okta](usecase-sso-saml)
> - [SAML with Entra ID](usecase-sso-saml-entra)
> - [SAML with Ping Identity](usecase-sso-ping-identity-saml)
> 
> If you encounter issues when setting up SSO integration,
> refer to [SSO Troubleshooting](sso-troubleshooting-on-prem).
{.box .success}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IDP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IDP instance by creating a chiclet.
Follow the guides for [Okta/SAML](usecase-sso-okta#oidc-chiclet) or [Okta/OIDC](usecase-sso-saml#saml-chiclet-okta) to create a chiclet.

[Snippet not found: content/shared/4.11/snippets/_sso-fallback-on-prem.md]

## Create an Application {#create-app}

In the IDP you are using, create a new application.

Where relevant, select the application type, such as `SAML` or `OIDC`.

If your IDP lets you configure **Application Username** (or similar), make sure its value is NameID.

{{% tabs "saml" "saml" "SAML" "oidc" "OIDC" %}}
{{% tab-item "oidc" %}}

Typically, you will need to provide to this application the callback URI for the Leader Node of your Cribl Stream deployment,
which takes the form `<yourdomain>/api/v1/auth/authorization-code/callback`, for example:
`https://yourDomain.com:9000/api/v1/auth/authorization-code/callback`.

{{% /tab-item %}}
{{% tab-item "saml" %}}

Typically, the application will require you to provide it with URLs for connecting with your Cribl Stream deployment.
You will configure them in Cribl Stream in the next step.

When you are done, return to your IDP app and complete the remaining required configuration
with the values defined.

{{% /tab-item %}}
{{% /tabs %}}

## Submit App Information to Cribl {#submit-configuration}

Next, provide Cribl with essential details about your application to implement the SSO setup on the Cribl side. 

1. In Cribl Stream, in the sidebar, select **Settings**, then **Global**.
1. In **Access Management**, select **Authentication**.
1. From the **Type** dropdown, choose `OpenID Connect` / `SAML 2.0`.

{{% tabs "saml" "saml" "SAML" "oidc" "OIDC" %}}
{{% tab-item "oidc" %}}

1. **Provider name**, select your IDP, if it is present in the list. Otherwise enter an arbitrary identifier for this integration.
1. In the **Audience (Relying Party ID)** field, enter your Cribl Stream Leader's base URL. Do **not** append a trailing slash.

   > If you have a Distributed deployment with a [fallback Leader](deploy-add-second-leader) configured,
   > modify the **Audience (Relying Party ID)** field to point to the load balancer instead of the Leader Node.
   {.box .info}

1. Fill in the remaining fields:

   | Cribl Stream field       | Description |
   | ---------------------- | ----------- |
   | **Client ID**          | The ID of your app, copied from the IDP configuration. |
   | **Client secret**      | The secret for your app, copied from the IDP configuration. |
   | **Scope**              | List of authentication scopes. (Enterprise only) Add the scope `groups` to the default space-separated list of scopes, so that it reads: `openid profile email groups` |
   | **Authentication URL** | The authentication URL from the IDP. It can be named differently in each IDP; for example, in Entra AD, this is **OAuth 2.0 authorization endpoint (v2)**. |
   | **Token URL**          | Access token URL from the IDP. It can be named differently in each IDP; for example, in Entra AD, this is **OAuth 2.0 token endpoint (v2)**. |

{{% /tab-item %}}
{{% tab-item "saml" %}}

1. In the **Audience (SP entity ID)** field, enter your Cribl Stream Leader's base URL. Do **not** append a trailing slash.
   
   Adding an **Audience (SP entity ID)** will automatically fill in **Sign-on callback URL**, **Logout callback URL**, and **Metadata URL**.
   Depending on the IDP you are using, you will need to provide the values of these fields in the IDP configuration.

   > If you have a Distributed deployment with a [fallback Leader](deploy-add-second-leader) configured,
   > modify the **Audience (SP entity ID)** field to point to the load balancer instead of the Leader Node.
   {.box .info}

1. Fill in the remaining fields:

| Cribl Stream field                    | Description                          |
| ----------------------------------- | ------------------------------------ |
| **Issuer (IDP entity ID)**          | The ID of your app, copied from the IDP configuration.|
| **Single sign-on (SSO) URL**        | IDP's single sign-on service URL. |
| **Single logout (SLO) URL**         | IDP's single logout URL. Setting this will enable Cribl-initiated logout (that is, when a user logs out of Cribl, they will be logged out from the IDP as well). |
| **Response validation certificate** | Certificate user to validate signed responses, contains the public key. PEM/Base64 format. |

{{% /tab-item %}}
{{% /tabs %}}

The other fields on the **Authentication** page are not mandatory and will depend on the IDP you are using
and on your specific requirements.

### Configure ID Token to Include Groups Claim

If you are creating an OIDC app,
to ensure that the `groups` scope you configured above is respected,
in your IDP, configure the ID token to include the groups claim.

## Map IDP Groups to Cribl Stream Roles {#map-groups}

Mapping groups to Roles is possible only for Cribl Stream deployments that are in Distributed mode, with an Enterprise license.
With a Standard license, all your external users will be imported to Cribl Stream in the `admin` role.

If you are running Cribl Stream in Single-instance mode, you cannot map IDP groups to Cribl Stream Roles.

As you think through how best to map your IDP groups to Cribl Stream Roles, keep these principles in mind:
- An IDP group can map to more than one Cribl Stream Role.
- A Cribl Stream Role can map to more than one IDP group. 
- If a user has multiple Roles, Cribl Stream applies the union of the most permissive levels of access.
- Cribl Stream automatically assigns the `default` Role to any user who has no mapped Roles.

For details on mapping your external identity provider's configured groups to corresponding Cribl Stream user access Roles, see [External Groups and Roles](roles#groups).

You can assign a Cribl Stream Role to each IDP group name, and you can specify a `default` Role for users who are not in any groups.

Depending on how you configured the group claim (GUID, Object ID, or group name),
you need to have a matching value in Cribl Stream.
The group name must also match the name configured in the IDP.

1. In Cribl Stream, in the sidebar, select **Settings**, go to **Access Management** and then select **Authentication**.
1. Scroll down to **Role Mapping**.

   Cribl recommends that you set the `default` Role to `user`, meaning that this Role will be assigned to users who are not in any groups.

1. Add mappings as needed.

   The IDP group names in the left column are case-sensitive, and must match the values returned by IDP.

![Role mapping section in Cribl Stream with sample mappings.](st-usecase-sso-okta_06.9892fa386f.png)
{border="true" caption="Example Role mapping"}

## Verify that SSO Connection Is Working {#verify}

1. Log out of Cribl Stream, and verify that your IDP is now an option on the login page.
1. Select **Log in with ...** (the button name will depend on your configured IDP).
1. You should be redirected to your IDP to authenticate yourself.
