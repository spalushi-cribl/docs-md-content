# Cribl.Cloud SSO Setup


This document outlines how, with a Cribl.Cloud Enterprise plan, you can set up a Single Sign-On (SSO) integration between your identity provider and your Cribl.Cloud portal. It covers both OIDC and SAML authentication options, in the following sections:
- [SSO Setup Overview](#ovw)
- [Common Initial Steps](#initial)
- [OIDC/Okta Setup Example](#oidc)
- [SAML Setup Examples](#saml)
- [Common Final Steps](#final)

SSO integration requires you to perform certain configuration steps in your identity provider (IDP), and to then submit corresponding information to Cribl. As of Cribl Stream 4.0, you can submit these details directly on your Cribl.Cloud portal's [Organization page](deploy-cloud#organization). 

![Organization page's SSO tab](cloud-sso-tab.bdedafc9d7.png)
{border="true"}

This document covers both sides of the process. For additional details specifically about integrating Cribl Stream with Okta, see [SSO/Okta Configuration](/stream/usecase-sso-okta).

## SSO Setup Overview {#ovw}

The general steps here are:

1. Invite at least one SSO admin to your Cribl.Cloud Organization from a fallback, separate email domain.

2. In your identity provider (IDP), configure user groups that map to Cribl.Cloud's four predefined Roles.

3. In your IDP, create an OIDC or SAML application.
   > If creating an OIDC application, you must use backchannel authentication. Cribl.Cloud does not support front-channel authentication via OIDC.
   {.box .info}

4. Submit your app's configuration details to Cribl on your Cribl.Cloud portal's **Organization** page > **SSO** tab. (This will complete your SSO setup on the Cribl side.)

5. In your IDP, assign groups to your users, matching the Role that each group of users should have in Cribl.Cloud.

6. In your IDP, assign the OIDC/SAML app to the Organization's owner, and to other Cribl.Cloud users.

## Common Initial Steps {#initial}

Whether you're configuring SSO via OIDC or SAML, these initial preparation steps are the same.

### Set Up Fallback Access {#fallback}

On your Cribl.Cloud Organization, ensure that at least one Owner creates a local account, using an email domain that's separate from the corporate domain on which you're configuring SSO. This will ensure backup access if SSO configuration breaks.

### Configure Groups {#configuring-groups}

In your IDP, configure user groups that match Cribl.Cloud [Roles](deploy-cloud#member-roles). Using Okta as an example, you'd map Okta groups (right side) to Cribl.Cloud Roles (left side) as follows. 

You can use either the open or the closed format in your IDP (right side). You can also add prefixes or suffixes, as outlined below in [Group Configuration Options](#fixes).

| Cribl.Cloud Role | IDP Group Name                                               |
|------------------|--------------------------------------------------------------|
| Owner            | Cribl Organization Owner /or/ CriblOrganizationOwner         |
| Admin            | Cribl Organization Admin /or/ CriblOrganizationAdmin         |
| Editor           | Cribl Organization Editor /or/ CriblOrganizationEditor       |
| Read-Only        | Cribl Organization Read Only /or/ Cribl OrganizationReadOnly |

#### Group Configuration Best Practices

* Only one user can be in the Cribl.Cloud Owner Role.
* That user should be in both the Owner and Admin groups in your IDP.   
(This enables them to acquire all needed permissions across Cribl's two corresponding Roles.)
* Aside from dual-assigning the Owner, you should assign every other user only one group in your IDP. (Cribl's Admin and Editor Roles include all the permissions of the Roles below them.)

#### Group Configuration Options {#fixes}

You can freely add prefixes or suffixes to Group Names that follow the above examples' formats. Cribl will ignore these additions when mapping IDP groups to Cribl Roles. Examples:
- `SOME-LABEL-12345-CriblOrganizationOwner`
- `CriblOrganizationEditor-420`

Here's an example of how groups configuration (at an early stage) might look in Okta's UI:

![Mapping Okta groups to Cribl.Cloud Roles](cloud-sso-okta-groups-3.af6acd976f.png)
{border="true"}

## OIDC/Okta Setup Example {#oidc}

This section expands on the above overview for OIDC, offering a detailed walkthrough with Okta as the example IDP. 

> As mentioned above, Cribl.Cloud supports only OIDC backchannel authentication, not front-channel.
>
{.box .info}

### Create OIDC App Integration {#create-app-oidc}

To create your app integration within Okta, start here:

![Creating an app integration](cloud-sso-okta-app-4.b16d09d3f9.png)
{border="true"}

Next, configure the app integration with the options below.
- **Sign-in method**: `OIDC - OpenID Connect`
- **Application type**: `Web Application`

![Configuring new Okta app integration](cloud-sso-okta-integration-5.0efdd7533b.png)
{border="true"}

### General Settings

Configure the app integration's **General Settings** with the options below.
- **App Integration Name**: `Cribl.Cloud`
- **Sign-In Redirect URIs**: [https://login.cribl.cloud/login/callback](https://login.cribl.cloud/login/callback)
- **Sign-Out Redirect URI**s: [https://login.cribl.cloud/v2/logout](https://login.cribl.cloud/v2/logout)
- **Grant Type**: `Authorization Code`, `Refresh Token`
- **Scopes**: `openid profile email groups`

![App integration name and URIs](cloud-sso-okta-uris-6a.bb28070e4c.png)
{border="true"}

> If your IDP is PingOne, you must also configure this (non-Okta) option:
> - **Authentication Options**: `Allow Client Secret`
>
{.box .info}

### Assignments

Configure the **Assignments** pane with the options below.
- **Controlled access**: `Limited access to selected groups`
- **Selected groups**: <The groups you mapped earlier in [Configuring Groups](#configuring-groups)>

![Assignments configuration](cloud-sso-okta-assignments-7.a677340385.png)
{border="true"}

### Sign-On Tab

Edit the **OpenID Connect ID Token** section, as shown here:

![Sign-On tab’s Settings/Tokens sections](cloud-sso-okta-sign-on-9.25e82a0d3d.png)
{border="true"}

Set the **Groups claim filter** to: `groups` : `Starts with` : `Cribl`, as shown below:

![OIDC token: configuring Groups claim filter](cloud-sso-okta-groups-claim-10.2abb6a4112.png)
{border="true"}

Change the **Issuer** from `Dynamic` to `Okta URL`, to get the **Issuer URL** to provide to Cribl in the next section:

![OIDC token: obtaining Issuer URL](cloud-sso-okta-issuer-url.5350098b87.png)
{scale="70%"}

### Submit Your App Info to Cribl {#tell-cribl}

This concludes the setup you need to do in Okta (or other IDP). Next, provide Cribl essential details about your application, to implement SSO setup on the Cribl side. 

On your Cribl.Cloud portal's [Organization page](deploy-cloud#organization) > **SSO** tab, select the **OIDC** lower tab. 

![SSO > OIDC lower tab](cloud-sso-oidc-tab.13db2badc1.png)
{border="true"}

The **Web Application Settings** are prefilled for you, so you only need to fill in the **Cribl Cloud SSO Settings** section with the following details from your IDP client configuration:
   - Client ID
   - Client Secret
   - Issuer URL.
     Make sure **Issuer URL** does not contain a trailing space.

Continuing with our Okta example, here's where you'd copy the Issuer URL from your Okta app:

![Obtaining Issuer URL from Okta](cloud-sso-okta-metadata-11.70345f8f9d.png)
{border="true"}

### OIDC/Okta Chiclet Setup (Optional) {#oidc-chiclet}

If you want to initiate login from your Okta instance with OIDC authentication configured, an Okta admin can configure an app integration as follows:

1. From Okta's left nav, select the **Applications** page.

2. Find the OIDC application created earlier in the [OIDC/Okta Setup Example](#oidc).

3. Click that application, and select **General Settings** > **Edit**.

4. In the **Initiate login URI** field, enter `https://portal.cribl.cloud/login?connection=<organizationID>` (where `<organizationID>` is your Cribl.Cloud Organization's ID).

![Adding the Initiate Login URI value](cloud-sso-oidc-app.d5e5aca928.png)
{border="true"}

5. Click **Save** to complete the chiclet.

## SAML Setup Examples {#saml}

This section outlines integrating SAML SSO with Cribl.Cloud. It includes examples of both [Okta](#saml-okta) and [Azure Active Directory](#saml-azure-ad) integrations.

### Okta/SAML {#saml-okta}

This example uses Okta as the identity provider (IDP).

#### Get URL and ID from Cribl {#from-cribl-saml-okta}

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

#### Create SAML 2.0 App Integration {#create-app-saml-okta}

Using Okta as our example, create a SAML app integration. Start here:

![Creating an app integration](cloud-sso-okta-app-4.b16d09d3f9.png)
{border="true"}

Next, create the app integration with **Sign‑in method**: `SAML 2.0`.


![Specifying SAML 2.0 sign‑in](cloud-saml-1-create-app.2bb6f43595.png)
{border="true"}

#### Configure SAML Settings {#config-saml-setting-okta}

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

Next, click the **SAML Settings** section's '**Show Advanced Settings** link. Then navigate down to configure a single row of  **Other Requestable SSO URLs**, as follows:

- **URL**: From your Cribl.Cloud Organization's **SSO** > **SAML** tab, this is the second **Single sign‑on URL**. It will be in this format:
`https://<$organizationID>.cribl.cloud/api/assert`

- **Index**: Set this to `0`.

![Configuring SAML Advanced Settings](cloud-saml-4-saml-adv-sso-url.66070def33.png)
{scale="80%" border="true"}

#### Configure Attribute Statements {#config-attributes-okta}

Configure **Attribute Statements** for these attributes, as shown below. Then save your app integration:

* `email`
* `given_name`
* `family_name`
* `groups`

![Configuring SAML Attribute Statements in Okta](cloud-saml-4-attributes-groups.01523b76bc.png)

#### Submit Your App Info to Cribl {#tell-cribl-saml-okta}

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

#### SAML/Okta Chiclet Setup (Optional) {#saml-chiclet-okta}

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

### Azure AD/SAML {#saml-azure-ad}

This example uses Azure Active Directory as the identity provider (IDP).

#### Get URL and ID from Cribl {#from-cribl-azure-ad}

Cribl's terminology corresponds to Azure AD's terminology as follows:

| Cribl.Cloud        | Microsoft Entra ID                                   |
|--------------------|--------------------------------------------|
| Single Sign-On URL | Reply URL (Assertion Consumer Service URL) |
| Audience URI       | Identifier (Entity ID)                     |

#### Create an Enterprise Application {#create-app-saml-azure-ad}

In Microsoft Entra ID: 
1. Select **Enterprise applications** (on the left) > **New application** > **Create your own application**.
2. Name your new app `Cribl.Cloud` (or any name you prefer).
3. Select **Integrate any other application you don't find in the gallery (Non‑gallery)**.
4. Click **Create**.

#### Assign Groups {#assign-groups-azure-ad}

From Microsoft Entra ID's left nav: 
1. Select **Users and groups**.
2. Select **Add user/group**.
3. Add the Cribl groups you created earlier in [Configure Groups](#configuring-groups).
4. Click **Assign** after selecting Groups.

#### Configure Single Sign-On {#config-sso-azure-ad}

From Microsoft Entra ID's left nav: 
1. Select **Single sign‑on** > **SAML**.
2. Edit the **Basic SAML Configuration** as follows.
3. Select **Add identifier**, and enter the **Audience URI** value from Cribl.Cloud's SAML setup page.
4. Select **Add reply URL**, and enter the **Single Sign‑on URL** value from Cribl.Cloud's SAML setup page.
5. Test the connection with both of the above values in place.
6. Save the new settings.

![Configuring SSO in Azure AD](cloud-sso-saml-azure-ad-config.144ceae742.png)
{border="true"}

#### Configure Attributes and Groups Claims {#sso-azure-ad-claims}

In Microsoft Entra ID, edit **Attribute & Claims** as follows. Start with the claim names:
1. Change `surname` to `family_name`.
2. Change `emailaddress` to `email`.
3. Change `givenname` to `given_name`.

Next, add a group claim:
1. Select **Groups assigned to the application**.
2. As the **Source Attribute**, select: `Cloud‑only group display names (Preview)`.
3. Accept the defaults for everything else, and save the new settings.

![SAML Microsoft Entra ID Attribute](cloud-sso-saml-azure-ad-attributes.c20ca5d6d2.png)
{border="true"}
         
#### Submit Your App Info to Cribl {#tell-cribl-saml-azure-ad}

After you've created the SAML app integration in your IDP, provide Cribl essential metadata about your application, to implement SSO setup on the Cribl side. 
1. On your Cribl.Cloud portal's [Organization page](deploy-cloud#organization) > **SSO** tab, select the **SAML** lower tab. 
2. Set the **IDP Login/Logout URL** to your Azure AD's **Set up CloudSAML** section > **Login URL** value.
3. Set the **IDP issuer** to your Azure AD's **Set up CloudSAML** section > **Azure AD Identifier** value.
4. To set the **X.509 certificate (base64-encoded)**, navigate to Azure AD's **SAML Certificates** section and  download your **Base64 Certificate**.
5. Click **Test Connection**.
6. When you've verified the connection, click **Save** to complete your submission.

## Common Final Steps {#final}

Whether you're integrating with OIDC or SAML, there's one more step for users who had an existing username/password-based login on Cribl.Cloud before SSO was set up: 

Upon first login with SSO, these users will see a prompt to link their identities. They should accept this prompt, to ensure that their existing profile is linked with their SSO profile. (This can be a multi-step flow.)

![Prompt to link accounts](cloud-sso-link-accounts.909117420b.png)
{scale="90%"}


