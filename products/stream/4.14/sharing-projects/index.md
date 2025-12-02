# Add Users to Projects {#access}


As a Stream admin, you control Members' access to individual Projects by assigning Permissions to those Members, as outlined below.

> ##### Breaking Change
>
> In Cribl Stream 4.2 and newer, Projects and Subscriptions rely entirely on the fine-grained [Members/Permissions](/stream/members) access control first introduced in Cribl Stream 4.2. The v.4.1.x `project_user` Role and `ProjectSourceSubscribe` Policy, which provided access to all Projects, are now retired. To migrate Projects configured in v.4.1.x, you'll find those projects' users visible in Cribl Stream as Members. Assign them appropriate new Permissions, as outlined here.
>
{.box .warning}

## Project-Level Permissions {#permissions-list}

You can share a Project with a Cribl Stream Member or Team using the following Permissions, with some constraints [inherited](permissions#inheritance) from the Member's Permissions at higher levels (Cribl Organization, Cribl Stream, and Worker Group).

Permission | Description
--- | ---
**Maintainer** | Admin-level Permission. Allows freely editing or deleting the Project and its settings. (Shared, by [inheritance](permissions#inheritance), only with Members/Teams who have the `Admin` or `Editor` Permission at higher levels.)
**Editor** | Can configure connections among the Project's Subscriptions, Packs, and Destinations, but cannot modify or delete these resources. Can create, modify, and delete Pipelines within the Project. (**Use this Permission to share a Project with** [Project editors](projects#user).)
**Read Only** | Can view Project and Subscription settings and connections, but not modify or delete them. (Designed specifically for support personnel. Members/Teams who have the product-level `Read Only` Permission will [inherit](permissions#inheritance), and will be locked to, this Permission.)
**No Access** | Self-explanatory: a Member's default state on a Project that has not (yet) been shared with them.

> Members or Teams who have the `User` Permission at the [top (Organization) level](permissions#permissions-org) are the most malleable at the Project level. These are the Members/Teams with whom you can readily share a Project-specific `Editor` Permission, to authorize them as a Project editor.
>
{.box .success}

## Share a Project (Example) {#sharing}

To share a Project with an eligible Cribl Stream Member or Team:

1. Select **Settings** > **Stream** > **Members and Teams**.
2. Make sure each applicable Member or Team has the `User` **Workspace Permission**, and the `User` **Stream Permission**. (Select an existing Member's row to adjust their Permissions.)

![Stream Settings > Members and Teams](members-stream-level-4.9.png)
{border="true"}

3. Select each applicable Member and Team's row to open their details drawer.

![Stream Members > User Details](member-details-drawer-4.9.png)
{border="true"}

4. Grant the Member or Team a `User` Permission on the applicable Project's parent Worker Group. (Higher Permissions offered on this drop-down are not applicable to Project editors.)
5. In the sidebar, select **Worker Groups** and select the same Worker Group.
6. Select **Projects** > **Data Projects** > **View all**.

![View all > All Projects modal](projects-all-modal-4.7.png)
{border="true"}

7. Select the Project's **Share** link to open the **Project Members and Teams** drawer. Here, you can see all the Group's Members and Teams with whom this Project can be (or has been) shared.
   
   You can also open an individual Project's **Sharing** drawer directly from the Project view. Give this Project focus in the Project view, then select the **Members and Teams** link at the top.

8. Grant each applicable Member or Team an `Editor` Permission on the Project. (The alternative `Read Only` Permission is intended for support users. It does not enable configuring connections or Pipelines.)

![Project Sharing drawer – assigning `Editor` Permission](projects-share-drawer-4.9.png)
{border="true"}

9. Select **Save** to confirm your changes, then select outside the drawer to close it. 
10. Commit your changes, and deploy the Group if prompted.

These Members and Teams who now have Project `Editor` Permissions – and no higher Stream Permissions – view data filtered through the Projects UI. They also have read access to Cribl Stream [Data Monitoring](monitoring#data-monitoring) and [Notifications](notifications).

> You can reopen the same drawer to modify Members' Permission levels. To remove a Member from the Project, assign them `No Access`.
>
{.box .success}
