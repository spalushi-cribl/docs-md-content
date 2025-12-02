# Cribl.Cloud SSO Setup


With a Cribl.Cloud Enterprise plan you can set up a Single Sign-On (SSO) integration
between your identity provider and your Cribl.Cloud portal.

The general steps to set up a Single Sign-On (SSO) integration between your identity provider and your Cribl.Cloud portal are:

1. In your IDP, [configure user groups](sso-initial-steps) that map to Cribl.Cloud's Teams or to predefined Roles.
1. In your IDP, create an OIDC or SAML application.
   
   > If creating an OIDC application, you must use backchannel authentication. Cribl.Cloud does not support front-channel authentication via OIDC.
   {.box .info}

3. Submit your app's configuration details to Cribl on your Cribl.Cloud portal's **Organization** page > **SSO** tab. (This will complete your SSO setup on the Cribl side.)
4. In your IDP, assign groups to your users, matching the Role that each group of users should have in Cribl.Cloud.
5. In your IDP, assign the OIDC/SAML app to the Organization's owner, and to other Cribl.Cloud users.

For detailed instruction on how to configure SSO using different IDPs, see:

- [SSO with Okta and OIDC](oidc-okta-setup)
- [SSO with Okta and SAML](saml-okta-setup)
- [SSO with Microsoft Entra ID and SAML](saml-azure-setup)

Additionally, refer to [SSO Troubleshooting](sso-final-steps) for help in resolving issues
you may encounter when setting up SSO in Cribl.Cloud.
