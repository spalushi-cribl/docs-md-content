# SSO with Ping Identity (On-Prem)


This topic explains how to configure Single Sign-On (SSO) using [Security Assertion Markup Language (SAML)](#saml-ping-on-prem) in Cribl with Ping Identity as the identity provider (IdP).

To configure Ping Identity as an IdP, refer to the [Ping Identity documentation](https://docs.pingidentity.com/).

> Before you begin, make sure to [enable TLS for UI access](securing-tls).
> 
> This page describes how to configure SSO for on-prem installations. For Cribl.Cloud, see [SSO with Ping Identity (Cribl.Cloud)](sso-ping-identity-cloud).
{.box .info}

[Snippet not found: content/shared/4.13/snippets/_sso-fallback-access-on-prem.md]

## SAML SSO with Ping Identity {#saml-ping-on-prem}

> This section provides SAML SSO configuration details that are specific to using Ping Identity as the IdP. For general step-by-step procedures, read [Configure SAML SSO](#configure-saml-on-prem).
{.box .info}

Configuring SSO in Cribl requires a SAML 2.0 application in Ping Identity. Read [Add a SAML application](https://docs.pingidentity.com/pingone/pingone_tutorials/p1_p1tutorial_add_a_saml_app.html) in the Ping Identity documentation for more information. Make sure that the SAML application includes at least one user so that you can test the configuration.

When you create the SAML application as you [configure SAML SSO](#configure-saml-on-prem), provide the values from the following fields in Cribl:

| Field in Cribl | Field in Ping Identity |
| -------------- | ---------------------- |
| **Sign-on callback URL** | **ACS URLs** |
| **Audience (SP entity ID)** | **Entity ID** |

After you save the SAML application, you will need the values from the following fields to finish SSO setup in Cribl:

| Field in Ping Identity | Field in Cribl |
| ---------------------- | -------------- |
| **Issuer ID** | **Issuer (IDP entity ID)** |
| **Single Signon Service** | **Single sign-on (SSO) URL** |
| **Signing Certificate** | **Response validation certificate** |

> The field names and documentation for adding a SAML application in Ping Identity might change without notice due to product changes in Ping Identity. Refer to the [Ping Identity documentation](https://docs.pingidentity.com/) for the latest information.
{.box .info}

## General SSO Configuration in Cribl {#general-sso-on-prem}

The procedures in this section generally describe how to configure [SAML SSO](#configure-saml-on-prem) in Cribl using any IdP.

[Snippet not found: content/shared/4.13/snippets/_sso-saml-on-prem.md]

[Snippet not found: content/shared/4.13/snippets/_sso-verify-on-prem.md]

## Troubleshooting {#troubleshooting}

If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting (On-Prem)](sso-troubleshooting-on-prem).
