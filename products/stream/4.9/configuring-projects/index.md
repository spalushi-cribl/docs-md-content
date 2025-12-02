# Configure Projects


Projects define the data that the Project's users can consume, where that data can be sent, and who has access to the Project. A Project associates specific [Subscriptions](configuring-subscriptions) with specific [Destinations](destinations). For fine-grained access control, you can share each Project with specific Members and Teams. 

By connecting incoming data sources (Subscriptions) to specific Destinations, Projects function like a Route's [Output selection](routes#output-destination). We expand on this similarity in [Subscriptions and Routes](configuring-subscriptions#subscriptions-and-routes).

## Access Projects {#project-view}

From your Worker Group's **Manage** submenu, select **Projects** to open the default **Data Projects** page. 

This is the "Project view," an expanded version of the UI in which Project editors will manage connections among the resources you define here. From left to right, the options in this administrator's view are:
- Projects drop-down list: To bring a different Project into the Project view, select it here.
- **View all** link: Select to open the **All Projects** modal, where you can configure and share existing Projects.
- **Pipelines** link: Select to open a modal where you can manage Pipelines that are scoped to this Project.
- **Packs** link: Works the same way as the **Pipelines** link. The modal will show only Packs that are part of this Project. For Project editors, Packs will be read-only resources – Project editors can insert or remove Packs, but not modify them.
- **Members and Teams** link: Select to open a drawer where you can grant Members and/or Teams access to the Project.
- **Commit Project** button: Commit changes from this Project to Git.
- **Add Project** button: Creates a new Project. Covered just below.

![Accessing the Project view](data-projects-tab-4.7.png)
{border="true"}

## Configure a New Project {#configure-new}

1. From the Project view shown above, select **Add Project** to open the **Add New Project** modal shown below.

![Configuring a new Project](project-config.9230b2bbd6.png)

2. Assign this Project a unique **Project ID**. (This can include letters, numbers, `_`, and `-`. IDs are case-sensitive, so entering the same characters with different capitalization will create a separate ID.)
3. Select one or more configured **[Subscriptions](configuring-subscriptions)**. (You can access only Subscriptions that are already configured in this Project. But you can reopen this modal later to add newly configured Subscriptions. Also, the Subscriptions UI enables you to add Subscriptions to Projects.)
4. Select one or more configured **[Destinations](destinations)** that will have access to this Project's data. (You can access only Destinations that are already configured in this Project – but you can configure and add more Destinations in the next section.)
5. Optionally, add a **Description** of the Project's purpose.
6. Select **Save** to finish configuring your Project.
7. Commit and deploy your changes.

> To view **Live Data** on your configured **Destination**, you must **Commit** and **Deploy** your changes.
>
{.box .warning}

## Test a Project (Manage Data Flow) {#test}

After you return to the Project view, make sure your desired Project has focus in the upper-left drop-down list. You can now use the Project view's visual UI to expand the Project and enable its data flow. The options here include:
- Connect the Project's already configured Subscriptions and Destinations, optionally adding a [Pipeline or Pack](#process) to each connection.
- Select **Subscriptions** > **Add / Remove** to open a drawer where you can expose any Subscription you've already configured in the Project, remove Subscriptions from the Project view, or add new Subscriptions. 

![](subscriptions-mgmt-modal.06e63c5840.png)
{border="true"}
- Select **Destinations** > **Add / Remove** to open a drawer where you can expose and configure any Destination type available in your Cribl Stream deployment. (Any Destination that you add here will automatically be added to the Project's configuration modal when you next [reopen it](#editing).)

![](project-destinations.307a4f2ac3.png)
{scale="60%" border="true"}

> As you configure and test your Project, beware of high ingestion rates that could overwhelm downstream receivers. To prevent dropped events, enable [Persistent Queues](persistent-queues) on Destinations that support them.
>
{.box .warning}

## Enable Pipelines and Packs {#process}

Each Project that you configure opens with a basic set of Pipelines and one default Pack. To manage these, select the upper **Pipelines** or **Packs** link, respectively. Each opens a modal similar to Cribl Stream's standard [Pipelines](pipelines) or [Packs](#packs) UI, with access to standard Sample Data and Preview options on the right.

![Managing Project Pipelines](project-pipelines-modal.859b4f90d3.png)
{border="true"}

To offer Project editors additional processing resources to insert on Subscription/Destination connections, select the modal's **Add Pipeline** or **Add Pack** button. You can then enable new Pipelines and Packs in the Project, with these constraints:
- You can create a new Pipeline or Pack here. It will be available only in the current Project.
- You can import an existing Pipeline or Pack. Any modifications that you make here will similarly be scoped to the current Project.
- To import an existing Pipeline from your Cribl Stream deployment, you must [export it as JSON](pipelines#json), then select the **Import from Pipeline** drop-down option shown above. (You can also **Import from URL**.)
- You can import Packs from an [exported JSON file](packs#export), a URL, or the Dispensary or Git options shown on the drop-down below.

![Adding Project Packs](project-packs-modal.dadc9742ce.png)
{border="true"}

## Edit a Project {#editing}

To update an existing Project, select the Project view's upper-left **View all** link. From the resulting **All Projects** modal, select the Project's row to reopen its configuration in a modal similar to the **Add New Project** modal covered [above](#configure-new).

You can also select any row's **Share** link to open a drawer in which to [manage users' access](/stream/sharing-projects) to the Project.

![View all > All Projects modal](projects-all-modal.682afeecba.png)
{border="true"}

## Access Project Metrics {#metrics}

You can access the following metrics on your Projects:
- Events and bytes in/out of Subscriptions.
- Bytes in/out of Pipelines, Packs, and Destinations.
- Events in/out/dropped through Pipelines, Packs, and Destinations;

To tap these metrics, enable Cribl Stream's internal [CriblMetrics Source](sources-cribl-internal#metrics). This Source's defaults are different for Cribl.Cloud versus on-prem deployments.

### Cribl.Cloud Leaders and Groups

For [Cribl.Cloud](deploy-cloud) Leaders, and Worker Groups of Cribl-managed Workers in Cribl.Cloud, Projects' metrics are always available to the CriblMetrics Source. This availability is not configurable.

### On-Prem Leaders and Groups

For on-prem Leaders, and customer-managed (hybrid or on-prem) Worker Groups, Projects' metrics are disabled by default. (As with other on-prem exclusions, we provide this default to optimize performance.) However, you can easily make these metrics available to the CriblMetrics Source.

See [Controlling Metrics](internal-metrics#controlling-metrics) for general instructions on modifying defaults at the Leader, Worker Group, or single-instance level. From the **Disable field metrics** list, remove the `project` metric. Then save, commit, and deploy your changes.
