# Configuring SSO groups


In your IDP (identity provider), configure user groups that match Permissions for both Cribl.Cloud [Organizations](cloud-managing#member-roles) and individual [products](members#permissions-product).

## Linking IDP Groups with Teams

You can use [Teams](teams) to automatically connect groups of Members with IDP users.

For each Team, you can specify a **Mapping ID** to use as an external ID.
In your IDP, create a group with the same ID to connect it to the Team
and in this way automatically grant IDP users access with Workspace-level permissions.

To create a Team with an external ID, refer to [Create a Team](teams#create-a-team).
To add an external ID to an existing Team:

1. Ensure you have [opted-in](workspaces#opt-in-to-workspaces) to the new Cribl.Cloud UI.
1. In the global navigation pane, expand **Organization** and then select **Teams**.
1. Find the Team you want to configure and select its action menu (...), then select **Details**.
1. In **Mapping ID**, provide an external ID for the team.
1. Confirm with **Save**.

> Users added to a Team through **Mapping IDs** will not be listed in the **Team Members** table.
> Use your IDP to view the list of Users and manage them.
{.box .warning}

## IDP Group Naming

If you are not using [Teams](teams) for IDP group mapping,
you can define groups that will be mapped to Cribl Roles based on their names.

In such a case, the names you define for groups in your IDP must include `Cribl` and the Role name (`Owner`, `Admin`, or `User`).
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

## Default Product Permissions {#inheritance}

When you map external users to your Cribl Organization, their initial product-level Permissions follow a different inheritance pattern than Members configured [within Cribl](members#inheritance). This is to avoid downgrading product-level Permissions that Organization-level `User`s might already have. 

The defaults for mapped users are:
- Organization `Owner` or `Admin` inherits `Admin` Permission on all products.
- Organization `User` inherits `User` Permission on all products (except Cribl Lake, which inherits `No Access`).
- Organization `Editor` (deprecated) inherited `editor` [legacy Roles](roles#default-roles) on all products.
- Organization `Read Only` (deprecated) inherited `Read Only` Permission on Stream and Edge, and `User` Permission on Search.

## Group Configuration Better Practices

- A Cribl.Cloud Organization's Owner Role can be shared and transferred among multiple users. This facilitates gradual ownership transfers, corporate reorganizations, and other scenarios.
- Those users should be in both the Owner and Admin groups in your IDP.
(This enables them to acquire all needed permissions across Cribl's two corresponding Roles.)
- Aside from dual-assigning the Owners, you should assign every other user only one group in your IDP. (Cribl's Admin and Editor Roles include all the permissions of the Roles below them.)

Here's an example of how groups configuration (at an early stage) might look in Okta's UI:

![Mapping Okta groups to Cribl.Cloud Roles](cloud-sso-okta-groups-3.2cae240328.png)
{border="true"}