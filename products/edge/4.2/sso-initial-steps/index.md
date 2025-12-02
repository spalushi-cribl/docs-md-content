# Common SSO Setup Steps


Whether you're configuring Single Sign-On (SSO) via OIDC or SAML, these initial preparation steps are the same.

## Set Up Fallback Access {#fallback}

On your Cribl.Cloud Organization, ensure that at least one Owner creates a local account, using an email domain that's separate from the corporate domain on which you're configuring SSO. This will ensure backup access if SSO configuration breaks.

## Configure Groups {#configuring-groups}

In your IDP, configure user groups that match Cribl.Cloud [Role/Permissions](deploy-cloud#member-roles). Using Okta as an example, you'd map Okta groups (right side) to Cribl.Cloud Roles (left side) as follows. 

You can use either the open or the closed format in your IDP (right side). You can also add prefixes or suffixes, as outlined below in [Group Configuration Options](#fixes).

| Cribl.Cloud Role/Permission | IDP Group Name                                               |
|------------------|--------------------------------------------------------------|
| Owner            | Cribl Organization Owner /or/ CriblOrganizationOwner         |
| Admin            | Cribl Organization Admin /or/ CriblOrganizationAdmin         |
| User            | Cribl Organization User /or/ CriblOrganizationUser         |
| Editor           | **(Deprecated, will be mapped to Admin)** Cribl Organization Editor /or/ CriblOrganizationEditor       |
| Read Only        | **(Deprecated, will be mapped to User)** Cribl Organization Read Only /or/ CriblOrganizationReadOnly  |

### Product-Level Mappings {#product-level}

Starting with Cribl's 4.2 release, you can map external IDP groups to specific Roles/Permissions on your Organization's Cribl Stream, Edge, and Search products. You can map these separately per product. 

For details about the Cribl.Cloud target Permissions, see our [Members and Permissions](members#permissions-product) topic. (That page also covers on-prem counterparts to the Cribl.Cloud Organization-level Permissions listed above.) To see the corresponding names to assign to your external IDP groups, see your Cribl.Cloud Organization's **Account** > **Organization** > **SSO** tab.

### Default Product Permissions {#inheritance}

When you map external users to your Cribl Organization, their initial product-level Permissions follow a different inheritance pattern than Members configured [within Cribl](members#inheritance). This is to avoid downgrading product-level Permissions that Organization-level `User`s might already have. 

The defaults for mapped users are:
- Organization `Owner` or `Admin` inherits `Admin` Permission on all products.
- Organization `User` inherits `User` Permission on all products.
- Organization `Editor` (deprecated) inherited `editor` [legacy Roles](roles#default-roles) on all products.
- Organization `Read Only` (deprecated) inherited `Read Only` Permission on Stream and Edge, and `User` Permission on Search.
## Group Configuration Best Practices

* A Cribl.Cloud Organization's Owner Role can be shared and transferred among multiple users. This facilitates gradual ownership transfers, corporate reorganizations, and other scenarios.
* Those users should be in both the Owner and Admin groups in your IDP.   
(This enables them to acquire all needed permissions across Cribl's two corresponding Roles.)
* Aside from dual-assigning the Owners, you should assign every other user only one group in your IDP. (Cribl's Admin and Editor Roles include all the permissions of the Roles below them.)

## Group Configuration Options {#fixes}

You can freely add prefixes or suffixes to Group Names that follow the above examples' formats. Cribl will ignore these additions when mapping IDP groups to Cribl Roles. Examples:
- `SOME-LABEL-12345-CriblOrganizationOwner`
- `CriblOrganizationEditor-420`

Here's an example of how groups configuration (at an early stage) might look in Okta's UI:

![Mapping Okta groups to Cribl.Cloud Roles](cloud-sso-okta-groups-3.af6acd976f.png)
{border="true"}