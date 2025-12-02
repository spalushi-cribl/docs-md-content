# SSO on Cribl.Cloud


With a Cribl.Cloud account on [certain plan/license tiers](https://cribl.io/pricing/), you can use an identity provider (IdP) to configure Single Sign-On (SSO) using OpenID Connect (OIDC) or Security Assertion Markup Language (SAML) in Cribl.Cloud.

Configuration details vary for different IdPs, but the general steps for setting up SSO between an IdP and Cribl.Cloud deployment are:

1. Set up [fallback access](#fallback-access-cloud).
1. Configure [OIDC](#configure-oidc-cloud) or [SAML](#configure-saml-cloud) SSO and [configure SSO Groups](sso-initial-steps) in the IdP that map to Cribl.Cloud Teams or Roles.
1. [Verify that SSO is working](#verify-cloud).

> The following guides describe how to configure SSO with a specific IdP:
>
> - [OIDC with Okta](oidc-okta-setup)
> - [SAML with Okta](saml-okta-setup)
> - [SSO with Microsoft Entra ID](sso-microsoft-entra-id-cloud)
> - [SSO with Ping Identity](sso-ping-identity-cloud)
{.box .success}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IdP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IdP instance by creating a chiclet.
Follow the guides for [Okta/SAML](saml-okta-setup#saml-chiclet-okta), [Okta/OIDC](usecase-sso-okta#oidc-chiclet), or [Microsoft Entra ID](sso-microsoft-entra-id-cloud#my-apps-chiclet) to create a chiclet.

[Snippet not found: content/shared/4.14/snippets/_sso-fallback-access-cloud.md]

## Bypass SSO with Multiple Organizations {#bypass-sso-multi-orgs-cloud}

> For Cribl.Cloud Government, SSO is mandatory once configured. It does not support bypassing SSO for local accounts or for multi-organization access. For details, see [SSO in Cribl.Cloud Government](/fedramp/fed_access_sso).
{.box .cloud}

If you have SSO configured and you want to sign up for an additional Cribl.Cloud Organization, you need to bypass SSO. Otherwise, you will be forced to log into your existing Organization, because SSO does Home Realm Discovery and recognizes your email address.

In that case, edit your login URL and delete the word identifier. For example:

* Original URL: `https://login.cribl.cloud/u/login/identifier?state=<long_string_of_characters>`

* Edited URL: `https://login.cribl.cloud/u/login/?state=<long_string_of_characters>`

When you use this URL, instead of forcing you through SSO, Cribl.Cloud will ask for a username and password.

If you try to log in with an account that has a bad Permissions state, bypassing SSO with multiple Organizations might not work. In that case, you might be able to resolve the Permission issue using a [fallback user](#fallback-access-cloud).

## General SSO Configuration in Cribl.Cloud {#general-sso-cloud}

> The registration process uses the email domain of the authenticated user who configures SSO in Cribl.Cloud, regardless of the domain that is specified in the IdP configuration.
{.box .info}

The procedures in this section generally describe how to configure [OIDC SSO](#configure-oidc-cloud) and [SAML SSO](#configure-saml-cloud) in Cribl.Cloud using any IdP.

[Snippet not found: content/shared/4.14/snippets/_sso-oidc-cloud.md]

> In Cribl.Cloud Government, the The SAML setup process involves a specific user workflow and distinct security
requirements for compliance. For details, see [SAML Configuration](/fedramp/fed_access_sso/#saml-configuration).
 {.box .warning}

[Snippet not found: content/shared/4.14/snippets/_sso-saml-cloud.md]


[Snippet not found: content/shared/4.14/snippets/_sso-verify-cloud.md]
> In Cribl.Cloud Government,the Test Connection feature provides fewer diagnostic details due to regulatory restrictions.  For details, see [SSO in Cribl.Cloud Government](/fedramp/fed_access_sso).
 {.box .info}

## Update the SAML SSO Certificate {#update-cert-saml-cloud}

Follow these steps to update the public certificate for your SAML SSO configuration:

1. In Cribl.Cloud, in the sidebar, select **Organization > SSO Management**.

1. Scroll down to the end of the **Product-Level Mappings** and select **SAML**.

1. Under **SAML configuration**, select **Update Certificate**. 

1. Enter the public certificate that the IdP uses to sign SAML responses. Paste the entire PEM-encoded certificate, including the `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines.

1. Select **Save**.

## Troubleshooting {#troubleshooting-cloud}

If you encounter issues when setting up SSO, refer to [SSO Troubleshooting in Cribl.Cloud](sso-troubleshooting-cloud).
