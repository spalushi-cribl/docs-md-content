# Manage Members and Permissions


Invite Cribl.Cloud Members, configuring their Permissions at the Organization or Cribl Search level. Learn about the access privileges
provided with each Member Permission level.

---

## Manage Invites and Members

From the **Organization** > **Members & Teams** page, an Organization owner can invite new users to join the
Organization, assign access Permissions to new and existing Members, remove pending invites, and remove existing
Members.


![Members & Teams page: Managing Invites and Members](cloud-members-ent-4.9.png)

## Invite Members {#inviting-members}

To invite a new Member:

1. On the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, and then select **Members & Teams**.
1. On the **Members & Teams** submenu, select **Members**.
1. Select **Invite Member** to open the page shown below.
1. Enter the **Email** of the new user you want to invite.
1. Under **Permissions**, assign appropriate role [on your Organization](#member-roles).
1. If you selected the **User** role, under **Workspace Access**, define access to specific [Workspaces](workspaces).


   > Assigning the **User** role is available on certain plan/license tiers. For details, see [Pricing](https://cribl.io/pricing/).
   {.box .info}

1. If you selected the **Member** Permission for any Workspace, additionally select
   [product-level Permissions](/stream/members#permissions-product) for that Workspace.
1. Select **Send Invite** to send the invitation.


![Invite Member modal](cloud-invite-ent-4.9.png)

## Respond to Invites

An invited Member receives an email with an **Accept Invitation** link to either sign into their existing Cribl.Cloud
account, or else sign up to create an account and its credentials.

After signing in, they'll have access to your Organization and Cribl instance at the Permission level you've specified.

## Organization Member Permissions {#member-roles}

You assign Organization-level Permissions per individual user when you [invite them](#inviting-members) to your Organization as Members or by updating an existing user's assigned **Organization Permissions** after they join.

If you [configured SSO](/stream/sso-initial-steps/), grant Organization-level Permissions by managing group membership within your IdP. Make sure that your IdP group names align with the core patterns for [automatic Permission mapping](/stream/sso-initial-steps/#idp-group-name-mapping).

Cribl.Cloud does not support globally predefining or assigning group Permissions, as
on-prem deployments do.

| Permission                                                                               | User | IAM Admin | Admin | Owner |
| ---------------------------------------------------------------------------------------- | ---- | --------- | ----- | ----- |
| Log into the system                                                                      | ✓    |  ✓        | ✓     | ✓     |
| Update own Member profile                                                                | ✓    |  ✓        | ✓     | ✓     |
| View Stream Worker Groups to which you have access                                       | ✓    |  ✓        | ✓     | ✓     |
| `Admin` Access to all [Products](/stream/permissions#product/)                           |      |           | ✓     | ✓     |
| View and execute Leader commits                                                          |      |           | ✓     | ✓     |
| View and modify Global Settings                                                          |      |           | ✓     | ✓     |
| Manage ACLs (Access Control lists)                                                       |      |           | ✓     | ✓     |
| View and update [SSO](/stream/cloud-sso/) settings                                       |      | ✓         | ✓     | ✓     |
| View Data Sources and Trust Policies                                                     |      |           | ✓     | ✓     |
| Manage API Credentials                                                                   |      |           | ✓     | ✓     |
| View and manage Cribl Suite Global Settings                                              |      |           | ✓     | ✓     |
| View and manage (provision, update, and delete) Stream Worker Groups                     |      |           | ✓     | ✓     |
| View the credit consumption dashboards in the [FinOps Center](cloud-billing)             |      |           | ✓     | ✓     |
| [Download invoices](cloud-billing#download-invoices)                                     |      |           | ✓     | ✓     |
| View Organization [Details](cloud-portal#configure-organization-details)                 |      |           | ✓     | ✓     |
| Update Organization Details                                                              |      |           |       | ✓     |
| Delete an Organization                                                                   |      |           |       | ✓     |
| View Organization Members                                                                |      | ✓         | ✓     | ✓     |
| Manage (invite, update, delete) Organization Members                                     |      | ✓         | ✓     | ✓     |
| Create and delete [Lakehouses](/lake/lakehouse)                                          |      |           | ✓     | ✓     |
| Link and unlink [Lakehouses](/lake/lakehouse) with Datasets                              |      |           | ✓     | ✓     |

Each user can have Owner Permissions on multiple Organizations, and each Organization can have multiple users as Owners.
However, each user – as defined by their email address – can [register](getting-started-guide#sign-up-for-criblcloud)
only one active Organization, and only if they are not already the Owner of a different Organization.


> Cribl.Cloud does not use the earlier access-management model of Local Users and Roles/Policies that is still supported in on-prem Cribl Stream and Edge. Create and configure only **Members** on Cribl.Cloud applications (Cribl Search and Lake). References elsewhere in Cribl docs to Local Users do not apply to Cribl.Cloud.
>
{.box .info}

## Search Member Permissions {#search-member-roles}

You can assign the following Permissions at the Cribl Search product level.

| Permission                                                                                                                          | User          | Editor       | Admin      |
| ----------------------------------------------------------------------------------------------------------------------------------- | ------------- | ------------ | ---------- |
| **Data, Dashboards, and Libraries**                                                                                                 |               |              |            |
| Create, configure, view, and modify Datasets                                                                                        |               | ✓            | ✓          |
| Create, configure, view, and modify Dataset Providers                                                                               |               | ✓            | ✓          |
| Unhide Dataset Provider credentials                                                                                                 |               |              | ✓          |
| Create, configure, view, and modify Dashboards                                                                                      | ✓             | ✓            | ✓          |
| Create, configure, view, and modify [saved](search-overview#saved) and [scheduled](scheduled-searches) searches                     | ✓             | ✓            | ✓          |
| Create, configure, view, and modify settings like [Datatypes](datatypes) <br /> and [Event Breakers](datatypes#event-breaker-types) | ✓             | ✓            | ✓          |
| Create, configure, view, modify, and export to [lookups](lookups)                                                                   | ✓             | ✓            | ✓          |
| Create, configure, view, and modify other libraries <br /> ([Parsers](parsers), [Regexes](regexes), [Grok Patterns)](grok-patterns) | ✓             | ✓            | ✓          |
| Create, view, use, import, modify, and export [Packs](packs)                                                                        |               | ✓            | ✓          |
| **Access Control and Visibility**                                                                                                   |               |              |            |
| Invite other users (Members) to the Workspace                                                                                       |               | ✓            | ✓          |
| Manage other Members                                                                                                                |               |              | ✓          |
| Promote Members to Admins                                                                                                           |               |              | ✓          |
| See search [history](search-overview#history)                                                                                       | Own searches  | Own searches | ✓          |
| **Search Query Language**                                                                                                           |               |              |            |
| [`export`](export) operator                                                                                                         |               | ✓            | ✓          |
| [`send`](send) operator (see: [`send` Permissions](send#permissions))                                                               | Default Group | Named Groups | Custom URL |
| [`.cancel`](cancel) command                                                                                                         | Own searches  | Own searches | ✓          |
| [`.show queries`](show-queries) command                                                                                             | Own searches  | Own searches | ✓          |
| [`$vt_datasets`](vt_datasets) virtual table                                                                                         |               |              | ✓          |
| [`$vt_dataset_providers`](vt_dataset_providers) virtual table                                                                       |               |              | ✓          |
| [`$vt_jobs`](vt_jobs) virtual table                                                                                                 | Own searches  | Own searches | ✓          |
| [`$vt_lookups`](vt_lookups) virtual table                                                                                           | ✓             | ✓            | ✓          |
| [`$vt_results`](vt_results) virtual table                                                                                           | Own searches  | Own searches | ✓          |
| [`set`](set) statements                                                                                                             | Own searches  | Own searches | ✓          |
| [`.clear options`](clear-options) command                                                                                           | Own options   | Own options  | ✓          |

## Manage Invites

While an invite is pending, the **Members & Teams** page offers you these options in the **Action** column to deal with
commonly encountered issues:

- ![Resend invite icon](send-invite-icon.svg) **Resend Invite**: If your invited Member didn't receive your invitation
  email, you can click this button to resend it.
- ![Copy invite icon](copy-invite-icon.svg) **Copy Invite Link**: If emails aren't getting through at all, click this
  button to copy and share a URL that will take the invitee directly to the signup page. This target page encapsulates
  the same identity, Organization, and Permissions you specified in the original email invite.
- ![Revoke invite icon](delete-icon.svg) **Revoke Invite**: This is for scenarios where you need to revoke a pending
  invite. (You sent someone a duplicate invite, or your invitee is spending too much time in outer space to be a
  productive collaborator, etc.) After clicking this button, you'll see a confirmation dialog.

After seven days, if an invite has been neither accepted nor revoked, it expires.


![Managing Invites](cloud-members-invites-cropped-4.9.png)

## Remove Members

To remove a Member from your Organization:

1. In the sidebar, select **Organization**, and then select **Members & Teams**.
1. Select **Members**, and next to the Member you want to remove, select ![Delete icon](delete-icon.svg) **Remove
   Member** from the **Action** column.

Users will retain access to any other Cribl.Cloud Organizations on which they are Owners or Members.

### Remove SSO Members

If your Organization is using [SSO integration](/stream/cloud-sso), the IDP (identity provider) admin manages Members in
the IDP system. When you remove such a Member from the IDP, they will still be left in the Members list in the UI.

After removing the Member from the IDP, you also need to manually delete them from the Members list in the Cribl.Cloud
Portal to completely remove them from the UI.
