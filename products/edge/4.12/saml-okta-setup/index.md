# SSO with Okta and SAML (Cribl.Cloud)


This page presents a walkthrough of setting up a SAML SSO, using Okta as the example.

> This page is a guide for configuring SSO for a Cribl.Cloud deployment.
> For information about an on-prem installation, see [SSO with Okta and SAML (on-prem)](usecase-sso-saml).
{.box .info}

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IDP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IDP instance by [creating a chiclet](#saml-chiclet-okta).

[Snippet not found: content/shared/4.12/snippets/_sso-fallback-access-cloud.md]

## Create SAML 2.0 App Integration {#create-app-saml-okta}

To create your app integration:

1. In Okta, navigate to the **Applications** section of your Okta environment and select **Create App Integration**. 
2. Create the app integration with **Sign‑in method**: `SAML 2.0`.
3. Provide a name for your app and proceed with **Next**.

## Configure SAML Settings {#config-saml-setting-okta}

Before you start configuring SAML settings on Okta side, gather the required information from your Cribl.Cloud Organization.

1. In the sidebar, under **Organization**, select **SSO Management**.

2. Scroll down to the **Web Application Settings** section and select **SAML**.

3. Note down the values for **Single Sign on URL** and **Audience URI**.

   **Single Sign on URL** lists two URLs that you use for SAML configuration.

   - `https://login.cribl.cloud/login/callback?connection=<organizationId>` is the URL you will use for the connection.
   - `https://manage.cribl.cloud/api/assert` is used during setup to test the connection.
   After you have successfully tested the connection, save the configuration and replace the second URL with the first one.

![SSO information for configuring SAML integration](cloud-sso-saml-azure-ad-config-4.7.png)
{border="true"}

4. Now, go to your Okta application.

5. In the **Configure SAML** tab, in the **SAML Settings** section, you will use information you get from your Cribl.Cloud Organization.
Configure the following options:

   - **Single sign-on URL**: enter the first of two URLs from **Single Sign on URL**:
   `https://login.cribl.cloud/login/callback?connection=<organizationId>`
   - **Audience URI (SP Entity ID)**: enter the **Audience URI** from your Cribl.Cloud SAML settings. For example:
   `urn:auth0:cribl-cloud-prod:<organizationId>`
   - **Application username**: enter `Email`

     > The `nameidentifier` assertion in SAML responses must be the user's `Email`.
     {.box .info}

6. Select **Show Advanced Settings**. Navigate down and configure a single row of **Other Requestable SSO URLs**, as follows:

   - **URL** is required to test your connection. Get it from your Cribl.Cloud Organization's **SSO** > **SAML** tab, where it is the  second **Single sign‑on URL**. It will be in this format:
   `https://manage.cribl.cloud/api/assert`
   - **Index**: Set this to `0`.

## Configure Attribute Statements {#config-attributes-okta}

1. Configure **Attribute Statements** for these attributes, as shown below:

   | Name          | Value            |
   | ------------- | ---------------- |
   | `email`       | `user.email`     |
   | `given_name`  | `user.firstName` |
   | `family_name` | `user.lastName`  |

1. Next, configure **Group Attribute Statements** to include `groups`.
   The filter depends on the type of groups you are using.

   - If you are using static groups, use `Cribl.*` as the filter:
  
     | Name     | Filter                                |
     | -------- | ------------------------------------- |
     | `groups` | Matches regex: `Cribl.*`              |

   - If you are using dynamic groups with [Teams](teams), you can use `.*` as the filter:

     | Name     | Filter                                |
     | -------- | ------------------------------------- |
     | `groups` | Matches regex: `.*`                   |

     In this case, we strongly recommend using a more specific regex that will match only the necessary groups.

1. Save your app integration.

## Submit Your App Info to Cribl {#tell-cribl-saml-okta}

After you've created the SAML app integration in your IDP, provide Cribl with the essential metadata about your application to implement SSO setup on the Cribl side. 

1. In Cribl Edge, on the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, then **SSO Management**.
1. Above **Web Application Settings**, select **SAML**. 
1. The **Web Application Settings** will be prefilled for you, and Cribl will also prefill the **SAML Assertion Mappings** based on the information you've registered with Cribl. So you only need to fill in the **SAML configuration** section with details from your IDP client configuration.
1. Return to your Okta environment to the **Sign On** tab and in the right pane, select **View SAML setup instructions**.
   Use the provided fields to fill in the information in Cribl.Cloud:

   | Cribl.Cloud field                      | Okta field                           |
   | -------------------------------------- | ------------------------------------ |
   | **IDP Login/Logout URL**               | Identity Provider Single Sign-On URL |
   | **IDP issuer**                         | Identity Provider Issuer             |
   | **X.509 certificate (base64-encoded)** | X.509 Certificate                    |


## SAML/Okta Chiclet Setup (Optional) {#saml-chiclet-okta}

If you want to initiate login from your Okta instance with SAML authentication configured, an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.
1. Select **Browse App Catalog**.
1. From the resulting catalog, use the search bar to find and select the `Bookmark App` application.
1. From that application's page, select **Add Integration**.
1. On the **General settings** page, enter an **Application label** that will identify this app as supporting Cribl.Cloud login. (`Cribl.Cloud` is a good choice, but the label is arbitrary.)
1. In the **URL** field, enter `https://manage.cribl.cloud/login?connection=<organizationId>` (where `<organizationId>` is your Cribl.Cloud Organization's ID).
1. Confirm with **Done**.
1. Select **Assign** and assign all of the Cribl.Cloud groups to the application.
1. The Cribl.Cloud chiclet should now be available for all users in the Cribl groups you've assigned.

[Snippet not found: content/shared/4.12/snippets/_sso-final-step-cloud.md]

## Troubleshooting

If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-cloud).
