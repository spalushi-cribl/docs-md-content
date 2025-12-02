# SAML/Okta Setup Examples


This example uses Okta as the identity provider (IDP).

## Get URL and ID from Cribl {#from-cribl-saml-okta}

Cribl will provide the following information about your Cribl.Cloud Organization, to include in the SAML application that you create in your IDP:
    
1. Assertion Consumer Service URL. 

   Okta calls this a "Single sign-on URL," and this is the first of two URLs that your Cribl Organization's **SSO** > **SAML** tab lists under the same label. Example:   
   `https://login.cribl.cloud/login/callback?connection=<$organizationID>`

2. Test SSO URL.

   Required to test your connection. Okta accepts this under "Other Requestable SSO URLs," and this is the second URL that your **SSO** > **SAML** tab lists under "Single sign-on URL." Example:
   `https://<$organizationID>.cribl.cloud/api/assert`
            
3. Entity ID.

   Okta calls this an "Audience URI (SP Entity ID)," and your **SSO** > **SAML** tab calls it just an "Audience URI." Example:
   `urn:auth0:cribl-cloud-prod:<$organizationID>`

## Create SAML 2.0 App Integration {#create-app-saml-okta}

Using Okta as our example, create a SAML app integration. Start here:

![Creating an app integration](cloud-sso-okta-app-4.b16d09d3f9.png)
{border="true"}

Next, create the app integration with **Sign‑in method**: `SAML 2.0`.


![Specifying SAML 2.0 sign‑in](cloud-saml-1-create-app.2bb6f43595.png)
{border="true"}

## Configure SAML Settings {#config-saml-setting-okta}

In the app integration **SAML Settings** section, configure the following options.

- **Single sign-on URL (Assertion Consumer Service URL)**:  
`https://login.cribl.cloud/login/callback?connection=<$organizationID>`
- **Audience URI (SP Entity ID)**:  
`urn:auth0:cribl-cloud-prod:<$organizationID>`
- **Application username**: `Email`

  > The `nameidentifier` assertion in SAML responses must be the user's `Email`.
  {.box .info}

![Configuring SAML Settings](cloud-saml-3-saml-settings.53c7142453.png)
{border="true"}

Next, click the **SAML Settings** section's **Show Advanced Settings** link. Then navigate down to configure a single row of  **Other Requestable SSO URLs**, as follows:

- **URL**: From your Cribl.Cloud Organization's **SSO** > **SAML** tab, this is the second **Single sign‑on URL**. It will be in this format:
`https://<$organizationID>.cribl.cloud/api/assert`

- **Index**: Set this to `0`.

![Configuring SAML Advanced Settings](cloud-saml-4-saml-adv-sso-url.66070def33.png)
{scale="80%" border="true"}

## Configure Attribute Statements {#config-attributes-okta}

Configure **Attribute Statements** for these attributes, as shown below. Then save your app integration:

* `email`
* `given_name`
* `family_name`
* `groups`

![Configuring SAML Attribute Statements in Okta](cloud-saml-4-attributes-groups.01523b76bc.png)

## Submit Your App Info to Cribl {#tell-cribl-saml-okta}

After you've created the SAML app integration in your IDP, provide Cribl essential metadata about your application, to implement SSO setup on the Cribl side. 

On your Cribl.Cloud portal's [Organization page](deploy-cloud#organization) > **SSO** tab, select the **SAML** lower tab. 

![SSO > SAML lower tab](cloud-sso-saml-tab.c723bf5a41.png)
{border="true"}

The **Web Application Settings** will be prefilled for you, and Cribl will also prefill the **SAML Assertion Mappings** based on the information you've registered with Cribl. So you only need to fill in the **SAML Configuration** section with the following details from your IDP client configuration:
- IDP SSO
- IDP Issuer
- X.509 Certificate



Continuing with our Okta example, these screenshots show how you'd export these metadata details from your Okta app: 

![Exporting SAML metadata](cloud-saml-5a-metadata-highlight.7560571318.png)
{border="true"}

![SAML metadata in Okta's UI](cloud-saml-7-metadata-xml-file.4095ec4999.png)
{border="true"}

## SAML/Okta Chiclet Setup (Optional) {#saml-chiclet-okta}

If you want to initiate login from your Okta instance with SAML authentication configured, an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.

2. Click **Browse App Catalog.**

3. From the resulting catalog, use the search bar to find and select the `Bookmark App` application.

4. From that application's page, click **Add Integration**.

5. On the **General settings** page, enter an **Application label** that will identify this app as supporting Cribl.Cloud login. (`Cribl.Cloud` is a good choice, but the label is arbitrary.)

6. In the **URL** field, enter `https://portal.cribl.cloud/login?connection=<organizationID>` (where `<organizationID>` is your Cribl.Cloud Organization's ID).

![Creating a bookmark app in Okta](cloud-sso-bookmark-app-1.851d979935.png)
{border="true"}

7. Click **Done**.

8. Click the **Assign** button, and assign all of the Cribl.Cloud groups to the application.

![Assigning groups to the bookmark app](cloud-sso-bookmark-app-2.7fea7c20e7.png)
{border="true"}

9. The Cribl.Cloud chiclet should now be available for all users in the Cribl groups you've assigned.

![Okta Cribl.Cloud chiclet example](cloud-sso-bookmark-app-3.a49c33ecac.png)
{border="true"}

## Link Existing Users

To ensure that your Cribl.Cloud Organization's local users have a smooth transition to SSO, see [Final SSO Steps & Troubleshooting](sso-final-steps).
