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

The core pattern must be a contiguous string, with or without spaces.

Cribl's Permission mapping logic uses the core pattern to automatically map IdP groups to Cribl Permissions as follows:

- `Cribl` + a Role name: Map to [Organization-level Permissions](permissions#organization)
- `Cribl` + `Organization` + a Role name: Map to [Organization-level Permissions](permissions#organization)
- `Cribl` + a product name + a Role name: Map to the corresponding product-level Permissions ([Cribl Stream and Edge](permissions#product), [Cribl Search](/search/cloud-managing/#search-member-roles), or [Cribl Lake](/lake/lake-permissions/))

The mapping logic tables for [Organization-level Permissions](#org-permissions-mapping-logic) and [Product-level Permissions](#product-permissions-mapping-logic) list the **valid** IdP group names for each Role.

You can add prefixes and suffixes to the core pattern for IdP group names if needed. Cribl's Permission mapping logic ignores these prefixes and suffixes and bases Permissions only on the core pattern. See examples of valid IdP group names with prefixes and suffixes: [Organization-level](#org-permissions-valid-prefix-suffix) and [Product-level](#product-permissions-valid-prefix-suffix) Permissions.

### Organization-Level Permissions Mapping Logic {#org-permissions-mapping-logic}

The following table lists the valid IdP group names for [Organization-level Permissions](permissions#organization) mapping:

| Organization-Level Role | Valid IdP Group Names |
| ----------------------- | --------------------- |
| Owner     | ![Valid identity provider group name](check-verified-01.svg) `CriblOrganizationOwner`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Organization Owner`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Owner` |
| Admin     | ![Valid identity provider group name](check-verified-01.svg) `CriblOrganizationAdmin`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Organization Admin`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Admin` |
| IAM Admin   | ![Valid identity provider group name](check-verified-01.svg) `CriblOrganizationIAMAdmin`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Organization IAM Admin`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl IAM Admin` |
| User      | ![Valid identity provider group name](check-verified-01.svg) `CriblOrganizationUser`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Organization User`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl User` |
| Editor    | **(Deprecated; mapped to Admin)**<br><br>![Valid identity provider group name](check-verified-01.svg) `CriblOrganizationEditor`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Organization Editor` |
| Read Only | **(Deprecated; mapped to User)**<br><br>![Valid identity provider group name](check-verified-01.svg) `CriblOrganizationReadOnly`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Organization Read Only` |

> Even though Cribl maps IdP group names that do not include `Organization` or a product name to Organization-level permissions, we recommend adding `Organization` to such names for clarity.
{.box .success}

#### Valid IdP Group Names with Prefixes and Suffixes (Organization-Level Permissions) {#org-permissions-valid-prefix-suffix}

IdP group names can include prefixes and suffixes as long as they contain the [core pattern](#idp-group-name-mapping). Examples of valid IdP group names that include prefixes and suffixes include:

- ![Valid identity provider group name](check-verified-01.svg) `US Cribl Organization Owner`
- ![Valid identity provider group name](check-verified-01.svg) `Cribl Admin EU`
- ![Valid identity provider group name](check-verified-01.svg) `US Staging Org Cribl Owner-Team B`
- ![Valid identity provider group name](check-verified-01.svg) `Cribl IAM Admin US`
- ![Valid identity provider group name](check-verified-01.svg) `CriblOrganizationUserProd`

#### Invalid IdP Group Names (Organization-Level Permissions) {#org-permissions-invalid-names}

Examples of IdP group names that are **invalid** because they contain hyphens:

- ![Invalid identity provider group name](x-close.svg) `Cribl-Organization-Owner` 
- ![Invalid identity provider group name](x-close.svg) `Cribl-Owner`

Examples of IdP group names that are **invalid** because they contain underscores:

- ![Invalid identity provider group name](x-close.svg) `Cribl_Organization_User`
- ![Invalid identity provider group name](x-close.svg) `Cribl_User`

Examples of IdP group names that are **invalid** because they contain intervening characters that disrupt the [core pattern](#idp-group-name-mapping):

- ![Invalid identity provider group name](x-close.svg) `Cribl Org Admin`
- ![Invalid identity provider group name](x-close.svg) `Cribl EU Admin`

### Product-Level Permissions Mapping Logic {#product-permissions-mapping-logic}

The following table lists the valid IdP group names for [Cribl Stream and Edge Product-level Permissions](permissions#product) mapping:

| Cribl Stream and Edge Role | Valid IdP Group Names (Cribl Stream) | Valid IdP Group Names (Cribl Edge) |
| -------------------------- | ------------------------------------ | ---------------------------------- |
| Admin     | ![Valid identity provider group name](check-verified-01.svg) `CriblStreamAdmin`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Stream Admin` | ![Valid identity provider group name](check-verified-01.svg) `CriblEdgeAdmin`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Edge Admin` |
| Editor    | ![Valid identity provider group name](check-verified-01.svg) `CriblStreamEditor`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Stream Editor`| ![Valid identity provider group name](check-verified-01.svg) `CriblEdgeEditor`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Edge Editor` |
| Read Only | ![Valid identity provider group name](check-verified-01.svg) `CriblStreamReader`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Stream Reader`| ![Valid identity provider group name](check-verified-01.svg) `CriblEdgeReader`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Edge Reader` |
| User      | ![Valid identity provider group name](check-verified-01.svg) `CriblStreamUser`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Stream User` | ![Valid identity provider group name](check-verified-01.svg) `CriblEdgeUser`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Edge User` |

The following table lists the valid IdP group names for [Cribl Search Product-level Permissions](/search/cloud-managing/#search-member-roles) mapping:

| Cribl Search Role | Valid IdP Group Names |
| ----------------- | --------------------- |
| Admin     | ![Valid identity provider group name](check-verified-01.svg) `CriblSearchAdmin`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Search Admin` |
| Editor    | ![Valid identity provider group name](check-verified-01.svg) `CriblSearchEditor`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Search Editor` |
| User      | ![Valid identity provider group name](check-verified-01.svg) `CriblSearchUser`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Search User` |

The following table lists the valid IdP group names for [Cribl Lake Product-level Permissions](/lake/lake-permissions/) mapping:

| Cribl Lake Role | Valid IdP Group Names |
| --------------- | --------------------- |
| Admin     | ![Valid identity provider group name](check-verified-01.svg) `CriblLakeAdmin`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Lake Admin` |
| Editor    | ![Valid identity provider group name](check-verified-01.svg) `CriblLakeEditor`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Lake Editor` |
| Read Only | ![Valid identity provider group name](check-verified-01.svg) `CriblLakeReader`<br><br>![Valid identity provider group name](check-verified-01.svg) `Cribl Lake Reader` |

#### Valid IdP Group Names with Prefixes and Suffixes (Product-Level Permissions) {#product-permissions-valid-prefix-suffix}

IdP group names can include prefixes and suffixes as long as they contain the [core pattern](#idp-group-name-mapping). Examples of valid IdP group names that include prefixes and suffixes include:

- ![Valid identity provider group name](check-verified-01.svg) `US Cribl Stream Admin`
- ![Valid identity provider group name](check-verified-01.svg) `Cribl Edge User-EU`
- ![Valid identity provider group name](check-verified-01.svg) `Staging Org CriblSearchEditor-Team A`
- ![Valid identity provider group name](check-verified-01.svg) `CriblLakeReader Prod`

#### Invalid IdP Group Names (Product-Level Permissions) {#product-permissions-invalid-names}

Examples of IdP group names that are **invalid** because they contain hyphens:

- ![Invalid identity provider group name](x-close.svg) `Cribl-Stream-Admin` 
- ![Invalid identity provider group name](x-close.svg) `Cribl-Lake-Reader`

Examples of IdP group names that are **invalid** because they contain underscores:

- ![Invalid identity provider group name](x-close.svg) `Cribl_Edge_Reader`
- ![Invalid identity provider group name](x-close.svg) `Cribl_Search_User`

Examples of IdP group names that are **invalid** because they contain intervening characters that disrupt the [core pattern](#idp-group-name-mapping):

- ![Invalid identity provider group name](x-close.svg) `Cribl EU Search Admin`
- ![Invalid identity provider group name](x-close.svg) `Cribl Stream Dev User`

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
