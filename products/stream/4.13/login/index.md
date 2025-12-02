# Log in to Cribl


When you first log in to a Cribl instance, you will be required to configure your account and authenticate.

Depending on how your Organization is configured, you may need to either authenticate via your SSO provider
(such as Okta, Entra ID, or Ping Identity), or create a password for use with your email address.

When creating a new username and password, you will be prompted for account information,
such as name, company, or phone number.

## Switch Between Organizations

When you are a member of multiple Organizations, you can switch between them on the top bar.
Select your current Organization's name on the bar to open a drop-down with all Organizations you have access to.

![Cribl top bar with Organization drop-down opened, showing three Organizations.](org-switcher-4.12.2.png)
{border="true" caption="Switching between Organizations"}

### Organizations with Different Authentication Methods

If you switch between Organizations that use different authentication methods (SSO versus username/password),
you will be asked to re-authenticate with the appropriate credentials.

If the main Organization uses SSO, and you are switching to an Organization that uses username/password authentication,
you will need to authenticate with a password.
If you are accepting an invitation to the latter Organization,
you will be prompted to create a password and go through a verification process.

If the main Organization uses username/password authentication, and you are switching to an SSO-based Organization,
you will need to authenticate via SSO.
