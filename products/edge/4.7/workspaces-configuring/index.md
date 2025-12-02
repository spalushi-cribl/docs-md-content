# Configuring Workspaces


## Add New Workspace

> Before you try creating a new Workspace, make sure you have [opted in](workspaces#opt-in-to-workspaces)
> to the new Cribl.Cloud UI.
{.box .info}

To create a new Workspace:

1. From the top navigation bar, click your current Workspace name. This opens a modal with a list of all Workspaces you have access to.
1. Select **Add Workspace**.
1. Fill the form with the following information:
   
   | Field | Description | Notes |
   | --- | --- | --- |
   | Workspace Name | Human-readable name for the Workspace. ||
   | Workspace ID | Internal ID for the Workspace. This ID will be part of the Workspace's Cribl.Cloud URL. | **Cannot be changed** once the Workspace is created. |
   | Description | Additional Workspace description. | Optional |
   | Region | Set automatically to correspond to the Organization region. ||
   | Tags | Up to 3 tags you can use to filter Workspaces. | Optional |

![Modal with form for creating Workspaces, including Workspace Name, ID, Description, Region, and Tags](create-new-workspace-4.6.png)
{border="true" caption="Creating a new Workspace"}

4. Confirm with **Save**.

The Workspace can take up to several minutes to spin up.
While the process continues, you can navigate away and keep working with your Cribl.Cloud instance as usual.

## Edit or Delete a Workspace

You can edit or delete your Workspace at any time by clicking the name of the Workspace in the top navigation bar, and then clicking **Settings** next to the Workspace.
You can change the name, description, and tags of a Workspace.
However, you can't change the Workspace ID after it has been created.

> Only Members with the Organization Owner permission can delete Workspaces.
{.box .info}
