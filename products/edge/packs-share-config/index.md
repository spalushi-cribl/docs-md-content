# Share Packs


Packs allow you to share your configurations,
including Sources, Destinations, Routes, Pipelines, and Knowledge Objects,
between Fleets and deployments.

If you are developing your resources with the intent of sharing them,
you can start building them up inside a Pack.

However, you can also copy or move your existing setup into a Pack for the purpose of sharing.

## Plan Your Pack

Before you start creating a Pack, first consider your setup and identify which resources you need to include in it.

Identify elements (Sources, Destinations, Pipelines, Routes, Event Breakers, and Data Samples) you want to share in the Pack.

## Create a Pack

1. Navigate to the **Packs** page and select **Add Pack**.
2. From the submenu, select **Create Pack**.
3. Enter a unique **Pack ID** and other details:

   - Each Pack within a Fleet must have a separate **Pack ID**, but you can assign arbitrary **Display names**.
   - **Version** is a required field identifying the Pack's own versioning.
   - **Minimum Edge version** is an optional field specifying the lowest compatible version of Cribl Edge software.
     If you intend to include Sources or Destinations in your Pack, set the minimum version to 4.12.0.
   - **Description** and **Author** are optional identifiers.
   - **Data type**, **Use cases**, and **Technologies** are optional combo boxes. You can insert one or multiple keywords to help users filter Packs that you post publicly on the Cribl Packs Dispensary.
   - **Tags** are optional, arbitrary labels that you can use to filter/search and organize Packs.

4. Select **Save**.

![Creating a Pack](create-pack-4.12.png)
{scale="50%" border="true"}

## Copy Resources

Now, go through all the resources you've listed, copying each of them to the new Pack.

### Copy Pipelines

To copy a Pipeline into a Pack:

1. In the sidebar, select **Fleets**, then select a Fleet.
1. On the **Fleets** submenu, select **More**, then **Pipelines**.
1. Locate the Pipeline you want to copy and open the Options ••• menu on its row
1. Select **Copy**.
1. Navigate to your new Pack and, in the Pack submenu, select **Pipelines**.
1. Select **Paste Pipeline** ![Paste button](paste-button.light.svg) on the filter bar, confirm the Pipeline name and select **Save**.

### Copy Routes

To copy a Route into a Pack:

1. In the sidebar, select **Fleets**, then select a Fleet.
1. On the **Fleets** submenu, select **More**, then **Routes**.
1. Locate the Route you want to copy and open the Options ••• menu on its row
1. Select **Copy**.
1. Navigate to your new Pack and, in the Pack submenu, select **Routes**.
1. Select **Paste route** ![Paste button](paste-button.light.svg) on the filter bar.
1. Reorder the Route according to your needs.

### Copy Sources

> Packs can't contain Collectors and Collector-based Sources,
> as well as K8s Metrics, K8s Logs, K8s Events, and Cribl Internal Sources in Cribl Edge.
{.box .info}

To copy a Source into a Pack:

1. In the sidebar, select **Fleets**, then select a Fleet.
1. On the **Fleets** submenu, select **More**, then **Sources**.
1. Locate the Source you want to copy and open it.
1. Select **Manage as JSON** on the **Configure** tab.
1. In the **Manage as JSON** modal, select **Export** and save the exported file.
1. Navigate to your new Pack and, in the Pack submenu, select **Sources**.
1. Choose the type of Source you are copying, and select **Add Source**.
1. Select **Manage as JSON**, select **Import**, and choose the exported Source file.
1. Confirm with **OK**, then save the new Source.

> If you are copying an enabled Source bound to a port,
> you must temporarily disable that Source before creating the copy.
> Once you've finished configuring the new Source and selected a new port,
> you can re-enable the original.
{.box .info}

### Copy Destinations

To copy a Destination into a Pack:

1. In the sidebar, select **Fleets**, then select a Fleet.
1. On the **Fleets** submenu, select **More**, then **Destinations**.
1. Locate the Destination you want to copy and open it.
1. Select **Manage as JSON** on the **Configure** tab.
1. In the **Manage as JSON** modal, select **Export** and save the exported file.
1. Navigate to your new Pack and, in the Pack submenu, select **Sources**.
1. Choose the type of Destination you are copying, and select **Add Destination**.
1. Select **Manage as JSON**, select **Import**, and choose the exported Destination file.
1. Confirm with **OK**, then save the new Destination.

> You can use the same method with **Manage as JSON** to copy
> Knowledge Objects such as variables, Regexes, or Event Breaker Rulesets.
{.box .info}

## Configure Variables

To facilitate other users integrating your Pack into their deployments,
you can replace values in certain fields in your configuration with variables.

A user who imports your Pack will then be prompted to provide their own values for all the variables you define.

1. Select ![Add Variable](variable.light.svg) next to the field you want to configure a variable for.
1. In the **Variables** modal, either choose an existing variable or select **Add Variable**.
1. If you are adding a new variable, in the **New Variable** modal, enter the name for the variable and its value for your current deployment, then save.
1. Confirm the use of the variable for the field by selecting **Add**.

> Secrets such as passwords are removed from the variables on export.
> They will be replaced with a `changeme` value, prompting users to provide the value for their own deployment.
{.box .info}

## Export the Pack

Now that your Pack is ready, you can export it for sharing.

Storing your Pack in a Git repository lets you version it and collaborate on its development.
You can use a private repository in any common Git hosting service.
This guide uses GitHub as an example.

1. Create an empty private repository.
   To do this in GitHub, select **Create New** > **New Repository** on the top bar.
   Give your repository a name, an optional description, and set it to **Private**.
1. Clone the new repository locally to your system.
1. In Cribl Edge, navigate to the **Packs** page and open the Options ••• menu on the row of your Pack.
1. Select **Export** and choose the **Merge** export mode.
1. Confirm with **Export** and save the file locally.
1. Extract the `.crbl` file into the repository directory.

   > Because they are gzipped, you can extract `.crbl` files using the `tar` utility. For example: `tar -xvzf WebdataStorage_0.0.1.crbl`.
   >
   {.box .info}

1. Add, commit, and push the Pack content to your remote repository.

At this point you can share the repository with other users.
They will be able to import the Pack by selecting **Add Pack** > **Import from Git**.

To learn more about collaborating on Packs based on a Git repository,
see [Collaborate on Developing Packs](packs-collaborate).