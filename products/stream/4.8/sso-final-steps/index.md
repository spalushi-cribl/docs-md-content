# SSO Troubleshooting in Cribl.Cloud


Issues with enabling SSO typically result from incorrect or missing configuration.
If your users are encountering problems logging in, make sure you have configured all the necessary fields (watch out for typos!):

- For SAML-based SSO, ensure that you have filled in the following fields correctly:
    - **IDP Login/Logout URL**
    - **IDP issuer**
    - **X.509 certificate (base64-encoded)**
- For OIDC-based SSO, verify the correctness of the following fields: 
    - **Client ID**
    - **Client secret**
    - **Issuer URL**. Make sure **Issuer URL** does not contain a trailing space.

Ensure all required users and groups are assigned to the application.

Make sure that the claims in your IDP are set up correctly. See our docs on [Entra ID SAML](saml-azure-setup#sso-azure-ad-claims) and [Okta OIDC](oidc-okta-setup#sign-on-tab).

If IDP-initiated logins are malfunctioning, you may see an error message: "IdP-Initiated login is not enabled".
In this case, refer to the chiclet setup docs for [Okta/OIDC](oidc-okta-setup#oidc-chiclet),
[Okta/SAML](saml-okta-setup#saml-chiclet-okta), or [Microsoft Entra ID/SAML](saml-azure-setup#my-apps-chiclet).

> ##### Troubleshooting Resources
>
> [Cribl University](https://cribl.io/university/) offers an [SSO Integration – Cribl.Cloud – Okta](https://university.cribl.io/#/online-courses/3245a783-4efd-467e-bb0e-29b9958c4072) Troubleshooting Criblet. To follow the direct course link, first log into your Cribl University account. 
> 
> To create an account, select the **Sign up** link. You’ll need to click through a short **Terms & Conditions** presentation, with chill music, before proceeding to courses. Cribl’s training is always free of charge.
> 
> Once logged in, check out other useful [Troubleshooting Criblets](https://university.cribl.io/#/curricula/2574ac50-05a3-4b50-8990-fcfa58a2aec0) and [Advanced Troubleshooting](https://university.cribl.io/#/curricula/4d42b5c4-5157-4c49-8322-89d16fad5b73) short courses.
>
{.box .info}

