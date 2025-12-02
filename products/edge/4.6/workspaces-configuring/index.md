# Configuring Workspaces


## Add New Workspace

> Before you try creating a new Workspace, make sure you have [opted in](workspaces#opt-in-to-workspaces)
> to the new Cribl.Cloud UI.
{.box .info}

To create a new Workspace:

1. From the top bar of your Organization's front page, click your current Workspace name. This opens a modal with a list of all Workspaces you have access to.
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

1. Confirm with **Save**.

The Workspace can take up to several minutes to spin up.
While the process continues, you can navigate away and keep working with your Cribl.Cloud instance as usual.

## Edit a Workspace

You can edit your Workspace at any time, by clicking the gear (⚙️) button in the top right corner.
You can change the name, description, and tags of a Workspace.
However, you can't change its identifier once it has been created.

## Delete a Workspace

To delete a Workspace:

1. Click the gear (⚙️) button in the top right corner of the Workspace's screen.
1. Select **Delete Workspace**.

> Only Members with the Organization Owner permission can delete Workspaces.
{.box .info}
