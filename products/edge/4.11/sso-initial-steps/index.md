# Configuring SSO groups


When creating an SSO integration with a Cribl.Cloud Organization,
configure user groups in your IDP (identity provider) to manage [Teams](teams) and [Members](members).
This enables you to map IDP groups to specific Permissions on all levels.

## Link IDP Groups with Teams {#link}

You can use [Teams](teams) to automatically connect groups of [Members](members) with IDP users.
This enables you to grant IDP users [Workspace](permissions#workspace)- and [Product-level](permissions#product) Permissions.

For each Team, you can list one or more IDP group names in the **Mapping IDs** field.
This will automatically grant IDP users in those groups access to that Team's Permissions.

To create a Team with an external ID, refer to [Create a Team](teams#create-a-team).
To add an external ID to an existing Team:

1. On the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, then **Members & Teams**, and then **Teams**.
1. Select the Team you want to configure.
1. In **Mapping IDs**, provide a list of IDP group names for the Team.
1. Confirm with **Save**.

> Users added to a Team through **Mapping IDs** will not be listed in the **Team Members** table.
> Use your IDP to view the list of Users and manage them.
{.box .warning}

## IDP Group Naming

When you configure group names in your IDP, we recommend following these guidelines:

- Use unique identifiers for each group. Do not reuse or alias names. This avoids unintended Permission overlaps.

- Carefully audit and validate groups names and mappings. Make sure you provide the names verbatim in **Mappings IDs**, to prevent misconfiguration.

- Verify your group mappings in a staging environment to prevent pushing misconfigurations to production.

- Ensure you have fallback. Define default mappings for IDP users that may not match any Teams you define. This minimizes risk of orphaned users.

## Organization-Level Permissions for IDP Users

If you are not using [Teams](teams), or you want to grant [Organization-level](permissions#organization) permissions to IDP users,
you can define IDP groups with predefined names that will be mapped to Cribl Permissions automatically.

In this case, the names you define must include `Cribl` and the Permission name (`Owner`, `Admin`, or `User`).
They can include either `Organization`, or product name (`Stream`, `Edge`, or `Search`).
If they don't contain a product name, the group is treated as an Organization name.

| Cribl.Cloud Permission | IDP Group Name                                         |
|------------------|--------------------------------------------------------------|
| Owner            | Cribl Organization Owner /or/ CriblOrganizationOwner /or/ Cribl Owner        |
| Admin            | Cribl Organization Admin /or/ CriblOrganizationAdmin /or/ Cribl Admin        |
| User             | Cribl Organization User /or/ CriblOrganizationUser /or/ Cribl User           |
| Editor           | **(Deprecated, will be mapped to Admin)** Cribl Organization Editor /or/ CriblOrganizationEditor       |
| Read Only        | **(Deprecated, will be mapped to User)** Cribl Organization Read Only /or/ CriblOrganizationReadOnly  |

> Even though IDP group names without `Organization` will be treated as Organization-level permissions,
> we recommend adding `Organization` to the name for clarity.
{.box .success}

You can use either the open or the closed format (with or without spaces) in group names.
You can freely add prefixes or suffixes to group names that follow the formats in the examples above.
Cribl will ignore these additions when mapping IDP groups to Cribl Permissions. Examples:

- `My-CriblOrganizationOwner` will map to an `Owner` Organization-level Permission.
- `CriblOrganizationUser-Test` will map to a `User` Organization-level Permission.

## Default Product Permissions {#inheritance}

When you map external users to your Cribl Organization, their initial product-level Permissions follow a different inheritance pattern than Members configured [within Cribl](permissions#inheritance). This is to avoid downgrading product-level Permissions that Organization-level `User`s might already have. 

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

Here's an example of how groups configuration (at an early stage) might look in Okta:

![Mapping Okta groups to Cribl.Cloud Roles](cloud-sso-okta-groups-3.2cae240328.png)
{border="true"}

This example shows how groups configuration might look in Microsoft Entra ID:

![Mapping Microsoft Entra ID groups to Cribl.Cloud Roles](cloud-sso-microsoftentraid-groups.png)
{border="true"}