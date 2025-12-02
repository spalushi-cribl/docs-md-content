# Permissions


Define and manage fine-grained access control across Cribl products and resources.

---

You can assign different access and capabilities (Permissions) to each user ([Member](members)) or user group ([Teams](teams)) at multiple levels:

- [Organization](#organization).
- [Workspace](#workspace).
- [Product](#product): Stream, Edge, Search, or Lake.
- [Group/Fleet](#group).
- [Resource](#resource): Cribl Stream [Project](/stream/sharing-projects), Cribl Search [Dataset](/search/basic-concepts/#datasets) or [Dataset Provider](/search/basic-concepts/#dataset-providers), and so on.

## Organization-Level Permissions (Cribl.Cloud) {#organization}

You assign Permissions per individual user when you [invite them](#inviting-members) to your Organization, or after they join it.
Cribl.Cloud does not support globally predefining Permissions, as with on-prem Cribl Edge.

| Permission                                                                               | User | Admin | Owner |
| ---------------------------------------------------------------------------------------- | ---- | ------|------ |
| Log into the system                                                                      | ✓    | ✓     | ✓     |
| Update own Member profile                                                                | ✓    | ✓     | ✓     |
| View Stream Worker groups you have access to                                             | ✓    | ✓     | ✓     |
| `Admin` Access to all [Products](permissions#product)                            |      | ✓     | ✓     |
| View and execute Leader commits                                                          |      | ✓     | ✓     |
| View and modify Global Settings                                                          |      | ✓     | ✓     |
| Manage ACLs (Access Control lists)                                                       |      | ✓     | ✓     |
| View and update [SSO](cloud-sso) settings                                                |      | ✓     | ✓     |
| View Data Sources and Trust Policies                                                     |      | ✓     | ✓     |
| Manage API Credentials                                                                   |      | ✓     | ✓     |
| View and manage Cribl Suite Global Settings                                              |      | ✓     | ✓     |
| View and manage (provision, update, and delete) Stream Worker Groups                     |      | ✓     | ✓     |
| View the Organization's [Billing](cloud-portal#check-plans-credits-and-usage) dashboards |      | ✓     | ✓     |
| [Download invoices](cloud-billing#download-invoices)                                     |      | ✓     | ✓     |
| View Organization [Details](cloud-portal#configure-organization-details)                 |      | ✓     | ✓     |
| Update Organization Details                                                              |      |       | ✓     |
| Delete an Organization                                                                   |      |       | ✓     |
| View Organization Members                                                                |      | ✓     | ✓     |
| Manage (invite, update, delete) Organization Members                                     |      | ✓     | ✓     |

Each user can have Owner Permissions for multiple organizations,
and each organization can have multiple users as Owners.
However, each user – as defined by their email address –
can [register](cloud-initial-setup) only one active Organization,
and only if they are not already the Owner of a different Organization. 

> Cribl.Cloud does not use the earlier access-management model of Local Users and Roles/Policies that on-prem Cribl Edge still supports. You can create and configure only Members and Permissions – and optionally, [Teams](teams) – on Cribl.Cloud Organizations and applications ([Cribl Search](/search/cloud-managing#search-member-roles) and [Cribl Lake](/lake/lake-permissions)). References elsewhere in Cribl docs to Local Users, Roles, and Policies do not apply to Cribl.Cloud Organizations or applications.
>
{.box .info}

## Workspace-Level Permissions {#workspace}

Workspace-level Permissions differ between Cribl.Cloud and on-prem deployments.
Because multiple Workspaces are available only on [Cribl-Cloud](workspaces),
Workspace-level Permissions in on-prem deployments cover the whole Organization (that is, the whole deployment).
In Cribl.Cloud, you can set specific Permissions separately for each Workspace.

### Workspace-Level Permissions (Cribl.Cloud) {#workspace-cloud}

| Permission                                                                               | Member | Admin | Owner |
| ---------------------------------------------------------------------------------------- | ---- | ------| ----- |
| Log into the system | ✓ | ✓ | ✓ | 
| View ACLs, Workspace details and users, API credentials and billing information | ✓ | ✓ | ✓ |
| View default data sources and trust policy |  | ✓  | ✓ |
| Mange ACLs, API credentials, SSO settings |  | ✓  | ✓ |
| Invite new Members to the Workspace, update and delete Workspace Members |  |  | ✓ |

### Workspace-Level Permissions (on-prem) {#workspace-on-prem}

| Permission                                                                               | User | Admin |
| ---------------------------------------------------------------------------------------- | ---- | ------|
| Log into the system | ✓ | ✓ |
| Create, view, update, and delete all Members. |  | ✓ |

An **Admin** Permission [inherits](#inheritance) Product-level **Admin** Permissions and resource-level **Maintainer** Permissions.

## Product-Level Permissions (Stream and Edge) {#product} 

> For Cribl Search's more-specific Product-level Permissions, see [Search Member Permissions](/search/cloud-managing#search-member-roles).
> For Cribl Lake's Product-level Permissions, see [Lake Permissions](/lake/lake-permissions).
{.box .success}

| Permission                                                                               | User | Read Only | Editor | Admin |
| ---------------------------------------------------------------------------------------- | ---- | ----- | ----- | ----- |
| Can be assigned to Fleets and resources | ✓    | ✓     | ✓     | ✓     |
| View all Members, Fleets, Settings, Leader commits, and legacy Local Users and Roles |      | ✓     | ✓     | ✓     |
| View all Groups and Monitoring pages |      |       | ✓     | ✓     |
| Create, view, update, and delete all Fleets and resources |     |      |      | ✓     |
| Manage Fleet Mappings |     |      |      | ✓     |
| Add, update, restart Edge Nodes |     |      |      | ✓     |
| Manage Notifications and Notification Targets |     |      |      | ✓     |

Product-level Permissions inherit Permissions of a different level in the following manners:

- A **Read Only** Permission automatically [inherits](#inheritance) **Read Only** Permissions on all Fleets and lower-level resources.
- An **Editor** Permission [inherits](#inheritance) the **Editor** Permission on Fleets, and the **Maintainer** Permission on lower-level resources.
- An **Admin** Permission [inherits](#inheritance) from the [Organization-level](#permissions-org) **Admin** Permission in on-prem deployments, and from the Org-level **Owner** Permission in [Cribl.Cloud](permissions#organization) deployments. Automatically inherits **Admin** Permissions on all Fleets and  **Maintainer** Permissions on lower-level resources.

## Group- and Fleet-Level Permissions {#group}

Cribl Edge Permissions at the Fleet level generally mirror those at the Product level:

| Permission                                                                               | User | Read Only | Collect | Editor | Admin |
| ---------------------------------------------------------------------------------------- | ---- | ----- | ----- | ----- | ----- |
| Can be assigned to resources within the Fleet | ✓    | ✓     |       | ✓     | ✓     |
| View all Fleet-level Settings, encryption keys, certificates, secrets, scripts  |      | ✓     |      | ✓     | ✓     |
| View all Sources, Destinations, Pipelines, Packs, Routes, QuickConnect connections, Knowledge objects, Notifications and Notification targets |      | ✓     |      | ✓     | ✓     |
| View all Edge Subfleets and their Settings, and Stream Projects and Subscriptions |      | ✓     |      | ✓     | ✓     |
| Run Collection jobs on the Fleet |     |      | ✓     | ✓     | ✓     |
| Create, view, update, and delete all Fleet-level encryption keys, certificates, secrets, scripts, Sources, Destinations, Pipelines, Packs, Routes, QuickConnect connections, Knowledge objects, and Notifications and Notification targets |     |      |       | ✓     | ✓     |
| Commit configuration changes  |     |      |       |✓     | ✓     |
| Create, view, update, and delete all Fleet-level access management (Members' Permissions), Settings, encryption keys, key management system (KMS) settings, certificates, secrets, scripts |     |      |      |      | ✓     |
| Create, view, update, and delete all Sources, Destinations, Pipelines, Packs, Routes, QuickConnect connections, Knowledge objects, Notifications and Notification targets, and Stream Projects/Subscriptions |     |      |      |      | ✓     |
| Add, update, and restart Edge Nodes. |     |      |      |      | ✓     |
| Commit and deploy configuration changes. |     |      |      |      | ✓     |
| On Edge, provides CRUD capabilities on Subfleets' Settings and access management identical to those on the parent Fleet. |     |      |      |      | ✓     |

Fleet-level Permissions inherit Permissions of a different level in the following manners:

- A **Read Only** Permission [inherits](#inheritance) **Read Only** Permissions on lower-level resources.
- An **Admin** Permission [inherits](#inheritance) **Maintainer** Permissions on lower-level resources.

### Assign Group-/Fleet-Level Permissions {#group-assign}

To assign Permissions on a Fleet:

1. Make sure this Member has a [**Workspace permission**](#workspace) level sufficient to support the Permission you want to assign on the Fleet.
1. In the sidebar, select **Settings**, then **Stream**/**Edge**, then **Members and Teams**.
1. Select each applicable Member's row to open their **Member Details** drawer.
1. Grant the Member the Permission level you want on this Fleet.

> Assigning Group-/Fleet-Level Permissions in only available in on-prem deployments,
> not in Cribl.Cloud.
{.box .cloud}

## Resource-Level Permissions {#resource}

Certain Cribl products provide Permission-based access to particular resources.

### Stream Projects {#projects}

Stream [Projects](/stream/sharing-projects) and Subscriptions rely entirely on the Members/Permissions access control system. These enable you to [assign](/stream/sharing-projects#sharing) different users different levels of access on individual Projects. 

> The 4.1.x `project_user` Role and `ProjectSourceSubscribe` Policy, which provided access to all Projects, are retired in Cribl Edge 4.2.x and later.
> **This is a breaking change.** If you are upgrading with Projects configured in v.4.1.x, those Projects' users will be visible in Stream as Members, and you'll need to assign them new Permissions.
>
{.box .warning}

For details, see [Adding Users to Projects](/stream/sharing-projects).

### Search Resources {#search}

For Permissions on Search resources, see the Search docs' [Sharing](/search/sharing) topic.

### Other Resources {#other-resources}

Access to certain resources can be managed only via legacy [Local Users](local-users) and [Roles](roles).

[GitOps](/stream/gitops) integration: Authorization to enable and manage GitOps requires the legacy `gitops` [Role](/stream/roles#general-roles). This legacy Role has no counterpart Permission.

Collectors: The `collect_all` legacy [Role](roles#group-level-roles) specifically enables creating, configuring, and running [Collection jobs](/stream/collectors) on all Stream Worker Groups. This Role has no counterpart Permission.

Notifications: The `notification_admin` legacy [Role](roles#general-roles) specifically enables creating and receiving all Notifications. This legacy Role has no counterpart Permission.

## Inheritance {#inheritance}

Members' Permissions at certain levels determine the Permissions that Admins can assign them at lower levels. Here is the current inheritance scheme:
- Members with the `Admin` Permission at the [Organization](#permissions-org) level will be locked to the `Admin` Permission on [Products](#permissions-product), to the `Editor` Permission on [Fleets](#group), and to the `Maintainer` Permission on lower-level [resources](#resource).
- Members with the Product-level `Editor` Permission will be locked to the `Editor` Permission on Fleets, and to the `Maintainer` Permission on lower-level resources.
- Members with the `User` Permission at each level can be assigned varying Permissions at the next level down. **`User` is the most malleable Permission.**
- On Stream [Projects](/stream/sharing-projects#permissions-list), the `Maintainer` Permission is available **only** to Members with higher-level `Admin` or `Editor` Permissions. (They're automatically assigned this Permission via inheritance.)
- [Search resources](/search/sharing#resources) are more flexible: Here, the `Maintainer` Permission can be shared with Members who have the `User` Permission at the Product level.
- Members with the Product-level `Read Only` Permission will be locked to the `Read Only` Permission on Fleets and on lower-level resources.
