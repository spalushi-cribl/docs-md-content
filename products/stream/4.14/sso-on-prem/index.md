# SSO in On-Prem Deployments


With a Cribl account on [certain plan tiers](https://cribl.io/pricing/), you can use an identity provider (IdP) to configure Single Sign-On (SSO) using OpenID Connect (OIDC) or Security Assertion Markup Language (SAML) in Cribl.

Configuration details vary for different IdPs, but the general steps for setting up SSO between an IdP and an on-prem Cribl deployment are:

1. Set up [fallback access](#fallback-access-on-prem).
1. Configure [OIDC](#configure-oidc-on-prem) or [SAML](#configure-saml-on-prem) SSO.
1. [Verify that SSO is working](#verify-cloud).

> The following guides describe how to configure SSO with a specific IdPs:
> 
> - [OIDC with Okta](usecase-sso-okta)
> - [SAML with Okta](usecase-sso-saml)
> - [SSO with Microsoft Entra ID](sso-microsoft-entra-id-on-prem)
> - [SSO with Ping Identity](sso-ping-identity-on-prem)
> 
> If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-on-prem).
{.box .success}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IdP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IdP instance by creating a chiclet.
Follow the guides for [Okta/SAML](usecase-sso-okta#oidc-chiclet) or [Okta/OIDC](usecase-sso-saml#saml-chiclet-okta) to create a chiclet.

[Snippet not found: content/shared/4.14/snippets/_sso-fallback-access-on-prem.md]

## General SSO Configuration in Cribl {#general-sso-on-prem}

The procedures in this section generally describe how to configure [OIDC SSO](#configure-oidc-on-prem) and [SAML SSO](#configure-saml-on-prem) in Cribl using any IdP.

[Snippet not found: content/shared/4.14/snippets/_sso-oidc-on-prem.md]

[Snippet not found: content/shared/4.14/snippets/_sso-saml-on-prem.md]

[Snippet not found: content/shared/4.14/snippets/_sso-verify-on-prem.md]

## Troubleshooting

If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-on-prem).
