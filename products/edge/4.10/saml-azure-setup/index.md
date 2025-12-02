# SSO with Microsoft Entra ID and SAML (Cribl.Cloud)


This page presents a walkthrough of setting up a SAML SSO, using Microsoft Entra ID as the example.

## Limitations

Cribl offers an SP-initiated (Cribl-initiated) flow, but does not support an IDP-initiated SSO flow.
As an alternative, you can allow users to initiate login from your IDP instance by [creating a chiclet](#my-apps-chiclet).

[Snippet not found: content/shared/4.10/snippets/_sso-fallback-access.md]

## Create an Enterprise Application {#create-app-saml-azure-ad}

From Microsoft Entra ID's left nav: 

1. Select **Enterprise applications** and choose **All applications**, then **New application**.
1. Name your new app `Cribl.Cloud` (or any name you prefer).
1. Select **Integrate any other application you don't find in the gallery (Non‑gallery)**.
1. Confirm with **Create**.

## Assign Groups {#assign-groups-azure-ad}

Now, map your group from Cribl.Cloud to your SSO groups.
From Microsoft Entra ID's left nav: 

1. Select **Users and groups**, then select **Add user/group**.
2. Add the Cribl groups you created in [Configure Groups](sso-initial-steps).
   Do not configure Entra ID roles: access control will be handled by Cribl groups.

> If Microsoft Entra ID is synchronized with your external Active Directory,
> you must set the groups claim: select `Groups assigned to the application`
> for association and set **Source attribute** to `sAMAccountName`.
> 
> Enable **Emit group name for cloud-only groups** to return the group names
> if it's defaulting to GUID or Object ID.
>
{.box .warning}

3. Click **Assign** after selecting Groups.

## Configure Single Sign-On {#config-sso-azure-ad}

Before you start configuring SAML settings on Entra ID side, gather the required information from your Cribl.Cloud Organization.

1. In the sidebar, under **Organization**, select **SSO Management**.

2. Scroll down to the **Web Application Settings** section and select **SAML**.

3. Note down the values for **Single Sign on URL** and **Audience URI**.

   **Single Sign on URL** lists two URLs that you use for SAML configuration.

   - `https://login.cribl.cloud/login/callback?connection=<$organizationID>` is the URL you will use for the connection.
   - `https://manage.cribl.cloud/api/assert` is used during setup to test the connection.
   After you have successfully tested the connection, save the configuration and replace the second URL with the first one.

![SSO information for configuring SAML integration](cloud-sso-saml-azure-ad-config-4.7.png)
{border="true"}

4. Now, go to your Entra ID application.

5. From the left nav, select **Single sign‑on**, then **SAML**, to open the **Basic SAML Configuration** page.

6. Then, fill in the form with the information from your Cribl.Cloud Organization.
Configure the following options:

   - **Identifier (Entity ID)**: Select **Add identifier** and enter the **Audience URI** value from your Cribl.Cloud SAML settings.

   - **Reply URL (Assertion Consumer Service URL)**: Select **Add reply URL** and enter the two **Single Sign‑on URL** values from Cribl.Cloud's SAML setup page.

     Of these two URLs, identify the one with the `connection` query parameter, and select the check box to make it the **Default**.

## Configure Attributes and Groups Claims {#sso-azure-ad-claims}

In Microsoft Entra ID, edit **User Attribute & Claims** as follows. Start with the claim names:

1. Change `surname` to `family_name`.
1. Change `emailaddress` to `email`.
1. Change `givenname` to `given_name`.

Next, add a group claim:

1. Select **Groups assigned to the application**.
1. Set the **Source attribute** to `Cloud‑only group display names (Preview)`.
1. Accept the defaults for everything else, and save the new settings.

![SAML Microsoft Entra ID Attribute](cloud-sso-saml-azure-ad-attributes.c20ca5d6d2.png)
{border="true"}
         
## Submit Your App Info to Cribl {#tell-cribl-saml-azure-ad}

After you've created the SAML app integration in your IDP, provide Cribl with essential metadata about your application to implement SSO setup on the Cribl side.

1. In Cribl Edge, on the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, then **SSO Management**.
1. Above **Web Application Settings**, select **SAML**.
1. Fill in the following fields with information from Entra ID:

   | Cribl.Cloud field                      | Entra ID                                         |
   | -------------------------------------- | ------------------------------------------------ |
   | **IDP Login/Logout URL**               | Login URL                                        |
   | **IDP issuer**                         | Microsoft Entra ID Identifier                    |
   | **X.509 certificate (base64-encoded)** | Certificate (Base64) under **SAML Certificates** |

1. Select **Test Connection** to test the connection.
1. When you've verified the connection, select **Save** to complete your submission.

## SAML/Entra ID Setup with My Apps Chiclet (Optional) {#my-apps-chiclet}

If you want to log into Cribl.Cloud via the [Microsoft My Apps](https://myapps.microsoft.com/) chiclet, complete the following procedure:
1. In Microsoft Entra ID, navigate to the enterprise application that you created to integrate SSO.
1. From the left nav, select **Single Sign-on**.
1. On the Enterprise Application's **Basic SAML Configurations** page, select **Edit**.
1. In the **Sign on URL (Optional)** section, enter the following URL:

    `https://manage.cribl.cloud/login?connection=<organizationID>` (where `<organizationID>` is your Cribl.Cloud Organization’s ID).

You also need to allow [self-service access](https://learn.microsoft.com/en-us/azure/active-directory/manage-apps/manage-self-service-access) to the Cribl App,
or [assign AD groups permissions](https://learn.microsoft.com/en-us/azure/active-directory/enterprise-users/groups-self-service-management) to access the application.

[Snippet not found: content/shared/4.10/snippets/_sso-final-step-cloud.md]

## Troubleshooting

If you encounter issues when setting up SSO integration, refer to [SSO Troubleshooting](sso-troubleshooting-cloud).
