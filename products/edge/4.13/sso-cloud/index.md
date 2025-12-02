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

[Snippet not found: content/shared/4.13/snippets/_sso-fallback-access-cloud.md]

## Bypass SSO with Multiple Organizations {#bypass-sso-multi-orgs-cloud}

If you have SSO configured and you want to sign up for an additional Cribl.Cloud Organization, you need to bypass SSO. Otherwise, you will be forced to log into your existing Organization, because SSO does Home Realm Discovery and recognizes your email address.

In that case, edit your login URL and delete the word identifier. For example:

* Original URL: `https://login.cribl.cloud/u/login/identifier?state=<long_string_of_characters>`

* Edited URL: `https://login.cribl.cloud/u/login/?state=<long_string_of_characters>`

When you use this URL, instead of forcing you through SSO, Cribl.Cloud will ask for a username and password.

If you try to log in with an account that has a bad Permissions state, bypassing SSO with multiple Organizations might not work. In that case, you might be able to resolve the Permission issue using a [fallback user](#fallback-access-cloud).

## General SSO Configuration in Cribl.Cloud {#general-sso-cloud}

The procedures in this section generally describe how to configure [OIDC SSO](#configure-oidc-cloud) and [SAML SSO](#configure-saml-cloud) in Cribl.Cloud using any IdP.

> The registration process uses the email domain of the authenticated user who configures SSO in Cribl.Cloud, regardless of the domain that is specified in the IdP configuration.
{.box .info}

[Snippet not found: content/shared/4.13/snippets/_sso-oidc-cloud.md]

[Snippet not found: content/shared/4.13/snippets/_sso-saml-cloud.md]

[Snippet not found: content/shared/4.13/snippets/_sso-verify-cloud.md]

## Troubleshooting {#troubleshooting-cloud}

If you encounter issues when setting up SSO, refer to [SSO Troubleshooting in Cribl.Cloud](sso-troubleshooting-cloud).
