# SSO with Ping Identity (Cribl.Cloud)


This topic provides details to help you configure Single Sign-On (SSO) using [OpenID Connect (OIDC)](#oidc-ping-cloud) or [Security Assertion Markup Language (SAML)](#saml-ping-cloud) with Ping Identity as the identity provider (IdP).

To configure Ping Identity as an IdP, refer to the [Ping Identity documentation](https://docs.pingidentity.com/).

> This page describes how to configure SSO for Cribl.Cloud. For on-prem installations, see [SSO with Ping Identity (On-Prem)](sso-ping-identity-on-prem).
{.box .info}

[Snippet not found: content/shared/4.12/snippets/_sso-fallback-access-cloud.md]

## OIDC SSO with Ping Identity {#oidc-ping-cloud}

> This section provides OIDC SSO configuration details that are specific to using Ping Identity as the IdP. For general step-by-step procedures, read [Configure OIDC SSO](#configure-oidc-cloud).
{.box .info}

Configuring OIDC SSO requires an OIDC application in Ping Identity. Read [Adding an application](https://docs.pingidentity.com/pingone/applications/p1_applications_add_applications.html) and [Editing an application - OIDC](https://docs.pingidentity.com/pingone/applications/p1_edit_application_oidc.html) in the Ping Identity documentation for more information. Make sure that the OIDC application includes at least one user so that you can test the configuration.

When you create the OIDC application as you [configure OIDC SSO](#configure-oidc-cloud), provide the values from the following fields in Cribl.Cloud:

| Field in Cribl.Cloud | Field in Ping Identity |
| -------------------- | ---------------------- |
| **App integration name** | **Application Name** |
| **Sign-in redirect URIs** | **Redirect URIs** |
| **Sign-out redirect URIs** | **Signoff URLs** |
| **Groups map key value** | **Resources > Attributes** |
| **Scopes** | **Resources > Scopes** |

In addition, configure the OIDC application to use the following settings:

* **Response Type**: Code 
* **Grant Type**: Authorization Code
* **PKCE Enforcement**: Optional 
* **Refresh Token Configuration**: Configure according to your best practices
* **Token Endpoint Authentication Method**: Client Secret Post

[Update the attribute mapping for your OIDC application](https://docs.pingidentity.com/pingone/applications/p1_customizing_oidc_attributes_for_application.html) in Ping Identity to match these settings:

| Attributes | PingOne Mappings | Scopes |
| ---------- | ---------------- | ------ |
| `sub` | `User ID` | `openid` |
| `email` | `Email Address` | `openid` |
| `family_name` | `Family Name` | `openid` |
| `given_name` | `Given Name` | `openid` |
| `groups` | `Group Names` | `openid` |

Make sure to configure an [Organization Owner group](sso-initial-steps) so that you aren't locked out of your Cribl Organization.

After you save the OIDC application, you will need the values from the following fields to finish SSO setup in Cribl.Cloud:

| Field in Ping Identity | Field in Cribl |
| ---------------------- | -------------- |
| **Client ID** | **Client ID** |
| **Client Secret** | **Client secret** |
| **Issuer ID** | **Issuer URL** |

> The field names and documentation for adding an OIDC application in Ping Identity might change without notice due to product changes in Ping Identity. Refer to the [Ping Identity documentation](https://docs.pingidentity.com/) for the latest information.
{.box .info}

## SAML SSO with Ping Identity {#saml-ping-cloud}

> This section provides SAML SSO configuration details that are specific to using Ping Identity as the IdP. For general step-by-step procedures, read [Configure SAML SSO](#configure-saml-cloud).
{.box .info}

Configuring SSO in Cribl.Cloud requires a SAML 2.0 application in Ping Identity. Read [Add a SAML application](https://docs.pingidentity.com/pingone/pingone_tutorials/p1_p1tutorial_add_a_saml_app.html) in the Ping Identity documentation for more information. Make sure that the SAML application includes at least one user so that you can test the configuration.

When you create the SAML application as you [configure SAML SSO](#configure-saml-cloud), provide the values from the following fields in Cribl.Cloud:

| Field in Cribl.Cloud | Field in Ping Identity |
| -------------------- | ---------------------- |
| **Single Sign on URL** | **ACS URLs** |
| **Audience URI** | **Entity ID** |

[Update the attribute mapping for your SAML application](https://docs.pingidentity.com/pingone/applications/p1_edit_application_saml.html) in Ping Identity to match these settings:

| Attributes | PingOne Mappings |
| ---------- | ---------------- |
| `sub` | `User ID` |
| `email` | `Email Address` |
| `family_name` | `Family Name` |
| `given_name` | `Given Name` |
| `groups` | `Group Names` |

Make sure to configure an [Organization Owner group](sso-initial-steps) so that you aren't locked out of your Cribl Organization.

After you save the SAML application, you will need the values from the following fields to finish SSO setup in Cribl.Cloud:

| Field in Ping Identity | Field in Cribl.Cloud |
| ---------------------- | -------------------- |
| **Single Signon Service** | **IDP Login/Logout URL** |
| **Issuer ID** | **IDP issuer** |
| **Signing Certificate** | **X.509 certificate (base64-encoded)** |

> The field names and documentation for adding a SAML application in Ping Identity might change without notice due to product changes in Ping Identity. Refer to the [Ping Identity documentation](https://docs.pingidentity.com/) for the latest information.
{.box .info}

## General SSO Configuration in Cribl.Cloud {#general-sso-cloud}

The procedures in this section generally describe how to configure [OIDC SSO](#configure-oidc-cloud) and [SAML SSO](#configure-saml-cloud) in Cribl.Cloud using any IdP.

[Snippet not found: content/shared/4.12/snippets/_sso-oidc-cloud.md]

[Snippet not found: content/shared/4.12/snippets/_sso-saml-cloud.md]

[Snippet not found: content/shared/4.12/snippets/_sso-verify-cloud.md]

## Troubleshooting {#troubleshooting-cloud}

If you encounter issues when setting up SSO, refer to [SSO Troubleshooting in Cribl.Cloud](sso-troubleshooting-cloud).
