# Teams


Teams are groups of [Members](members) who share the same Organization, Workspace, and product-level [Permissions](permissions).
This allows you to efficiently manage access control by assigning permissions to the Team rather than configuring each Member individually.

Teams are available only in Distributed deployments or in Cribl.Cloud.

## Create a Team

To create and configure a Team:

{{% tabs "cloud" "cloud" "Cribl.Cloud" "on-prem" "On-Prem Deployment" %}}
{{% tab-item "cloud" %}}

1. On the top bar, select **Products**, and then select **Cribl**.
1. In the sidebar, select **Organization**, then **Members & Teams**.
1. Go to the **Teams** tab and select **Create Team**.
1. Provide a meaningful **Name** and optionally a **Description**.
1. If you have SSO configured in your Organization, in **Mapping ID**, you can provide an external ID for the Team.
   Use it when integrating with SSO to map Workspace roles with [IDP groups](sso-initial-steps#linking-idp-groups-with-teams).
1. Select the desired Workspace and access to assign to the Team. **Member** allows granular [product-level](/stream/permissions#product) permissions.
1. In **Team Members**, select the Members you want to add, then select **Add**. A Member must already exist to be added to a Team.
1. The table is updated to show the Members you've just added.
1. When finished, select **Save**.

The main Teams page displays the Teams configured on your Organization.
You can edit them by selecting their row, or delete them as needed from the **Actions** column.

{{% /tab-item %}}
{{% tab-item "on-prem" %}}

1. In the sidebar, select **Settings**.
1. Select **Global**, then **Access Management**, then select **Members and Teams**.
1. Go to the **Teams** tab and select **Add Team**.
1. Provide a meaningful **Name** and optionally a **Description**.
1. In **Mapping ID**, you can provide an external ID for the team.
   Use it when integrating with SSO to [map roles](authentication#role-mapping) with IDP groups.
1. Select the desired Workspace- and product-level access access to assign to the Team. **User** allows granular [product-level](permissions#product) permissions.
1. In **Team Members**, select the Members you want to add, then select **Add**. A Member must already exist to be added to a Team.
1. The table is updated to show the Members you've just added.
1. When finished, select **Save**.

The main Teams page displays the Teams configured on your Organization.
You can edit them by selecting their row, and from there, delete them, if needed.

{{% /tab-item %}}
{{% /tabs %}}