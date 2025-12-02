# Manage Access to Multiple Organizations


As an administrator, you can maintain multiple Cribl.Cloud Organizations and their Members.
Organizations can have one of two authentication methods: either SSO, or username/password.

> Because each Organization is associated with a unique domain,
> you can't use the same email to access multiple Organizations via SSO.
{.box .info}

## Multiple Organizations - Example

To illustrate the experience of accessing multiple Organizations, let's look at the following example.
Assume two Organizations:

- **SSOOrg**, which uses SSO
- **UPOrg**, which uses username/password

The primary Organization, SSOOrg, requires the user to authenticate via SSO.
When you invite that user to UPOrg, they are prompted to set up their username and password pair.
On first login to UPOrg, the user will need to configure their account
and provide some additional information such as company name.

When you add one more Organization, **NewOrg**, also using username/password,
and invite your user to it, they will authenticate with the same username/password combination as for UPOrg.
