# Members and Permissions


Define and manage fine-grained access control across Cribl products and resources

---

Members and Permissions enable fine-grained access control and authorization.
You can assign different access and capabilities (Permissions) to each user (Member) at multiple levels:

- [Organization](#organizations).
- [Product](#permissions-product): Stream, Edge, or Search.
- [Group/Fleet](#group).
- Resource: Stream [Project](/stream/sharing-projects), Search [Dataset](/search/basic-concepts/#datasets) or [Dataset Provider](/search/basic-concepts/#dataset-providers), and so on.

## Availability

Permission-based access control is available on:

- Customer-managed Distributed deployments ([Stream](/stream/deploy-distributed), [Edge](/edge/setting-up-leader-and-edge-nodes)) with an Enterprise license.
- All Cribl.Cloud deployments, at the Organization level. (A Cribl.Cloud Enterprise [plan](cloud-enterprise) also offers granular access control on products, Fleets, and lower-level resources.)
 
>On customer-managed single-instance deployments, or distributed deployments with other licenses, all users will have full `Admin`-level privileges.
{.box .info}

### Initial Permissions

When you first deploy Cribl Edge with the above prerequisites, you will be granted the Organization-level `Admin` Permission. Using this Permission, you can then assign additional Permissions to yourself and other Members, as outlined below in [Organizations](#organizations) and [Assigning Group-/Fleet-Level Permissions](#group-assign).

## Cross-Compatibility

Members and Permissions are available as the successors to Cribl's original role-based access control (RBAC) model
of [Local Users](local-users) and [Roles](roles)/[Policies](roles#policies). The earlier model is still supported across most of the Cribl product suite.

However, [Cribl.Cloud invitations](cloud-managing) and [Stream Projects](/stream/sharing-projects) have fully transitioned to the new model. For detailed differences between the two access control systems, see [Which Access Method Should I Use?](access-management#restrictions).

> ##### Known Issue
> 
> Existing Local Users display on **Settings** > **Members** pages with the `No Access` Permission
> even if they've been assigned a higher Role.
> This is a display-only bug: These users' original Roles still function as configured.
> For details and fix progress, please see [Known Issues](known-issues#CRIBL-18973).
>
{.box .warning}

## Organizations

The Members and Permissions model uses the concept of Organization as a container for the deployment of a whole suite of Cribl products (Stream, Edge, and Cloud-only Search and Lake).

In Cribl.Cloud, each account has one Organization. Use [Workspaces](workspaces) for multi-tenancy within your Organization. Contact Cribl Support if you have a use case that requires more than one Organization.

To access the Organization-level Members as an Admin:

1. From Cribl Edge's top nav, select **Settings** > **Global Settings**.

1. From the left nav, select **Members and Teams**.

1. Here, you can click an existing Member's row to change their access Permissions or other details, or click **Add Member** to grant a new user access to your Cribl Organization.

![Organization > Members/Permissions UI](permissions-org-level-4.7.png)
{border="true"}

> For Cribl.Cloud's counterpart to this on-prem UI for managing Organization-level Permissions, see [Inviting Members](cloud-managing#inviting-members).
>
{.box .cloud}

### Members and Local Users {#members-users}

Members and Local Users are interchangeable: Members can be reconfigured using [legacy Roles](roles) instead of Permissions,
and anyone you add to **Local Users** will also show in **Members**.

## Organization- and Product-Level Permissions {#permissions-top}

Each newly created Member starts out with **User** Permission at the Organization level, and with the self-explanatory **No Access** Permission on each product. You can use the toggles to assign the following Permission levels.

> For Cribl.Cloud Organization-level Permissions, see Cribl.Cloud docs' [Member Permissions](cloud-managing#member-roles) section.
>
{.box .cloud}

### Organization-Level Permissions {#permissions-org} 

Permission | Description
--- | ---
**User** | The most basic Permission. At the Organization level, gets the Member into the system, without conferring any access to peer Members, products, or lower-level resources. (Admins can freely assign these Members varying Permissions at lower levels.)
**Admin** | This is a superuser Permission. At the Organization level, allows creating, viewing, updating, and deleting all Members. (Automatically [inherits](#inheritance) Product-level `Admin` Permissions and resource-level `Maintainer` Permissions, which confer comparably broad capabilities.)

> For Stream and Edge, the Product-level Permissions below apply to both Cribl.Cloud and on-prem deployments.
> For Cribl Search's more-specific Product-level Permissions, see [Search Member Permissions](/search/cloud#search-member-roles).
> For Cribl Lake's Product-level Permissions, see [Lake Permissions](/lake/lake-permissions).
>
{.box .success}

### Product-Level Permissions (Stream and Edge) {#permissions-product} 

Permission | Description
--- | ---
**User** | The most basic Permission. At the Product level, makes the Member assignable to Fleets and resources, with no initial access to any.
**Read Only** | At the Product level, this Permission is designed specifically to enable support personnel to help other users by viewing (but not modifying) their configurations. Allows viewing all Members, Fleets, Settings, Leader commits, and legacy Local Users and Roles, with no configuration capabilities. (A `Read Only` Permission at the Product level automatically [inherits](#inheritance) `Read Only` Permissions on all Fleets and lower-level resources.)
**Editor** | Allows viewing all Groups and Monitoring pages. (Configuring these options requires `Admin`-level permission on the product and/or Fleet.) Automatically [inherits](#inheritance) the `Editor` Permission on Fleets, and the `Maintainer` Permission on lower-level resources.
**Admin** | This is a superuser Permission at the Product level. It allows creating, viewing, updating, and deleting all Fleets and resources; managing Fleet Mappings; adding, updating, and restarting Edge Nodes; and managing Notifications and Notification targets. (Automatically [inherited](#inheritance) from the [Organization-level](#permissions-org) `Admin` Permission in on-prem deployments, and from the Org-level `Owner` Permission in [Cribl.Cloud](cloud-managing#member-roles) deployments. Automatically inherits `Admin` Permissions on all Fleets and  `Maintainer` Permissions on lower-level resources.)

> Once Members are in your Organization, you can assign or reassign the above Organization- and Product-level Permissions. Return to **Settings** > **Global Settings** > **Members** and click the Member's row to reopen the same configuration modal.
>
{.box .success}

## Group- and Fleet-Level Permissions {#group}

Cribl Edge Permissions at the Fleet level generally mirror those at the Product level:

Permission | Description
--- | ---
**User** | The most basic Permission. Gets the Member into the Fleet, where Admins can then assign them access on individual resources. (Unless Members have a higher Permission at the [Product](#permissions-top) level, they have no initial access to Fleets or resources.)
**Read Only** | Designed for support personnel. Allows viewing all Fleet-level Settings, encryption keys, certificates, secrets, scripts, Sources, Destinations, Pipelines, Packs, Routes, QuickConnect connections, Knowledge objects, Notifications and Notification targets, Edge Subfleets and their Settings, and Stream Projects and Subscriptions, with no configuration capabilities. (Automatically [inherited](#inheritance) from a Product-level `Read Only` Permission. Automatically inherits `Read Only` Permissions on lower-level resources.)
**Editor** | Allows creating, viewing, updating, and deleting all Fleet-level encryption keys, certificates, secrets, scripts, Sources, Destinations, Pipelines, Packs, Routes, QuickConnect connections, Knowledge objects, and Notifications and Notification targets. Allows committing configuration changes, but not deploying them. (An `Editor` Permission at the [Product](#permissions-product) level automatically [inherits](#inheritance) `Editor` Permissions on all Fleets.)
**Admin** | Superuser Permission. Allows creating, viewing, updating, and deleting all Fleet-level access management (Members' Permissions), Settings, encryption keys, key management system (KMS) settings, certificates, secrets, scripts, Sources, Destinations, Pipelines, Packs, Routes, QuickConnect connections, Knowledge objects, Notifications and Notification targets, and Stream Projects/Subscriptions. <br/>  <br/> Allows adding, updating, and restarting Edge Nodes. Allows committing and deploying configuration changes. On Edge, provides CRUD capabilities on Subfleets' Settings and access management identical to those on the parent Fleet. (Automatically [inherited](#inheritance) from the [Product](#permissions-product)-level `Admin` Permission. Automatically inherits `Maintainer` Permissions on lower-level resources.)

### Assigning Group-/Fleet-Level Permissions {#group-assign}



To assign Permissions on a Fleet:

1. Select **Settings** > **Global Settings** > **Members**.
2. Make sure this Member has an **Org Permission** level sufficient to support the Permission you want to assign on the Fleet.
3. From the top-left product switcher, select **Cribl Edge**. Then select (depending on the product) **Settings** > **Stream Settings** > **Members** or **Settings** > **Edge Settings** > **Members**.

![Edge Settings > Members](members-stream-level.9afb60a715.png)
{border="true"}



4. Click each applicable Member's row to open their **User Details** drawer.

![Product Members > User Details](member-details-drawer.b704f463c4.png)
{border="true"}

5. Grant the Member the Permission level you want on this Fleet.

## Resource-Level Permissions {#resource}

Certain Cribl products provide Permission-based access to particular resources.

### Stream Projects {#projects}

Stream [Projects](/stream/sharing-projects) and Subscriptions rely entirely on the Members/Permissions access control system. These enable you to [assign](/stream/sharing-projects#sharing) different users different levels of access on individual Projects. 

> The 4.1.x `project_user` Role and `ProjectSourceSubscribe` Policy, which provided access to all Projects, are retired in Cribl Edge 4.2.x and later.
> **This is a breaking change.** If you are upgrading with Projects configured in v.4.1.x, those Projects' users will be visible in Stream as Members, and you'll need to assign them new Permissions.
>
{.box .warning}

For details, see [Adding Users to Projects](/stream/sharing-projects).

### Search Resources {#search}

For Permissions on Search resources, see the Search docs' [Sharing](/search/sharing) topic.

### Other Resources {#other-resources}

Access to certain resources can be managed only via legacy [Local Users](local-users) and [Roles](roles).

[GitOps](/stream/gitops) integration: Authorization to enable and manage GitOps requires the legacy `gitops` [Role](/stream/roles#general-roles). This legacy Role has no counterpart Permission.

Collectors: The `collect_all` legacy [Role](roles#group-level-roles) specifically enables creating, configuring, and running [Collection jobs](/stream/collectors) on all Stream Worker Groups. This Role has no counterpart Permission.

Notifications: The `notification_admin` legacy [Role](roles#general-roles) specifically enables creating and receiving all Notifications. This legacy Role has no counterpart Permission.

## Inheritance {#inheritance}

Members' Permissions at certain levels determine the Permissions that Admins can assign them at lower levels. Here is the current inheritance scheme:
- Members with the `Admin` Permission at the [Organization](#permissions-org) level will be locked to the `Admin` Permission on [Products](#permissions-product), to the `Editor` Permission on [Fleets](#group), and to the `Maintainer` Permission on lower-level [resources](#resource).
- Members with the Product-level `Editor` Permission will be locked to the `Editor` Permission on Fleets, and to the `Maintainer` Permission on lower-level resources.
- Members with the `User` Permission at each level can be assigned varying Permissions at the next level down. **`User` is the most malleable Permission.**
- On Stream [Projects](/stream/sharing-projects#permissions-list), the `Maintainer` Permission is available **only** to Members with higher-level `Admin` or `Editor` Permissions. (They're automatically assigned this Permission via inheritance.)
- [Search resources](/search/sharing#resources) are more flexible: Here, the `Maintainer` Permission can be shared with Members who have the `User` Permission at the Product level.
- Members with the Product-level `Read Only` Permission will be locked to the `Read Only` Permission on Fleets and on lower-level resources.
