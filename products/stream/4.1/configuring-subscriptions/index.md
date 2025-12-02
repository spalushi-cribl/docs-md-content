# Configuring Subscriptions (Beta)


You use a Subscription's **Filter** expression to specify a subset of a Worker Group's overall incoming data stream to forward to Destinations. Optionally, you can also specify a Pre‑processing [Pipeline](pipelines) to condition that data before it leaves the Subscription.

You gather Subscriptions into [Projects](configuring-projects), where you match them up with [Destinations](destinations).

Although a Project can contain multiple Subscriptions, each Subscription can be associated with only one Project. (This is analogous to how each Cribl Stream Route is matched with a single Destination.)

## Subscriptions and Routes

In general, Subscriptions look and function much like [Routes](routes), with these exceptions: 
- Subscriptions lack a Route's **Output** or **Output Expression** field. (Instead, you associate Subscriptions with their Destinations using Projects, and link them up in the visual UI.)
- Subscriptions have no **Final** flag.

## Usage Tips

Compared to sending the same data flow through Routes, adding Subscriptions can have a performance impact, because each Subscription creates a copy of the data that Cribl Stream needs to process. Therefore, your resource requirements could change based on the number of Subscriptions you add. 

Please work with your Cribl Solutions Engineer to select an architecture that best balances your needs for performance versus data access segmentation.


Cribl recommends that you configure a separate Subscription – and corresponding Pipeline(s) – for each data type you ingest. This enables you to filter by data type in the Subscription's definition, and again in individual Pipeline Functions.

## Configuring a New Subscription {#configure-new}

1. From your Worker Group's **Manage** submenu, select **Projects**.
2. Select the **Subscriptions** lower tab.
3. Click **Add Subscription** to open the configuration options shown below.

![Configuring a new Subscription](project-subs-config.dae30fb843.png)
{border="true"}

4. Assign this Subscription a unique **Subscription ID**. (This can include letters, numbers, `_`, and `-`. IDs are case-sensitive, so entering the same characters with different capitalization will create a separate ID.)
5. Modify the **Filter** by replacing the default `true` with a JavaScript expression that selects a subset of events to filter through this Subscription.  
(You can specify an upstream sender, a data type, the presence of particular fields, etc.)
6. Modify (as needed) the Source's default `passthru` [Pre‑processing Pipeline](pipelines#input-conditioning-pipelines).
7. Optionally, add a **Description** of the Subscription's purpose.
8. Click **Save** to save the Subscriptions table.
9. Commit and deploy your changes.

    > A Subscription must specify a Pre‑processing Pipeline, even if this is the default `passthtru`, which performs no processing.
    {.box .warning}

## Adding and Previewing Subscriptions {#configure-more}

From the **Subscriptions** tab, you can click **Add Subscription** repeatedly to define multiple Subscriptions. These stack on the left, like Routes in Cribl Stream's [Routing table](routes).

![Stacking Subscriptions](projects-subs-2nd-config.3bb44e25a2.png)
{border="true"}

Note these further similarities to the Routing table:
- Each Subscription's blue title bar has a slider that you can use to disable the Subscription from gathering data, while preserving its configuration.
- On the right, you can capture, import, and save sample data, and. You can use these samples to preview how your Subscriptions transform incoming data.



> A further link to Cribl Stream's Data Routes interface is that Subscriptions can ingest data **only** from Sources configured as **Send to Routes**. Subscriptions (and Projects) cannot ingest data from [QuickConnect](quickconnect) Sources, despite the similarity of Projects' visual UI (the Project view).
>
{.box .info}

## Editing a Subscription {#editing}

To update an existing Subscription, just click its accordion in the Subscriptions table. This will reopen the same configuration options outlined [above](#configure-new).

![Subscriptions table](subscriptions-submenu-filled.4d6b6507e0.png)
{border="true"}
