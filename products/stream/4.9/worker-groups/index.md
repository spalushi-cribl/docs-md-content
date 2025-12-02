# Manage Worker Groups


Worker Groups are collections of Worker Nodes that share the same configuration.
They facilitate authoring and management of configuration settings.

You configure data flow, including [Routes](routes), [Pipelines](pipelines),
[Sources](sources), [Destinations](destinations), per Worker Group.
Each Worker Group also has separate [version control](version-control).

> A Cribl.Cloud deployment with a [certain plan tier](https://cribl.io/pricing/) also allows you to create Cribl-managed Worker Groups in Cribl.Cloud.
> See [Managing Cribl.Cloud Worker Groups](cloud-workers) for more details.
{.box .cloud}

## Create a Worker Group

To create a new Worker Group:

1. In the sidebar, select **Worker Groups**.
1. Select **Add Group**.
1. If you are on Cribl.Cloud and see the **Worker Group type** field, select **Hybrid**.
1. Enter a unique **Group name**.
1. Optionally, you can **Enable teleporting to Workers** to allow [UI access](setting-up-leader-and-worker-nodes#worker-access) for individual Worker Nodes.
1. Optionally, type in a **Description** and select [**Tags**](settings-group-fleet#workergroup-configuration).

![Creating a New Group](new-group-4.8.png)
{scale="60%" border="true"}

> To use the [Cribl API](/api/) to create a new Worker Group, see the [API Reference](/api-reference/).
>
> Configuring multiple Worker Groups, or configuring more than 10 Worker Processes, requires a Cribl Stream on-prem deployment with a [certain license tier](https://cribl.io/pricing/).
{.box .success}

> To explicitly set passwords for Worker Groups, see [User Authentication](authentication#worker_pwd).
>
{.box .warning}

## Clone a Worker Group

Cloning a Worker Group copies everything configured on the original Group: Sources, Pipelines, Packs, Routes, and Destinations.

To clone a Worker Group:

1. In the sidebar, select **Worker Groups**.
1. In the **Actions** column, select ••• (Options).
1. Select **Clone**.
1. Configure the cloned group by choosing its name, and optionally description and tags.

## Recover a Worker Group

If you delete a Worker Group, you can no longer access its **Version Control** menu.
As a result, you also lose the option to directly recover the Worker Group. 

You can, however, still recover a deleted Worker Group, using one of the following two methods:

- [Create a new Worker Group with the same name](#newgroup).
- [Revert commit via command line](#revert-via-cli).

### Create a New Worker Group with the Same Name {#newgroup}

When you create a new Worker Group with the same name as a deleted one,
it will restore that group with all of its configuration.

1. In the sidebar, select **Worker Groups**, then select **Add Group**.
1. Set **Group name** to be exactly the same as the deleted group. Fill in the remaining information and **Save**.
1. If you previously committed the deletion of the original Worker Group, open the **Version Control** menu.
1. Select a previous version of the deleted Worker Group and confirm with **Done**.

### Revert Commit via Command Line {#revert-via-cli}

You can restore a deleted Worker Group by reverting the deletion commit in version control via command line.

1. Stop the Cribl server.
1. Using command line, look at the version control history for the Leader Node
   and identify the commit which deleted the Worker Group.
1. Revert to the last commit before the Group was deleted.
1. Restart the Cribl server.
