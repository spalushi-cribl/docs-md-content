# Configuring Projects (Beta)


Projects define the data that the Project's users can consume, and where that data can be written to. A Project associates specific Subscriptions with specific [Destinations](destinations). 

In this way, Projects function like a Route's [Output selection](routes#output-destination). They also provide deliberately high-level access control, encapsulating the inputs and outputs available to each Project's users.

> In the current release, users with the `project_user` Role have access to all configured Projects.
>
{.box .info}

## Configuring a New Project {#configure-new}

1. From your Worker Group's **Manage** submenu, select **Projects**.
2. Select the **Data Projects** lower tab.
3. Click **Add Project** to open the **New Project** modal shown below.

![Configuring a new Project](project-config.0c1e75c2f7.png)

4. Assign this Project a unique **Project ID**. (This can include letters, numbers, `_`, and `-`. IDs are case-sensitive, so entering the same characters with different capitalization will create a separate ID.)
5. Optionally, add a **Description** of the Project's purpose.
6. Select one or more configured Subscriptions. (You can access only Subscriptions that are already configured.)
7. Select one or more configured [Destinations](destinations) that will have access to this Project's data. (You can access only Destinations that are already configured as QuickConnect Destinations.)
8. Click **Save** to finish configuring your Project.
9. Commit and deploy your changes.

### Testing in QuickConnect {#qc}

In the Projects table, note each Project's **View in QuickConnect** link. Click this to connect a Project's selected configured Subscriptions and Destinations, and to test your Project – all in the Project user's graphical view.

> As you configure and test your Project, beware of high ingestion rates that could overwhelm downstream receivers. To prevent dropped events, enable Persistent Queues on Destinations that support them.
>
{.box .warning}

To return to your admin's configuration view, click the **Projects** submenu's **Subscriptions** or **Data Projects** tab.

## Editing a Project {#editing}

To update an existing Project, click it in the Projects table. This will open its configuration in a modal similar to the **New Project** modal covered above.
