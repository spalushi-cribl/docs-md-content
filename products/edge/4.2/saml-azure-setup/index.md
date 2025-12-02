# SAML/Azure AD Setup Examples


This example uses Azure Active Directory as the identity provider (IDP).

## Get URL and ID from Cribl {#from-cribl-azure-ad}

Cribl's terminology corresponds to Azure AD's terminology as follows:

| Cribl.Cloud        | Microsoft Entra ID                                   |
|--------------------|--------------------------------------------|
| Single Sign-On URL | Reply URL (Assertion Consumer Service URL) |
| Audience URI       | Identifier (Entity ID)                     |

## Create an Enterprise Application {#create-app-saml-azure-ad}

In Microsoft Entra ID: 
1. Select **Enterprise applications** (on the left) > **New application** > **Create your own application**.
2. Name your new app `Cribl.Cloud` (or any name you prefer).
3. Select **Integrate any other application you don't find in the gallery (Non‑gallery)**.
4. Click **Create**.

## Assign Groups {#assign-groups-azure-ad}

From Microsoft Entra ID's left nav: 
1. Select **Users and groups**.
2. Select **Add user/group**.
3. Add the Cribl groups you created in [Configure Groups](sso-initial-steps#configuring-groups).
4. Click **Assign** after selecting Groups.

## Configure Single Sign-On {#config-sso-azure-ad}

From Microsoft Entra ID's left nav, select **Single sign‑on** > **SAML** to open the **Basic SAML Configuration** page. Then, as shown in the screenshot below:
1. Select **Add identifier** and enter the **Audience URI** value from Cribl.Cloud's SAML setup page.
2. Select **Add reply URL** and enter the two **Single Sign‑on URL** values from Cribl.Cloud's SAML setup page.
3. Of these two URLs, identify the one with the `connection` query parameter, and check the checkbox to make it the **Default**.

![Configuring SSO in Azure AD](cloud-sso-saml-azure-ad-config.144ceae742.png)
{border="true"}

## Configure Attributes and Groups Claims {#sso-azure-ad-claims}

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
         
## Submit Your App Info to Cribl {#tell-cribl-saml-azure-ad}

After you've created the SAML app integration in your IDP, provide Cribl essential metadata about your application, to implement SSO setup on the Cribl side. 
1. On your Cribl.Cloud portal's [Organization page](deploy-cloud#organization) > **SSO** tab, select the **SAML** lower tab. 
2. Set the **IDP Login/Logout URL** to your Azure AD's **Set up CloudSAML** section > **Login URL** value.
3. Set the **IDP issuer** to your Azure AD's **Set up CloudSAML** section > **Azure AD Identifier** value.
4. To set the **X.509 certificate (base64-encoded)**, navigate to Azure AD's **SAML Certificates** section and  download your **Base64 Certificate**.
5. Click **Test Connection**.
6. When you've verified the connection, click **Save** to complete your submission.

## Link Existing Users

To ensure that your Cribl.Cloud Organization's local users have a smooth transition to SSO, see [Final SSO Steps & Troubleshooting](sso-final-steps).
