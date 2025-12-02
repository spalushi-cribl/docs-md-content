# Roles


Define and manage access-control Roles and Policies

---

The Roles/Policies model exists in parallel with a more flexible [Members/Permissions](members) model, which will eventually replace it.
Cross-compatible [Default Roles](#default-roles) and [Default Policies](#permissions)
support customers who still choose to configure [Local Users](local-users) with Roles and Policies.
Your configured Local Users appear interchangeably in the new Members UI, and vice versa. 

> See [Which Access Method Should I Use?](access-management#restrictions)
> for detailed differences between the two access control systems.
> 
{.box .success}

## Availability

Role-based access control is available only on distributed deployments
([Stream](/stream/deploy-distributed), [Edge](/edge/setting-up-leader-and-edge-nodes)) with an [Enterprise license tier](https://cribl.io/pricing/).
 
On single-instance deployments or distributed deployments with other licenses all users will have full administrative privileges.

> ##### Known Issue
> 
> Existing Local Users display in **Settings** > **Global** > **Members and Teams** with the `No Access` Permission
> even if they've been assigned a higher Role.
> This is a display-only bug: These users' original Roles still function as configured.
> For details and fix progress, please see [Known Issues](/results/?query=&searchOption=known-issues&tickets=cribl-18973).
>
{.box .warning}

## RBAC Concepts

Cribl Stream's RBAC mechanism is designed around the following concepts, which you manage in the UI:

- **Roles**:  Logical entities that are associated with one or multiple **Policies** (groups of permissions). You use each Role to consistently apply these permissions to multiple Cribl Stream **users**.

- **Policies**: A set of **permissions**. A Role that is granted a given Policy can access, or perform an action on, a specified Cribl Stream object or objects.

- **Permissions**: Access rights to navigate to, view, change, or delete specified **objects** in Cribl Stream.
 
- **Users**: You map Roles to Cribl Stream users in the same way that you map **user groups** to users in LDAP and other common access-control frameworks.

> Users are independent Cribl Stream objects that you can configure even without RBAC enabled. For details, see [Local Users](local-users).
>
{.box .success}

Cribl Stream Roles can be integrated with external authorization/IAM mechanisms, such as LDAP and OIDC and mapped to their respective groups, tags, etc.

## Using Roles {#roles}

Cribl Stream ships with a set of default Roles, which you can supplement.

### Default Roles {#default-roles}



These Roles ship with Cribl Stream by default. 

#### Organization-Level Roles {#org}

Note that some of the Roles below have no counterpart Permission in Cribl's newer Members/Permissions model.

Name | Description | Permission Equivalent
--- | --- | --- |
**admin** | Superuser – authorized to do anything in the system. | Organization [Admin](permissions#organization)
**gitops** | Ability to sync the Cribl Stream deployment to a remote Git repository. | N/A
**notification_admin** | Read/write access to all Notifications. | N/A
**user** | Default Role that gets only a home/landing page to authenticate. This is a fallback for users who have not yet been assigned a higher Role by an admin. | Organization [User](permissions#organization)
**project_user** | Read/write access to the simplified Stream [Projects](/stream/projects) UI and related data Subscriptions. **Deprecated as of v.4.2.** <br/>  <br/> Instead [assign](/stream/sharing-projects) **Editor**, as the most comparable new Project-level Permission. The more-permissive **Maintainer**, and the more-restrictive **Read Only**, are also available Permissions. | Project [Editor](/stream/sharing-projects#permissions-list)

#### Stream Roles

Name | Description | Permission Equivalent
--- | --- | --- |
**stream_user** | Basic Role for Stream. | Stream [**User**](permissions#product).
**stream_reader** | Allows viewing all Members, Worker Groups, Settings, Leader commits, and legacy Local Users and Roles, with no configuration capabilities. | Stream [**Read Only**](permissions#product).
**stream_editor** | Allows viewing all Groups and Monitoring pages. | Stream [**Editor**](permissions#product).
**stream_admin** | Superuser Role at the Product level | Stream [**Admin**](permissions#product).

#### Edge Roles

Name | Description | Permission Equivalent
--- | --- | --- |
**edge_user** | Basic Role for Edge. | Edge [**User**](permissions#product).
**edge_reader** | Allows viewing all Members, Fleets, Settings, Leader commits, and legacy Local Users and Roles, with no configuration capabilities. | Edge [**Read Only**](permissions#product).
**edge_editor** | Allows viewing all Groups and Monitoring pages. | Edge [**Editor**](permissions#product).
**edge_admin** | Superuser Role at the Product level. | Edge [**Admin**](permissions#product).

#### Worker Group–Level Roles {#group-level-roles}

Name | Description | Permission Equivalent
--- | --- | --- |
**owner_all** | Read/write access to (and Deploy permission on) all Worker Groups. | N/A
**editor_all** | Read/write access to all Worker Groups. | N/A
**reader_all** | Read-only access to all Worker Groups. | N/A
**collect_all** | Ability to create, configure, and run [Collection jobs](/stream/collectors) on all Worker Groups. | N/A

Cribl **strongly recommends** that you do not edit or delete these default Roles. However, you can readily [clone them](#add-roles), and modify the duplicates to meet your needs.

#### Cribl Search and Cribl Lake RBAC

Because [Cribl Search](/search/) and [Cribl Lake](/lake/) are available only in Cribl.Cloud, they use Cribl's newer [Members/Permissions](members) access-control system. For details about managing access to these products and their resources, see:

- [Search Member Permissions](/search/cloud-managing#search-member-roles)

- [Cribl Lake Permissions](/lake/lake-permissions/)

> ##### Initial Installation or Upgrade
>
> When you first install Cribl Stream with the prerequisites to enable RBAC (Distributed deployment with an [Enterprise license](https://cribl.io/pricing/)), you will be granted the **admin** Role. Using this Role, you can then define and apply additional Roles for other users.
> 
> You will similarly be granted the **admin** Role upon upgrading an existing Cribl Stream installation from pre-2.4 versions to v. 2.4 or higher. This maintains backwards-compatible access to everything your organization has configured under the previous Cribl Stream version's single Role.
>
{.box .success}

### Adding and Modifying Roles {#add-roles}

To add a new Role:

1. In the sidebar, select **Settings**, then **Global**.
1. Under **Access Management**, select **Roles**.
1. Select **Add Role**.
1. Provide the Role **Name**.
1. Select [**Policies**](#policies) for this Role.

### Adding and Removing Policies {#policies}

The **Policies** section is an expandable table. In each row, you select a Policy using the left drop-down, and apply that Policy to objects (i.e., assign permissions on those objects) using the right drop-down.

Let's highlight an example from the [above screen capture](#add-edit-role) of Cribl Streams built-in Roles: The `editor_all` Role has the `GroupEdit` Policy, with permission to exercise it on any and all Worker Groups (as indicated by the `*` wildcard).

![Policies on the left, objects on the right](policies-and-permissions.66a1801f45.png)
{border="true"}

To add a new Policy to a Role:

1. Select **Add Policy** to add a new row to the **Policies** table.
1. Select a Policy from the left column drop-down.
1. Accept the default object on the right; or select one from the drop-down.

To modify an already-assigned Policy, just edit its row's drop-downs in the **Policies** table.

To remove a Policy from the Role, select the **X** icon at right.

In all cases, click **Save** to confirm your changes and close the modal.

### Default Policies {#permissions}

In the **Policies** table's left column, the drop-down offers the following default Policies:

#### Worker Group-Level Policies

Name | Description | Permission Equivalent
--- | --- | ---
`GroupRead` | The most basic Worker Group-level permission. Enables users to view a Worker Group and its configuration, but not modify or delete the config. | Worker Group-level [**Read Only**](permissions#group).
`GroupEdit` | Building on `GroupRead`, grants the ability to also change and commit a Worker Group's configuration. | Worker Group-level [**Editor**](permissions#group).
`GroupFull` | Building on `GroupEdit`, grants the ability to also deploy a Worker Group. | Worker Group-level [**Admin**](permissions#group).
`GroupCollect` | Grants the ability to create, configure, and run Collectors on a Worker Group. | N/A
`GroupUser` | Access Worker Group. | Worker Group-level [**User**](permissions#group).

#### Project-Level Policies

Name | Description | Permission Equivalent
--- | --- | ---
`ProjectMaintain` | Grants ability to edit or delete the Project and its settings. | Project-level [**Maintainer**](/stream/sharing-projects#permissions-list).
`ProjectRead` | Can configure connections among the Project's Subscriptions, Packs, and Destinations, but cannot modify or delete these resources. | Project-level [**Read Only**](/stream/sharing-projects#permissions-list).
`ProjectEdit` | Can view Project and Subscription settings and connections, but not modify or delete them. | Project-level [**Editor**](/stream/sharing-projects#permissions-list).

#### Product-Level Policies

Name | Description | Permission Equivalent
--- | --- | ---
`ProductUser` | Makes the Member assignable to Worker Groups and resources, with no initial access to any. | Product-level [**User**](permissions#product).
`LimitedProductUser` | Similar to `ProductUser`, but omits the ability to read or act on all the endpoints within a Worker Group. | N/A
`ProductAdmin` | Superuser Permission at the Product level. | Product-level [**Admin**](permissions#product).

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

In the **Policies** table's right column, use the drop-down to select the Cribl Stream objects on which the left column's Policy will apply. The objects available for selection are specific Worker Groups, or a wildcard representing all Worker Groups. For example:

- `Worker Group <id>`
- `NewGroup2`
- `default` (Worker Group)
- `*` (all Worker Groups)

## Extending Default Roles

Here's a basic example that ties together the above concepts and facilities. It demonstrates how to add a Role whose permissions are restricted to a particular Worker Group.

Here, we've cloned the `editor_all` Role that we unpacked [above](#policies). We've named the clone `editor_default`. 

We've kept the `GroupEdit` Policy from `editor_all`. But in the right column, we're restricting its object permissions to the `default` Worker Group that ships with Cribl Stream.

![Cloning a default Role](role-new.76bbe3be3f.png)
{border="true"}

You can readily adapt this example to create a Role that has permissions on an arbitrarily named Worker Group of your own.

## Roles and Users

Once you've defined a Role, you can associate it with Cribl Stream users. For details, see [Local Users](local-users). 

Note that when you assign multiple Roles to a given user, the Roles' permissions are additive: This user is granted a superset of all the permissions contained in all the assigned Roles.

By default, Cribl Stream will log out a user upon a change in their assigned Roles. You can defeat this behavior at  **Settings** > **Global** > **General Settings** > **API Server Settings** > **Advanced** > **Logout on roles change**.

## External Groups and Cribl Stream Roles {#groups}

You can map user groups from external identity providers (LDAP, Splunk, or OIDC) to Cribl Stream Roles, as follows:

1. In the sidebar, select **Settings**, then **Global**.
1. Under **Access Management**, select **Authentication**
1. Choose **LDAP**, **Splunk**, or **OpenID Connect** as authentication **Type**.
1. Configure your identity provider's connection and other basics. (For configuration details, see the appropriate [Authentication](authentication) section.)
1. Under **Role Mapping**, first select a Cribl Stream **Default role** to apply to external user groups that have no explicit Cribl Stream mapping defined below.
1. Next, map external groups as you've configured them in your external identity provider (left field below) to Cribl Stream Roles (right drop-down list below).
1. To map more user groups, select **Add Mapping**.
1. When your configuration is complete, select **Save**.
