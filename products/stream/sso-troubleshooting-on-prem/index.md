# SSO Troubleshooting (On-Prem)


Issues with enabling SSO typically result from incorrect or missing configuration,
and can occur when:

- Logging in via SSO,
- Testing an IdP connection.

## OIDC Troubleshooting

Check the following table for errors you may encounter when using OIDC-based SSO in an on-prem deployment.

| Error | Possible solutions |
|---|---|
| "400 Bad Request. Your request resulted in an error. The 'redirect_uri' parameter must be a Login redirect URI in the client app settings" | **Audience (Relying Party ID)**: verify it does not include trailing slash. <br> **Sign-in redirect URIs**: verify they are correct. |
| "400 Bad Request. Your request resulted in an error." | **Client ID** and **Response validation certificate**: verify they are correct. |
| "404 Page Not Found" | **Authentication URL** and **Single sign-on (SSO) URL**: verify they are correct.  |
| "Unauthorized" | **Scope** field: verify it is: `openid profile email groups` <br> Make sure that the user is part of an Okta group. |
| "You are not assigned Permissions for any Groups." <br> "You do not have sufficient permissions to access this resource." | **Group name field**: verify it is `groups` (plural). <br> **Scope** field: verify it is: `openid profile email groups`. <br> Make sure that external group names match in **Role Mapping**. |
| After logging in through the IdP button, Cribl redirects back to main login page. | **Client Secret**, **Token URL**, and **UserInfo URL**: verify they are correct. |

## SAML Troubleshooting

Check the following table for errors you may encounter when using SAML-based SSO in an on-prem deployment.

| Error | Possible solutions |
|---|---|
| "AADSTS50011: The reply URL '{URL}' specified in the request does not match the reply URLs configured for the application..." | **Audience (SP entity ID)**: verify it does not include a trailing slash. |
| "AADSTS700016: Application with identifier '{app-id}' was not found in the directory 'Default Directory'" | **Identifier (Entity ID)**: verify it is correct. |
| "AADSTS900023: Specified tenant identifier '{id}' is neither a valid DNS name, nor a valid external domain." | **Single sign-on (SSO) URL**: verify it is correct. |
| "For permissions on Groups, contact your Stream Admin." | **Group name**: verify it is correct. <br> Make sure that Role Mapping matches **IdP Group Name** or GUID based on groups claim. |
| "This login.microsoftonline.com page can’t be found" | **IDP Login/Logout URL**: verify it is correct. |
