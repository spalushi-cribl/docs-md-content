# Configuring SSO Groups


When creating an SSO integration with a Cribl.Cloud Organization, configure user groups in your identity provider (IdP) to manage [Teams](teams) and [Members](members). This enables you to map IdP groups to specific Permissions on all levels.

## Link IdP Groups with Teams {#idp-groups-teams}

You can use [Teams](teams) to automatically connect groups of [Members](members) with IdP users. This enables you to grant IdP users [Workspace](permissions#workspace)- and [Product](permissions#product)-level Permissions.

> It is **not** possible to use Teams to grant [Organization](permissions#organization)-level Permissions. Instead, grant Organization-level Permissions by managing group membership within your IdP. Make sure that your IdP group names align with the core patterns for [automatic Permission mapping](#idp-group-name-mapping).
{.box .info}

For each Team, you can list one or more IdP group names in the **Mapping IDs** field. This will automatically grant IdP users in those groups access to that Team's Permissions.

To create a Team with an external ID, refer to [Create a Team](teams#create-a-team). To add an external ID to an existing Team:

1. On the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, then **Members & Teams**, and then **Teams**.
1. Select the Team you want to configure.
1. In **Mapping IDs**, provide a list of IdP group names for the Team.
1. Confirm with **Save**.

> Users added to a Team through **Mapping IDs** will not be listed in the **Team Members** table.
> Use your IdP to view the list of Users and manage them.
{.box .warning}

## IdP Group Names and Automatic Permission Mapping {#idp-group-name-mapping}

If you are not using [Teams](teams), IdP group names must include one of the following core patterns:

- `Cribl` + a Role name
- `Cribl` + `Organization` or a product name + a Role name

The core pattern must be a contiguous string, with or without spaces. Cribl's [Permission mapping logic](#permission-mapping) ignores prefixes and suffixes before or after the core pattern in IdP group names.

For example, these are **valid** IdP group names for Cribl's mapping logic:

- `CriblOwner`
- `Cribl Owner`
- `CriblOrganizationUser`
- `Cribl Search Admin`
- `dev-CriblOwner`
- `Cribl Stream User_Test`

These are examples of **invalid** IdP group names for Cribl's mapping logic:

- ![Invalid identity provider group name](slash-circle-01.svg) `Cribl-Owner`
- ![Invalid identity provider group name](slash-circle-01.svg) `Cribl - Admin`
- ![Invalid identity provider group name](slash-circle-01.svg) `Cribl_Organization_User`
- ![Invalid identity provider group name](slash-circle-01.svg) `dev-Cribl-Owner`
- ![Invalid identity provider group name](slash-circle-01.svg) `Cribl Lake_Editor_Test`

### Permission Mapping Logic {#permission-mapping}

If you are not using [Teams](teams), Cribl's Permission mapping logic uses the [core patterns](#idp-group-name-mapping) to automatically map IdP groups to Cribl Permissions as follows:

- IdP group names that include `Organization` map to [Organization-level Permissions](permissions#organization).
- IdP group names that do not include `Organization` or a product name map to [Organization-level Permissions](permissions#organization).
- IdP group names that include a product name map to the corresponding [Product-level Permissions](permissions#product).

> Even though Cribl maps IdP group names that do not include `Organization` or a product name to Organization-level permissions, we recommend adding `Organization` to such names for clarity.
{.box .success}

The following table demonstrates which Permissions Cribl maps to example IdP group names. 

| Cribl.Cloud Permission | Example IdP Group Names |
| ---------------------- | ----------------------- |
| Owner     | `Cribl Organization Owner` or `CriblOrganizationOwner` or `Cribl Owner` |
| Admin     | `Cribl Organization Admin` or `CriblOrganizationAdmin` or `Cribl Admin` |
| User      | `Cribl Organization User` or `CriblOrganizationUser` or `Cribl User` |
| Editor    | **(Deprecated; mapped to Admin)** `Cribl Organization Editor` or `CriblOrganizationEditor` |
| Read Only | **(Deprecated; mapped to User)** `Cribl Organization Read Only` or `CriblOrganizationReadOnly` |

For a more complete list of IdP group names that map to specific Organization- and Product-level Permissions, navigate to **Organization > SSO Management** in Cribl.Cloud.

## Default Product Permissions and Inheritance {#inheritance}

When you map external users to your Cribl Organization, their initial product-level Permissions follow a different inheritance pattern than Members configured [within Cribl](permissions#inheritance). This is to avoid downgrading product-level Permissions that Organization-level `User`s might already have. 

The defaults for mapped users are:
- Organization `Owner` or `Admin` inherits `Admin` Permission on all products.
- Organization `User` inherits `User` Permission on all products (except Cribl Lake, which inherits `No Access`).
- Organization `Editor` (deprecated) inherited `editor` [legacy Roles](roles#default-roles) on all products.
- Organization `Read Only` (deprecated) inherited `Read Only` Permission on Cribl Stream and Cribl Edge, and `User` Permission on Cribl Search.

## Better Practices for Group Naming and Configuration

Follow these guidelines for IdP group naming and configuration:

- Use unique names for each group to prevent unintended Permission overlaps. Do not reuse or alias group names.
- Carefully audit and validate group names and mappings. To prevent misconfiguration, make sure to provide the group names verbatim in **Mappings IDs**.
- Verify your group mappings in a staging environment to prevent pushing misconfigurations to production.
- [Set up fallback access](sso-cloud#set-up-fallback-access) before you configure SSO. Define default mappings for IdP users who may not match any Teams you define. This minimizes the risk of orphaned users.
- A Cribl.Cloud Organization's Owner Role can be shared and transferred among multiple users. This facilitates gradual ownership transfers, corporate reorganizations, and other scenarios. These multiple users should be in both the Owner and Admin groups in your IdP so that they can acquire all needed permissions across Cribl's two corresponding Roles.
- Aside from dual-assigning the Owners, you should assign every other user only one group in your IdP. Cribl's Admin and Editor Roles include all the permissions of the Roles below them.
- If only one user is assigned to an Organization's Owner Role, IdP group updates will not remove the user from the Owner Role. This prevents Owner lockout in case of SSO configuration issues.

Here's an example of how groups configuration (at an early stage) might look in Okta:

![Mapping Okta groups to Cribl.Cloud Roles](cloud-sso-okta-groups-3.2cae240328.png)
{border="true"}

This example shows how groups configuration might look in Microsoft Entra ID:

![Mapping Microsoft Entra ID groups to Cribl.Cloud Roles](cloud-sso-microsoftentraid-groups.png)
{border="true"}
