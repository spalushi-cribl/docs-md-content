# Cribl.Cloud SSO Setup


The pages in this section outline how, with a Cribl.Cloud Enterprise plan, you can set up a Single Sign-On (SSO) integration between your identity provider and your Cribl.Cloud portal. The following pages cover both OIDC and SAML authentication options: 
- [Common SSO Setup Steps](sso-initial-steps)
- [OIDC/Okta Setup Example](oidc-okta-setup)
- [SAML/Okta Setup Examples](saml-okta-setup)
- [SAML/Microsoft Entra ID Setup Examples](saml-azure-setup)
- [Final SSO Steps & Troubleshooting](sso-final-steps)

SSO integration requires you to perform certain configuration steps in your identity provider (IDP), and to then submit corresponding information to Cribl. As of Cribl Stream 4.0, you can submit these details directly on your Cribl.Cloud portal's [Organization](deploy-cloud#organization) page.

![Organization page SSO tab](cloud-sso-tab.bdedafc9d7.png)
{border="true"}

This section covers both sides of the process. For additional details specifically about integrating Cribl Stream with Okta, see SSO/Okta Configuration.

The general steps to set up a Single Sign-On (SSO) integration between your identity provider and your Cribl.Cloud portal are:

1. Invite at least one SSO admin to your Cribl.Cloud Organization from a fallback, separate email domain.

2. In your identity provider (IDP), configure user groups that map to Cribl.Cloud's four predefined Roles.

3. In your IDP, create an OIDC or SAML application.
   > If creating an OIDC application, you must use backchannel authentication. Cribl.Cloud does not support front-channel authentication via OIDC.
   {.box .info}

4. Submit your app's configuration details to Cribl on your Cribl.Cloud portal's **Organization** page > **SSO** tab. (This will complete your SSO setup on the Cribl side.)

5. In your IDP, assign groups to your users, matching the Role that each group of users should have in Cribl.Cloud.

6. In your IDP, assign the OIDC/SAML app to the Organization's owner, and to other Cribl.Cloud users.
