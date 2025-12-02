# Common SSO Setup Steps


This page lists initial preparation steps that are the same for all Single Sign-On (SSO) configurations,
whether you're using OIDC or SAML.

## Set Up Fallback Access {#fallback}

In your Cribl.Cloud Organization, ensure that at least one Owner creates a local account, using an email domain that's separate from the corporate domain on which you're configuring SSO. This will ensure backup access if SSO configuration breaks.

#### Avoiding Being Forced into SSO {#workaround}

By design, SSO does Home Realm Discovery (HRD), meaning that when you enter your email to log in, SSO checks the domain specified in the email address; and, if the domain matches a connected SSO provider, SSO sends you to that provider to log in.

Normally, this is a good thing. One exception is when you want to sign up for additional Cribl.Cloud orgs without being forced through the SSO for an existing Cribl.Cloud org. In that case, try the following workaround:

1. Edit the login URL to delete the word `identifier`.
1. Use the edited URL to log in; instead of forcing you through SSO, Cribl.Cloud will ask for a username and password.

For example:

- Original URL:

    `https://login.cribl.cloud/u/login/identifier?state=<long_string_of_characters>`

- Edited URL:

    `https://login.cribl.cloud/u/login/?state=<long_string_of_characters>`

## Configure Groups {#configuring-groups}

In your IDP (identity provider), configure user groups that match Permissions for both Cribl.Cloud [Organizations](cloud-managing#member-roles) and individual [products](members#permissions-product).

Using Okta as an example, you'd map Okta groups (right side) to Cribl.Cloud Roles (left side) as follows. 

### IDP Group Naming

The names you define for groups in your IDP must include `Cribl` and the Role name (`Owner`, `Admin`, or `User`).
They can include `Organization` or product name (`Stream`, `Edge`, or `Search`).
If they don't contain a product name, the group is treated as an Organization name.

> Even though IDP group names without Organization will be treated as Organization-level permissions,
> we recommend keeping Organization in the name for clarity.
{.box .success}

You can use either the open or the closed format (with or without spaces) in group names.
You can freely add prefixes or suffixes to Group Names that follow the formats in the examples above.
Cribl will ignore these additions when mapping IDP groups to Cribl Roles. Examples:

- `SOME-LABEL-12345-CriblOrganizationOwner`
- `CriblOrganizationEditor-420`

| Cribl.Cloud Role/Permission | IDP Group Name                                               |
|------------------|--------------------------------------------------------------|
| Owner            | Cribl Organization Owner /or/ CriblOrganizationOwner         |
| Admin            | Cribl Organization Admin /or/ CriblOrganizationAdmin         |
| User            | Cribl Organization User /or/ CriblOrganizationUser         |
| Editor           | **(Deprecated, will be mapped to Admin)** Cribl Organization Editor /or/ CriblOrganizationEditor       |
| Read Only        | **(Deprecated, will be mapped to User)** Cribl Organization Read Only /or/ CriblOrganizationReadOnly  |

> ##### Considerations for Microsoft (Azure) AD
>
> For the groups claim configuration, you must select `Groups assigned to the application` for association. The source attribute must be set to `sAMAccountName`. Enable **Emit group name for cloud-only groups** to also return the `sAMAccountName` attribute for cloud-only groups.
> See the [Azure AD + OpenID Configuration](/stream/usecase-azure-ad#configure-token-and-claims) topic.
>
{.box .warning}

### Default Product Permissions {#inheritance}

When you map external users to your Cribl Organization, their initial product-level Permissions follow a different inheritance pattern than Members configured [within Cribl](members#inheritance). This is to avoid downgrading product-level Permissions that Organization-level `User`s might already have. 

The defaults for mapped users are:
- Organization `Owner` or `Admin` inherits `Admin` Permission on all products.
- Organization `User` inherits `User` Permission on all products.
- Organization `Editor` (deprecated) inherited `editor` [legacy Roles](roles#default-roles) on all products.
- Organization `Read Only` (deprecated) inherited `Read Only` Permission on Stream and Edge, and `User` Permission on Search.

### Group Configuration Better Practices

- A Cribl.Cloud Organization's Owner Role can be shared and transferred among multiple users. This facilitates gradual ownership transfers, corporate reorganizations, and other scenarios.
- Those users should be in both the Owner and Admin groups in your IDP.
(This enables them to acquire all needed permissions across Cribl's two corresponding Roles.)
- Aside from dual-assigning the Owners, you should assign every other user only one group in your IDP. (Cribl's Admin and Editor Roles include all the permissions of the Roles below them.)

Here's an example of how groups configuration (at an early stage) might look in Okta's UI:

![Mapping Okta groups to Cribl.Cloud Roles](cloud-sso-okta-groups-3.2cae240328.png)
{border="true"}