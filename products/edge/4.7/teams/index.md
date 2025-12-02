# Teams


Teams are groups of Members who share the same Organization, Workspace, and product-level permissions.
This allows you to efficiently manage access control by assigning permissions to the Team rather than configuring each Member individually.

Teams are available only in distributed deployments or in Cribl.Cloud.

## Create a Team

To create and configure a Team:

{{% tabs "cloud" "cloud" "Cribl.Cloud" "on-prem" "On-Prem Deployment" %}}
{{% tab-item "cloud" %}}

1. Ensure you have [opted-in](workspaces#opt-in-to-workspaces) to the new Cribl.Cloud UI.
2. In the global navigation pane, expand **Organization** and then select **Teams**.
3. Select **Create Team** and the configuration modal is displayed.
4. Provide a meaningful **Name** and optionally a **Description**.
5. Select the desired Workspace and access to assign to the Team. **Member** allows granular [product-level](/stream/members#permissions-product) permissions. 
6. In **Team Members**, select the Members you want to add, then **Add** them. A Member must already exist to be added to a Team.
7. The table is updated to show the Members you've just added.
8. When finished, **Save** your new Team.

The main Teams page displays the Teams configured on your Workspace. You can **Edit** or delete them as needed from the **Actions** column.

{{% /tab-item %}}
{{% tab-item "on-prem" %}}

1. In the Cribl Organization's top bar, select **Settings**.
1. In **Global Settings**, under **Access Management**, select **Members and Teams**.
1. Go to the **Teams** tab and select **Add Team**.
1. Provide a meaningful **Name** and optionally a **Description**.
1. In **Mapping ID**, you can provide an external ID for the team to be used in [role mapping](authentication#role-mapping).
1. Select the desired Organization- and product-level access access to assign to the Team. **User** allows granular [product-level](members#permissions-product) permissions.
1. In **Team Members**, select the Members you want to add, then **Add** them. A Member must already exist to be added to a Team.
1. The table is updated to show the Members you've just added.
1. When finished, **Save** your new Team.

The main Teams page displays the Teams configured on your Organization. You can edit them by clicking their row,
or delete them as needed from the **Actions** column.

{{% /tab-item %}}
{{% /tabs %}}