# SAML/Okta Setup Examples


This example uses Okta as the identity provider (IDP).

## Get URL and ID from Cribl {#from-cribl-saml-okta}

Cribl will provide the following information about your Cribl.Cloud Organization, to include in the SAML application that you create in your IDP:
    
1. Assertion Consumer Service URL. 

   Okta calls this a "Single sign-on URL," and this is the first of two URLs that your Cribl Organization's **SSO** > **SAML** tab lists under the same label. Example:   
   `https://login.cribl.cloud/login/callback?connection=<$organizationID>`

2. Test SSO URL.

   Required to test your connection. Okta accepts this under "Other Requestable SSO URLs," and this is the second URL that your **SSO** > **SAML** tab lists under "Single sign-on URL." Example:
   `https://manage.cribl.cloud/api/assert`
            
3. Entity ID.

   Okta calls this an "Audience URI (SP Entity ID)," and your **SSO** > **SAML** tab calls it just an "Audience URI." Example:
   `urn:auth0:cribl-cloud-prod:<$organizationID>`

## Create SAML 2.0 App Integration {#create-app-saml-okta}

1. To create your app integration within Okta, navigate to the **Applications** section of your Okta environment and click **Create App Integration**. 

2. Next, create the app integration with **Sign‑in method**: `SAML 2.0`.

## Configure SAML Settings {#config-saml-setting-okta}

1. In the app integration **SAML Settings** section, configure the following options.

- **Single sign-on URL (Assertion Consumer Service URL)**:  
`https://login.cribl.cloud/login/callback?connection=<$organizationID>`
- **Audience URI (SP Entity ID)**:  
`urn:auth0:cribl-cloud-prod:<$organizationID>`
- **Application username**: `Email`

  > The `nameidentifier` assertion in SAML responses must be the user's `Email`.
  {.box .info}

2. Next, click the **SAML Settings** section's **Show Advanced Settings** link. Then navigate down to configure a single row of  **Other Requestable SSO URLs**, as follows:

- **URL**: From your Cribl.Cloud Organization's **SSO** > **SAML** tab, this is the second **Single sign‑on URL**. It will be in this format:
`https://manage.cribl.cloud/api/assert`
- **Index**: Set this to `0`.

## Configure Attribute Statements {#config-attributes-okta}

Configure **Attribute Statements** for these attributes, as shown below:

* `email`
* `given_name`
* `family_name`
* `groups`

Then save your app integration.

## Submit Your App Info to Cribl {#tell-cribl-saml-okta}

After you've created the SAML app integration in your IDP, provide Cribl with the essential metadata about your application to implement SSO setup on the Cribl side. 

1. On your Cribl.Cloud portal's [Organization page](cloud-portal#organization) > **SSO** tab, select the **SAML** lower tab. 

The **Web Application Settings** will be prefilled for you, and Cribl will also prefill the **SAML Assertion Mappings** based on the information you've registered with Cribl. So you only need to fill in the **SAML Configuration** section with the following details from your IDP client configuration:
- IDP SSO
- IDP Issuer
- X.509 Certificate

2. Return to your Okta environment and click **View SAML setup instructions**. 

## SAML/Okta Chiclet Setup (Optional) {#saml-chiclet-okta}

If you want to initiate login from your Okta instance with SAML authentication configured, an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.

2. Click **Browse App Catalog.**

3. From the resulting catalog, use the search bar to find and select the `Bookmark App` application.

4. From that application's page, click **Add Integration**.

5. On the **General settings** page, enter an **Application label** that will identify this app as supporting Cribl.Cloud login. (`Cribl.Cloud` is a good choice, but the label is arbitrary.)

6. In the **URL** field, enter `https://manage.cribl.cloud/login?connection=<organizationID>` (where `<organizationID>` is your Cribl.Cloud Organization's ID).

7. Click **Done**.

8. Click **Assign** and assign all of the Cribl.Cloud groups to the application.

9. The Cribl.Cloud chiclet should now be available for all users in the Cribl groups you've assigned.

## Link Existing Users

To ensure that your Cribl.Cloud Organization's local users have a smooth transition to SSO, see [Final SSO Steps & Troubleshooting](sso-final-steps).
