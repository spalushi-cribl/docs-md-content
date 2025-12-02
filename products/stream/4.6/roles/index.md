# Roles


Define and manage access-control Roles and Policies

---

Cribl Stream offers role-based access control (RBAC) to serve these common enterprise goals:

- **Security**: Limit the blast radius of inadvertent or intentional errors, by restricting each user's actions to their needed scope within the application.

- **Accountability**: Ensure compliance, by restricting read and write access to sensitive data.

- **Operational efficiency**: Match enterprise workflows, by delegating access over subsets of objects/resources to appropriate users and teams.

> Role-based access control is available only on distributed deployments ([Stream](/stream/deploy-distributed), [Edge](/edge/setting-up-leader-and-edge-nodes)) with an Enterprise license or Cribl.Cloud [plan](cloud-enterprise). With other license/plan types and/or single-instance deployments, all users will have full administrative privileges.
>
{.box .info}

## Roles, Meet Permissions

In Cribl Stream 4.2 and later, the Roles/Policies model described on this page exists in parallel with a new, more flexible [Members/Permissions](members) model, which will eventually replace it. To provide cross-compatibility, we have added several new [Default Roles](#default-roles) and [Default Policies](#permissions) that are counterparts to new Permissions. 

As noted below, these cross-compatible Roles/Policies support customers who still choose to configure [Local Users](local-users) with Roles and Policies. Your configured Local Users appear interchangeably in the new Members UI, and vice versa. 

> ##### Known Issue {{< id `no-access` >}}
>
> In the current release, existing Local Users display incorrectly with `No Access` Permissions on **Settings** > **Members** pages. This is a display-only bug: These users' Roles still function as originally configured. For details and fix timeline, please see [Known Issues](known-issues#CRIBL-18973).
>
{.box .info}

## RBAC Concepts

Cribl Stream's RBAC mechanism is designed around the following concepts, which you manage in the UI:

- **Roles**:  Logical entities that are associated with one or multiple **Policies** (groups of permissions). You use each Role to consistently apply these permissions to multiple Cribl Stream **users**.

- **Policies**: A set of **permissions**. A Role that is granted a given Policy can access, or perform an action on, a specified Cribl Stream object or objects.

- **Permissions**: Access rights to navigate to, view, change, or delete specified **objects** in Cribl Stream.
 
- **Users**: You map Roles to Cribl Stream users in the same way that you map **user groups** to users in LDAP and other common access-control frameworks.

> Users are independent Cribl Stream objects that you can configure even without RBAC enabled. For details, see [Local Users](local-users).
>
{.box .success}

## How Cribl Stream RBAC Works

Cribl Stream RBAC is designed to grant arbitrary permissions over objects, attributes, and actions at arbitrary levels.



> In Cribl Stream v.2.4.x through 4.1.x, Roles are customizable only down to the Worker Group/Fleet level. E.g., you can grant Edit permission on Worker Group/Fleet `WG1` to User A and User B; but you cannot grant them finer-grained permissions on child objects such as Pipelines, Routes, etc. through the UI. 
> 
> Setting these granular permissions requires creating custom Policies in `policies.yml` [configuration files](policiesyml). Note that this option is currently supported only with on-prem (customer-managed) deployments, not on Cribl.Cloud.
>
{.box .info}

Cribl Stream's UI will be presented differently to users who are assigned Roles that impose access restrictions. Controls will be visible but disabled, and search and log results will be limited, depending on each user's permissions.

Access to the same objects via Cribl Stream's API and CLI will be similarly filtered, with appropriate error reporting. E.g., if a user tries to commit and deploy changes on a Worker Group/Fleet where they are not authorized, they might receive a CLI error message like this: `git commit-deploy command failed with err: Forbidden`

Cribl Stream Roles can be integrated with external authorization/IAM mechanisms, such as LDAP and OIDC and mapped to their respective groups, tags, etc.

## Using Roles {#roles}

Cribl Stream ships with a set of default Roles, which you can supplement.

### Default Roles {#default-roles}



These Roles ship with Cribl Stream by default. 

#### Organization-Level Roles {#org}

Note that some of the Roles below have no counterpart Permission in Cribl's newer Members/Permissions model.

Name | Description | Permission Equivalent
--- | --- | --- |
**admin** | Superuser – authorized to do anything in the system. | Organization [Admin](members#permissions-org)
**gitops** | Ability to sync the Cribl Stream deployment to a remote Git repository. | N/A
**notification_admin** | Read/write access to all Notifications. | N/A
**user** | Default Role that gets only a home/landing page to authenticate. This is a fallback for users who have not yet been assigned a higher Role by an admin. | Organization [User](members#permissions-org)
**project_user** | Read/write access to the simplified Stream [Projects](/stream/projects) UI and related data Subscriptions. **Deprecated as of v.4.2.** <br/>  <br/> Instead [assign](/stream/sharing-projects) **Editor**, as the most comparable new Project-level Permission. The more-permissive **Maintainer**, and the more-restrictive **Read Only**, are also available Permissions. | Project [Editor](/stream/sharing-projects#permissions-list)

#### Stream Roles

Name | Description | Permission Equivalent
--- | --- | --- |
**stream_user** | Basic Role for Stream. | Stream [**User**](members#permissions-product).
**stream_reader** | Allows viewing all Members, Worker Groups, Settings, Leader commits, and legacy Local Users and Roles, with no configuration capabilities. | Stream [**Read Only**](members#permissions-product).
**stream_editor** | Allows viewing all Groups and Monitoring pages. | Stream [**Editor**](members#permissions-product).
**stream_admin** | Superuser Role at the Product level | Stream [**Admin**](members#permissions-product).

#### Edge Roles

Name | Description | Permission Equivalent
--- | --- | --- |
**edge_user** | Basic Role for Edge. | Edge [**User**](members#permissions-product).
**edge_reader** | Allows viewing all Members, Fleets, Settings, Leader commits, and legacy Local Users and Roles, with no configuration capabilities. | Edge [**Read Only**](members#permissions-product).
**edge_editor** | Allows viewing all Groups and Monitoring pages. | Edge [**Editor**](members#permissions-product).
**edge_admin** | Superuser Role at the Product level. | Edge [**Admin**](members#permissions-product).

#### Worker Group–Level Roles

Name | Description | Permission Equivalent
--- | --- | --- |
**owner_all** | Read/write access to (and Deploy permission on) all Worker Groups. | N/A
**editor_all** | Read/write access to all Worker Groups. | N/A
**reader_all** | Read-only access to all Worker Groups. | N/A
**collect_all** | Ability to create, configure, and run [Collection jobs](/stream/collectors) on all Worker Groups. | N/A

#### Cribl Search Roles

Name | Description | Permission Equivalent
--- | --- | --- |
**search_user** | Basic Role for Cribl Search. | Search [**User**](members#permissions-product).
**search_editor** | Manage Datasets, Dataset Providers, Dashboards and settings. | Search [**Editor**](members#permissions-product).
**search_admin** | Superuser Role at the Product level. | Search [**Admin**](members#permissions-product).

Cribl **strongly recommends** that you do not edit or delete these default Roles. However, you can readily clone them (see the **Clone Role** button in the [next section](#add-roles)'s screenshots), and modify the duplicates to meet your needs. 

> ##### Initial Installation or Upgrade
>
> When you first install Cribl Stream with the prerequisites to enable RBAC (Enterprise license and distributed deployment), you will be granted the **admin** Role. Using this Role, you can then define and apply additional Roles for other users.
> 
> You will similarly be granted the **admin** Role upon upgrading an existing Cribl Stream installation from pre-2.4 versions to v. 2.4 or higher. This maintains backwards-compatible access to everything your organization has configured under the previous Cribl Stream version's single Role.
>
{.box .success}

### Adding and Modifying Roles {#add-roles}

In a distributed environment, you manage Roles at the Leader level, for the entire deployment. On the Leader Node, select **Settings** > **Global Settings** > **Access Management** > **Roles**. 

![Manage Roles page](manage-roles.ae81e42396.png)

To add a new Role, click **New Role** at the upper right. To edit an existing Role, click anywhere on its row. Here again, either way, the resulting modal offers basically the same options.
{{< id `add-edit-role` >}}

![Add/edit Role modal](add-edit-role-simple.3b04dc119b.png)

The options at the modal's top and bottom are nearly self-explanatory:

**Role name**: Unique name for this Role. Cannot contain spaces.

**Description**: Optional free-text description.

**Delete Role**: And...it's gone. (But first, there's a confirmation prompt. Also, you cannot delete a Role assigned to an active user.)

**Clone Role**: Opens a **New role** version of the modal, duplicating the **Description** and **Policies** of the Role you started with.

The modal's central **Policies** section (described below) is its real working area.

## Adding and Removing Policies {#policies}

The **Policies** section is an expandable table. In each row, you select a Policy using the left drop-down, and apply that Policy to objects (i.e., assign permissions on those objects) using the right drop-down.

Let's highlight an example from the [above screen capture](#add-edit-role) of Cribl Streams built-in Roles: The `editor_all` Role has the `GroupEdit` Policy, with permission to exercise it on any and all Worker Groups/Fleets (as indicated by the `*` wildcard).

![Policies on the left, objects on the right](policies-and-permissions.66a1801f45.png)

To add a new Policy to a Role:

1. Click **Add Policy** to add a new row to the **Policies** table.

2. Select a Policy from the left column drop-down.

3. Accept the default object on the right; or select one from the drop-down.

To modify an already-assigned Policy, just edit its row's drop-downs in the **Policies** table.

To remove a Policy from the Role, click its close box at right.

In all cases, click **Save** to confirm your changes and close the modal.

### Default Policies {#permissions}

In the **Policies** table's left column, the drop-down offers the following default Policies:

#### Worker Group-Level Policies

Name | Description | Permission Equivalent
--- | --- | ---
`GroupRead` | The most basic Worker Group-level permission. Enables users to view a Worker Group and its configuration, but not modify or delete the config. | Worker Group-level [**Read Only**](members#group).
`GroupEdit` | Building on `GroupRead`, grants the ability to also change and commit a Worker Group's configuration. | Worker Group-level [**Editor**](members#group).
`GroupFull` | Building on `GroupEdit`, grants the ability to also deploy a Worker Group. | Worker Group-level [**Admin**](members#group).
`GroupCollect` | Grants the ability to create, configure, and run Collectors on a Worker Group. | N/A
`GroupUser` | Access Worker Group. | Worker Group-level [**User**](members#group).

#### Project-Level Policies

Name | Description | Permission Equivalent
--- | --- | ---
`ProjectMaintain` | Grants ability to edit or delete the Project and its settings. | Project-level [**Maintainer**](/stream/sharing-projects#permissions-list).
`ProjectRead` | Can configure connections among the Project's Subscriptions, Packs, and Destinations, but cannot modify or delete these resources. | Project-level [**Read Only**](/stream/sharing-projects#permissions-list).
`ProjectEdit` | Can view Project and Subscription settings and connections, but not modify or delete them. | Project-level [**Editor**](/stream/sharing-projects#permissions-list).

#### Product-Level Policies

Name | Description | Permission Equivalent
--- | --- | ---
`ProductUser` | Makes the Member assignable to Worker Groups and resources, with no initial access to any. | Product-level [**User**](members#permissions-product).
`LimitedProductUser` | Similar to `ProductUser`, but omits the ability to read or act on all the endpoints within a Worker Group. | N/A
`ProductAdmin` | Superuser Permission at the Product level. | Product-level [**Admin**](members#permissions-product).

#### General Policies

Name | Description | Permission Equivalent
--- | --- | ---
`*` (wildcard) | Grants **all** permissions on associated objects. | N/A

#### Internal Policies

The following policies are internal building blocks for Product-specific Policies. Do not add them directly to Roles.

Name | Description | Permission Equivalent
--- | --- | ---
`Product` | N/A | N/A
`BaseProductUser` | N/A | N/A
`MaintainBase` | N/A | N/A

> ##### Use Policies As-Is
>
> By design, the default Policies that ship with Cribl Stream cannot be modified via the UI or API. Do not attempt to modify them by other means. Breaking the built-in model could undermine your intended access-control protections, opening your deployment and data to security vulnerabilities.
>
{.box .warning}

### Objects and Permissions

In the **Policies** table's right column, use the drop-down to select the Cribl Stream objects on which the left column's Policy will apply. (Remember that in v. 2.4, the objects available for selection are specific Worker Groups/Fleets, or a wildcard representing all Worker Groups/Fleets.) For example:

- `Worker Group <id>`
- `NewGroup2`
- `default` (Worker Group)
- `*` (all Worker Groups)

## Extending Default Roles

Here's a basic example that ties together the above concepts and facilities. It demonstrates how to add a Role whose permissions are restricted to a particular Worker Group/Fleet.

Here, we've cloned the `editor_all` Role that we unpacked [above](#policies). We've named the clone `editor_default`. 

We've kept the `GroupEdit` Policy from `editor_all`. But in the right column, we're restricting its object permissions to the `default` Worker Group/Fleet that ships with Cribl Stream.

![Cloning a default Role](role-new.76bbe3be3f.png)

You can readily adapt this example to create a Role that has permissions on an arbitrarily named Worker Group/Fleet of your own.

## Roles and Users

Once you've defined a Role, you can associate it with Cribl Stream users. On the Leader Node, select **Settings** > **Global Settings** > **Access Management** > **Local Users**. For details, see [Local Users](local-users). 

Note that when you assign multiple Roles to a given user, the Roles' permissions are additive: This user is granted a superset of all the permissions contained in all the assigned Roles.

By default, Cribl Stream will log out a user upon a change in their assigned Roles. You can defeat this behavior at  **Settings** > **Global Settings** > **General Settings > API Server Settings > Advanced > Logout on roles change.**

## External Groups and Cribl Stream Roles {#groups}

You can map user groups from external identity providers (LDAP, Splunk, or OIDC) to Cribl Stream Roles, as follows:

1. Select **Settings** > **Global Settings** > **Access Management** > **Authentication**. 

1. From the **Type** drop-down, select **LDAP**, **Splunk**, or **OpenID Connect**, according to your needs.

1. On the resulting **Authentication Settings** page, configure your identity provider's connection and other basics. (For configuration details, see the appropriate [Authentication](authentication) section.)

1. Under **Role Mapping**, first select a Cribl Stream **Default role** to apply to external user groups that have no explicit Cribl Stream mapping defined below.

1. Next, map external groups as you've configured them in your external identity provider (left field below) to Cribl Stream Roles (right drop-down list below).

1. To map more user groups, click **Add Mapping**.

1. When your configuration is complete, click **Save**.

Here's a composite showing the built-in Roles available on both the **Default role** and the **Mapping** drop-downs:

![Mapping external user groups to Cribl Stream Roles](role-ldap-mapping-composite.642d315413.png)

And here, we've set a conservative **Default Role** and one explicit **Mapping**:

![External user groups mapped to Cribl Stream Roles](role-ldap-mapped.04460283ba.png)


