# Adding Users to Projects {#access}


As a Stream admin, you control Members' access to individual Projects by assigning Permissions to those Members, as outlined below.

> ##### Breaking Change
>
> In Cribl Stream 4.2 and later, Projects and Subscriptions rely entirely on the fine-grained [Members/Permissions](/stream/members) access control first introduced in Stream 4.2. The 4.1.x `project_user` Role and `ProjectSourceSubscribe` Policy, which provided access to all Projects, are now retired. To migrate Projects configured in v.4.1.x, you'll find those projects' users visible in Stream as Members. Assign them appropriate new Permissions, as outlined here.
>
{.box .warning}

## Project-Level Permissions {#permissions-list}

You can share a Project with a Cribl Stream Member using the following Permissions, with some constraints [inherited](members#inheritance) from the Member's Permissions at higher levels (Cribl Organization, Cribl Stream, and Worker Group).

Permission | Description
--- | ---
**Maintainer** | Admin-level Permission. Allows freely editing or deleting the Project and its settings. (Shared, by [inheritance](members#inheritance), only with Members who have the `Admin` or `Editor` Permission at higher levels.)
**Editor** | Can configure connections among the Project's Subscriptions, Packs, and Destinations, but cannot modify or delete these resources. Can create, modify, and delete Pipelines within the Project. (**Use this Permission to share a Project with** [Project editors](projects#user).
**Read Only** | Can view Project and Subscription settings and connections, but not modify or delete them. (Designed specifically for support personnel. Members who have the product-level `Read Only` Permission will [inherit](members#inheritance), and will be locked to, this Permission.)
**No Access** | Self-explanatory: a Member's default state on a Project that has not (yet) been shared with them.

> Members who have the `User` Permission at the [top (Organization) level](members#permissions-org) are the most malleable at the Project level. These are the Members with whom you can readily share a Project-specific `Editor` Permission, to authorize them as a Project editor.
>
{.box .success}

## Sharing a Project (Example) {#sharing}

To share a Project with an eligible Cribl Stream Member:



1. Select **Settings** > **Stream Settings** > **Members**.
2. Make sure each applicable Member has the `User`  **Org Permission**, and the `User` **Stream Permission**. (Click an existing Member's row to adjust their Permissions.)

![Stream Settings > Members](members-stream-level2.5d7945c3d0.png)
{border="true"}

3. Click each applicable Member's row to open their **User Details** drawer.

![Stream Members > User Details](member-details-drawer2.cf1f7ded59.png)
{border="true"}

4. Grant the Member a `User` Permission on the applicable Project's parent Group. (Higher Permissions offered on this drop-down are not applicable to Project editors.)
5. Select **Manage** and click the same Group to give it focus.
6. Select **Projects** > **Data Projects** > **View all**.

![View all > All Projects modal](projects-all-modal.682afeecba.png)

7. In the resulting **All Projects** modal, click the Project's **Share** link to open the **Project Members** drawer. Here, you can see all the Group's Members with whom this Project can be (or has been) shared.

    > In Cribl Stream 4.3 and later, you can instead open an individual Project's **Sharing** drawer directly from the Project view. Give this Project focus in the Project view, then click the **Members** link at the top.
    {.box .success}

8. Grant each applicable Member an `Editor` Permission on the Project. (The alternative `Read Only` Permission is intended for support users. It does not enable configuring connections or Pipelines.)

![Project Sharing drawer – assigning `Editor` Permission](projects-share-drawer.b5521c4ebe.png)

9. Click **Save** to confirm your changes, then click outside the drawer to close it. 
10. Commit your changes, and deploy the Group if prompted.

These Members who now have Project `Editor` Permissions – and no higher Stream Permissions – view data filtered through the Projects UI. They also have read access to Cribl Stream [Data Monitoring](monitoring#data-monitoring) and [Notifications](notifications).

> You can reopen the same drawer to modify Members' Permission levels. To remove a Member from the Project, assign them `No Access`.
>
{.box .success}
