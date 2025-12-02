# SSO on Cribl.Cloud


With a Cribl.Cloud account on [certain plan/license tiers](https://cribl.io/pricing/),
you can use an identity provider (IDP) to set up Single Sign-On (SSO) for your Cribl.Cloud portal.

The general steps to set up an integration between your IDP and your Cribl.Cloud deployment are:

1. In your IDP, [configure user groups](sso-initial-steps) that map to Cribl.Cloud's Teams or to predefined Roles.
1. Set up [fallback access](#set-up-fallback-access).
1. In your IDP, [create an application](#create-app).
1. [Submit your app's configuration details](#submit-configuration) to Cribl.
1. [Verify your connection](#verify).
1. [Link existing Cribl.Cloud users](#link-existing-users).

The details of specific steps can differ depending on the IDP that you are using.

> The following guides show how to configure SSO with different IDPs:
>
> - [OIDC with Okta](oidc-okta-setup)
> - [SAML with Okta](saml-okta-setup)
> - [SAML with Entra ID](saml-azure-setup)
> - [SAML with Ping Identity](saml-ping-identity-setup)
>
> If you encounter issues when setting up SSO integration,
> refer to [SSO Troubleshooting](sso-troubleshooting-cloud).
{.box .success}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IDP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IDP instance by creating a chiclet.
Follow the guides for [Okta/SAML](saml-okta-setup#saml-chiclet-okta), [Okta/OIDC](usecase-sso-okta#oidc-chiclet), or [Entra ID/SAML](saml-azure-setup#my-apps-chiclet) to create a chiclet.

[Snippet not found: content/shared/4.11/snippets/_sso-fallback-access.md]

## Create an Application {#create-app}

In the IDP you are using, create a new application.

Where relevant, select the application type, such as `SAML` or `OIDC`
(for example, in Okta you would select `SAML 2.0` as the **Sign‑in method**,
and in PingOne, you would choose application type: **SAML application**).

In Cribl Edge, locate the URLs for your Cribl.Cloud deployment:

1. On the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, then **SSO Management**.
1. Above **Web Application Settings**, select **OIDC** or **SAML** and look for the required URLs below:

{{% tabs "saml" "saml" "SAML" "oidc" "OIDC" %}}
{{% tab-item "oidc" %}}

   - Sign-in URIs: 
     - `https://login.cribl.cloud/login/callback` is used for the connection.
     - `https://manage.cribl.cloud/organizations/<organizationID>/sso`  is used during setup to test the connection.
   - Sign-out URIs:
     - `https://login.cribl.cloud/v2/logout`

{{% /tab-item %}}
{{% tab-item "saml" %}}

   - Sign-in URIs: 
     - `https://login.cribl.cloud/login/callback?connection=<organization-id>` is used for the connection.
     - `https://manage.cribl.cloud/api/assert` is used during setup to test the connection.
   - Audience URI:
     - `urn:auth0:cribl-cloud:<organization-id>`

{{% /tab-item %}}
{{% /tabs %}}

1. Provide the required redirect URIs to your IDP.
   The exact name and location of the configuration to fill in will depend on your IDP
   (for example, in Entra ID, you would use **Audience URI** to fill in **Identifier (Entity ID)**).

1. If you are creating a SAML application, in your IDP, configure the following attribute claims:

   | Claim Name    | Value            |
   | ------------- | ---------------- |
   | `email`       | `user.email`     |
   | `given_name`  | `user.firstName` |
   | `family_name` | `user.lastName`  |

1. Next, in the SAML app configure **Group Attribute Statements** to include groups.
   The filter depends on the type of groups you are using.

   - If you are using static groups, use `Cribl.*` as the filter.
   - If you are using dynamic groups with [Teams](teams), you can use `.*` as the filter.

     In this case, we strongly recommend using a more specific regex that will match only the necessary groups.

> If creating an OIDC application, you must use backchannel authentication. Cribl.Cloud does not support front-channel authentication via OIDC.
{.box .info}

### Custom Scopes and Groups

In an OIDC connection, if your IDP uses a custom key to send group information,
you can indicate it in **Web Application Settings** > **Groups map key value**.

You can also configure the scopes used to request access to user information from the IDP under **Scopes**.

## Submit App Information to Cribl {#submit-configuration}

Next, provide Cribl with essential details about your application to implement the SSO setup on the Cribl side.

In **Web Application Settings**, fill in the **Cribl Cloud SSO Settings** section with the following details from your IDP client configuration:

{{% tabs "saml" "saml" "SAML" "oidc" "OIDC" %}}
{{% tab-item "oidc" %}}

- Client ID
- Client Secret
- Issuer URL

{{% /tab-item %}}
{{% tab-item "saml" %}}

- IDP Login/Logout URL
- IDP issuer
- X.509 certificate (base64-encoded)

{{% /tab-item %}}
{{% /tabs %}}

## Verify that SSO Connection Is Working {#verify}

You can now test whether your SSO connection is configured correctly.
Navigate to **Cribl Cloud SSO settings** and select **Test Connection**.

You will see an error message when the test encounters a configuration error.

[Snippet not found: content/shared/4.11/snippets/_sso-final-step-cloud.md]
