# SSO Troubleshooting in Cribl.Cloud


Issues with enabling SSO typically result from incorrect or missing configuration,
and can occur when:

- Logging in via SSO,
- Testing an IDP connection.

## OIDC Troubleshooting

Check the following table for errors you may encounter when using OIDC-based SSO in Cribl.Cloud.

| Error | Possible solutions |
|---|---|
| "400 Bad Request. The 'redirect_uri' parameter must be a Login redirect URI in the client app settings" | **Sign-in Redirect URI**: verify it is correct. |
| "400 Bad Request. Your request resulted in an error." | **Client ID**: verify it is correct.  |
| "invalid_request: IdP-Initiated login is not enabled for connection" after initiating login from Cribl App chiclet | Configure an [Okta/OIDC](oidc-okta-setup#oidc-chiclet) chiclet. |
| "You are not assigned Permissions for any Groups." | **Groups Claim Filter**: verify that the claim name is set to `groups` (plural). <br> Make sure that the user is attached to a group. <br> Make sure that the group name is spelled correctly. |
| After logging in through the IDP button, Cribl.Cloud redirects back to main login page. | Make sure that the user or group is assigned to the application.|

## SAML Troubleshooting

Check the following table for errors you may encounter when using SAML-based SSO in Cribl.Cloud.

| Error | Possible solutions |
|---|---|
| "400 Bad Request. Your request resulted in an error." | **Other Requestable SSO URLs**: verify they are correct and contain no trailing slash. |
| "404 Not found" | **Other Requestable SSO URLs**: verify it is set to the **Single sign-on URL**. <br> **Reply URL (Assertion Consumer Service URL)** and **IDP Login/Logout URL**: verify they are correct. |
| "Cannot read properties of undefined (reading 'startWith')" | Make sure that the `email` attribute statement is correctly set to `user.email`. |
| "ERR_UNMATCH_ISSUER" | **IDP Issuer**: verify it is correct. |
| "ERROR_UNMATCH_CERTIFICATE_DECLARATION_IN_METADATA" | **X.509 Certificate**: verify it is correct. |
| "invalid_request: IdP-Initiated login is not enabled for connection" after initiating login from Cribl App chiclet | Configure an [Okta/SAML](saml-okta-setup#saml-chiclet-okta) chiclet. |
| "invalid_request: The connection {Organization ID} was not found." | **Cribl SSO URL**: verify it is correct. <br> Check whether SSO configuration was saved and is enabled. |
| "invalid_request: You may have pressed the back button..." after initiating login from Cribl App chiclet | Make sure that the URL in chiclet configuration is correct. |
| "Single Sign-On Error - We can't log you in because of an issue with single sign-on. " | Close all pages, sign out from IDP and log in again. |
| "Sorry, but we’re having trouble signing you in." | **Identifier (Entity ID)**: verify it is correct. <br> Check whether the Group is  assigned to application. |
| "This login.microsoftonline.com page can’t be found" | **IDP Login/Logout URL**: verify it is correct. |
| "There could be a misconfiguration in the system or a service outage."| **Sign on URL (Optional)**: verify it is correct. |
| "This page isn’t working. If the problem continues, contact the site owner" | **Reply URL (Assertion Consumer Service URL)** and **Sign on URL (Optional)**: verify they are correct. |
| After logging in and authenticating, the page is stuck in a loading loop with a spinner. | Make sure that the email claim is not missing and that the user has an email address set in their contact information. <br> Make sure that the `email` attribute statement is correctly set to `user.email`. <br>|
| After logging in through the IDP button, Cribl.Cloud redirects back to main login page. | **Audience URI (SP Entity ID)**, **Single Sign-On URL**, and chiclet URL: verify they are correct. <br> Make sure that SAML configuration is enabled. |

## Troubleshooting Resources

[Cribl University](https://cribl.io/university/) offers an [SSO Integration – Cribl.Cloud – Okta](https://university.cribl.io/#/online-courses/3245a783-4efd-467e-bb0e-29b9958c4072) Troubleshooting Criblet. To follow the direct course link, first log into your Cribl University account. 

To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses. Cribl’s training is always free of charge.

Once logged in, check out other useful [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0) and [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses.
