# Members


Control access to Cribl products by inviting and managing Members.

---

Members work together with [Teams](teams) and [Permissions](permissions) to enable fine-grained access control and authorization.

![Members page in Cribl.Cloud](cloud-members-ent-4.9.png)
{border="true"}

![Members page in an on-prem deployment](members-on-prem-4.10.png)
{border="true"}

## Availability

Member- and Permission-based access control is available on:

- On-prem Distributed deployments ([Stream](/stream/deploy-distributed), [Edge](/edge/setting-up-leader-and-edge-nodes)) with [certain plan/license tiers](https://cribl.io/pricing/).
- All Cribl.Cloud deployments, at the Organization level. (Cribl.Cloud with [certain plan/license tiers](https://cribl.io/pricing/) also offers granular access control on products, Worker Groups, and lower-level resources.)
 
> On on-prem Single-instance deployments, or Distributed deployments with other licenses, all users will have full `Admin`-level privileges.
{.box .info}

### Initial Permissions

When you first deploy Cribl Stream with the above prerequisites, you will be granted the [Organization-level](permissions#organization) `Admin` Permission. Using this Permission, you can then assign additional Permissions to yourself and other Members.

## Cross-Compatibility

Members and Permissions are available as the successors to Cribl's original role-based access control (RBAC) model
of [Local Users](local-users) and [Roles](roles)/[Policies](roles#policies). The earlier model is still supported across most of the Cribl product suite.

However, [Cribl.Cloud invitations](members#invite-members) and [Stream Projects](/stream/sharing-projects) have fully transitioned to the new model. For detailed differences between the two access control systems, see [Which Access Method Should I Use?](access-management#restrictions).

> ##### Known Issue
> 
> Existing Local Users display in **Settings** > **Global** > **Members and Teams** with the `No Access` Permission
> even if they've been assigned a higher Role.
> This is a display-only bug: These users' original Roles still function as configured.
> For details and fix progress, please see [Known Issues](known-issues#CRIBL-18973).
>
{.box .warning}

## Organizations

The Members and Permissions model uses the concept of Organization as a container for the deployment of a whole suite of Cribl products (Stream, Edge, and Cloud-only Search and Lake).

In Cribl.Cloud, each account has one Organization. Use [Workspaces](workspaces) for multi-tenancy within your Organization. Contact Cribl Support if you have a use case that requires more than one Organization.

To access the Organization-level Members as an Admin:

{{% tabs "cloud" "cloud" "Cribl.Cloud" "on-prem" "On-Prem Deployment" %}}
{{% tab-item "cloud" %}}

1. On the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, and then select **Members & Teams**.
1. Here, you can select an existing Member's row to change their access Permissions or other details, or select **Invite Member** to grant a new user access to your Cribl Organization.

{{% /tab-item %}}
{{% tab-item "on-prem" %}}

1. In the sidebar, select **Settings**, then **Global**.
1. Under **Access Management**, select **Members and Teams**.
1. Here, you can select an existing Member's row to change their access Permissions or other details, or select **Add Member** to grant a new user access to your Cribl Organization.

{{% /tab-item %}}
{{% /tabs %}}

### Invite Members

To invite new Members to your Organization:

{{% tabs "cloud" "cloud" "Cribl.Cloud" "on-prem" "On-Prem Deployment" %}}
{{% tab-item "cloud" %}}

1. On the top bar, select **Products**, and then select **Cribl**.
2. In the sidebar, select **Organization**, and then select **Members & Teams**.
3. On the **Members & Teams** submenu, select **Members**.
4. Select **Invite Member**.

![Invite Member modal](cloud-invite-4.10.png)
{border="true" scale="50%"}

5. Enter the **Email** of the new user you want to invite.
6. Optionally, enter the **First name** and **Last name** for the user.
7. Under **Permissions**, assign appropriate role [on your Organization](permissions#organization).
8. If you selected the **User** role, under **Workspace Access**, define access to specific [Workspaces](workspaces).

   > Assigning the **User** role requires certain plan/license tiers. For details, see [Pricing](https://cribl.io/pricing/).
   {.box .info}

9. If you selected the **Member** Permission for any Workspace, additionally select [product-level permissions](permissions#product) for that Workspace.
10. Select **Send Invite** to send the invitation.

{{% /tab-item %}}
{{% tab-item "on-prem" %}}

1. In the sidebar, select **Settings**, and then select **Global**.
2. Under **Access Management** submenu, select **Members and Teams**.
3. Select **Add Member**.

![New Member modal](onprem-invite-4.9.png)
{border="true"}

4. Enter the new Member's information, email, and password.
5. Under **Workspace permission**, select the permission you want to grant to the new Member for the whole Organization.
6. If you chose the **User** permission, configure [access for each Cribl product](permissions#product) separately.
   An **Admin** permission automatically grants admin access to all products.
7. When finished, select **Save**.

{{% /tab-item %}}
{{% /tabs %}}

### Responding to Invites

When you send an invite from a Cribl.Cloud deployment, the new Member receives an email with an **Accept Invitation** link to either sign into their existing Cribl.Cloud account, or sign up to create an account and its credentials.

After a Member signs in, they'll have access to your Organization and Cribl Stream instance at the Permission level you've specified.

## Manage Cribl.Cloud Invites

While an invite is pending, the **Members & Teams** page offers you these options in the **Action** column to deal with commonly encountered issues:

- ![Resend invite icon](send-invite-icon.svg) **Resend Invite**: If your invited Member didn't receive your invitation email, you can select this button to resend it.
- ![Copy invite icon](copy-invite-icon.svg) **Copy Invite Link**: If emails aren't getting through at all, select this button to copy and share a URL that will take the invitee directly to the signup page. This target page encapsulates the same identity, Organization, and Permission you specified in the original email invite.
- ![Revoke invite icon](delete-icon.svg) **Revoke Invite**: This is for scenarios where you need to revoke a pending invite. (You sent someone a duplicate invite, your invitee is spending too much time in space to be a productive collaborator, etc.) After selecting this button, you'll see a confirmation dialog.

An invite expires after seven days if it has been neither accepted nor revoked.
It must then be resent.

![Managing Invites](cloud-members-invites-cropped-4.9.png)
{border="true"}

## Remove Members

To remove a Member from your Organization:

{{% tabs "cloud" "cloud" "Cribl.Cloud" "on-prem" "On-Prem Deployment" %}}
{{% tab-item "cloud" %}}

1. In the sidebar, select **Organization**, and then select **Members & Teams**.
1. Select **Members**, and next to the Member you want to remove, select ![Delete icon](delete-icon.svg) **Remove Member** from the **Action** column.

Users will retain access to any other Cribl.Cloud Organizations they own or are Members of.

{{% /tab-item %}}
{{% tab-item "on-prem" %}}

1. In the sidebar, select **Settings**, and then select **Global**.
1. Under **Access Management** submenu, select **Members and Teams**.
1. Select **Members**, and next to the Member you want to remove, select ![Delete icon](delete-icon.svg) from the **Action** column.

{{% /tab-item %}}
{{% /tabs %}}

### Removing SSO Members

If your Cribl.Cloud Organization is using [SSO integration](cloud-sso),
the IDP (identity provider) admin manages Members in the IDP system.
When you remove such a Member from the IDP, they will still be left in the Members list in the UI.

After removing the Member from the IDP, you also need to manually delete them from the Members list
in the Cribl.Cloud Portal to completely remove them from the UI.

### Members and Local Users {#members-users}

In on-prem deployments, Members and Local Users are interchangeable:
Members can be reconfigured using [legacy Roles](roles) instead of Permissions,
and anyone you add to **Local Users** will also show in **Members**.
