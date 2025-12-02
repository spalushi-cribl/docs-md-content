# SSO with Ping Identity and SAML (Cribl.Cloud)


This page presents a walkthrough of setting up a SAML SSO, using Ping Identity as the example.

> This page is a guide for configuring SSO for Cribl.Cloud. For on-prem installations, see [SSO with Ping Identity and SAML (on-prem)](usecase-sso-ping-identity-saml).
{.box .info}

[Snippet not found: content/shared/4.11/snippets/_sso-fallback-access.md]

## Retrieve URLs in Cribl​ {#retrieve-urls-cribl}

In Cribl, retrieve the URLs you will need to create your Ping Identity application for SSO.

1. In the sidebar, select **Organization** > **SSO Management**.

1. Scroll down to **Web Application Settings** and select **SAML**.

1. Note the values for **Single Sign on URL** and **Audience URI**. **Single Sign on URL** lists two URLs that you use for SAML configuration:

   * `https://login.cribl.cloud/login/callback?connection=<$organizationID>` is the URL you will use for the connection.

   * `https://manage.cribl.cloud/api/assert` is used during setup to test the connection. After you successfully test the connection, save the configuration and replace the second URL with the first one.

## Create SAML 2.0 Application in Ping Identity {#create-app-ping-identity}

To create your SAML 2.0 application in Ping Identity:

1. In Ping Identity, in the sidebar, select **Environments** and choose your desired environment.

1. In the right panel, select **Manage Environment**.

1. Follow the [Ping Identity tutorial to add a SAML application](https://docs.pingidentity.com/pingone/pingone_tutorials/p1_p1tutorial_add_a_saml_app.html). Enter the URLs that you [generated in Cribl](#generate-urls-cribl) as follows:
   
   | Field in Ping Identity | Field in Cribl |
   | ---------------------- | -------------- |
   | **ACS URLs** | **Single Sign on URL** |
   | **Entity ID** | **Audience URI** |

1. After you save the application, select it to display the **Single Signon Service**, **Issuer ID**, and **Download Signing Certificate** button. You will need this information to [submit your application information to Cribl](#submit-app-info-cribl).

## Submit Your App Info to Cribl {#submit-app-info-cribl}

After you’ve created the SAML application in Ping Identity, provide Cribl with the essential metadata about your application to implement SSO setup on the Cribl side.

1. In the sidebar, select **Organization** > **SSO Management**.

1. Scroll down to **Web Application Settings** and select **SAML**. The **Web Application Settings** and **SAML Assertion Mappings** are prefilled based on the information you’ve registered with Cribl.

1. Under **SAML configuration**, enter the information from your Ping Identity application as follows:

    | Field in Cribl | Field in Ping Identity |
    | -------------- | ---------------------- |
    | **IDP Login/Logout URL** | **Single Signon Service** |
    | **IDP issuer** | **Issuer ID** |
    | **X.509 certificate (base64-encoded)** | **Signing Certificate** |

1. Select **Save**.

## Verify that SSO with Ping Identity Is Working {#verify}

1. Log out of Cribl Stream, and verify that the **Log in with Saml 2.0** option appears on the login page.

1. Select **Log in with Saml 2.0**.

You should be redirected to Ping Identity to authenticate yourself, and the SAML connect flow should complete the authentication process.

## Troubleshooting {#troubleshooting}

If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-cloud).
