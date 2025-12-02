# Configuring Subscriptions


You use a Subscription's **Filter** expression to specify a subset of a Worker Group's overall incoming data stream to forward to Destinations. Optionally, you can also specify a Pre‑processing [Pipeline](pipelines) to condition that data before it leaves the Subscription.

You gather Subscriptions into [Projects](configuring-projects), where you match them up with [Destinations](destinations) and share them with authorized users.

A Project can contain multiple Subscriptions. In Cribl Stream 4.2 and later, each Subscription can be associated with multiple Projects. Each Project's copy of the Subscription will share the same configuration, but will have independent data flow.

## Subscriptions and Routes

From the UI similarity, you've probably noticed that Subscriptions look and function much like [Routes](routes). There are both similarities and differences – which we'll consider in reverse order.

### Subscriptions/Routes Differences

Compared to Routes, Subscriptions present these exceptions: 
- Subscriptions lack a Route's **Output** or **Output Expression** field. (Instead, you associate Subscriptions with Projects, where Project editors can process the data and send it to Destinations.
- Subscriptions have no **Final** flag. So a Subscription is similar to a Route with the **Final** toggle switched off.
- Subscriptions do not relay backpressure upstream to data senders. (This is by design, to prevent any Subscription from affecting data flow through parallel Subscriptions, Projects, or Routes/Pipelines.) If any backpressure occurs, a Subscription will drop data.

### Subscriptions/Routes Similarities


A further link to Cribl Stream's Data Routes interface is that Subscriptions can ingest data **only** from Sources configured (at the Cribl Stream product level) as **Send to Routes**. (Subscriptions and Projects cannot ingest data from [QuickConnect](quickconnect) Sources, despite the similarity of Projects' visual UI (the Project view).

This also means that Subscriptions tap exactly the same overall flow of inbound data that Data Routes do, in parallel. So there are cases where, after you've configured Subscriptions to ingest certain data, you might want to configure a Pipeline at the top of the Routing table to drop the same data. This will prevent Cribl Stream from processing the same events twice.

## Usage Tips

Compared to sending the same data flow through Routes, adding Subscriptions can have a performance impact, because each Subscription creates a copy of the data that Cribl Stream needs to process. Therefore, your resource requirements could change with the number of Subscriptions you add. 

Please work with your Cribl Solutions Engineer to select an architecture that best balances your needs for performance versus segmented data/configuration access.


Cribl recommends that you configure a separate Subscription – and corresponding Pipeline(s) – for each data type you ingest. This enables you to filter by data type in the Subscription's definition, and again in individual Pipeline Functions.

## Accessing Subscriptions {#subs-tab}

1. From your Worker Group's **Manage** submenu, select **Projects**.
2. Select the **Subscriptions** lower tab.

    Any Subscriptions you've already configured will appear here. Expand the accordions to access each Subscription's configuration.



![Configuring a new Subscription](project-subs-config.022980e370.png)
{border="true"}

## Configuring a New Subscription {#configure-new}

1. From the **Subscriptions** lower tab, click **Add Subscription** to open the configuration options described below.
2. Assign this Subscription a unique **Subscription ID**. (This can include letters, numbers, `_`, and `-`. IDs are case-sensitive, so entering the same characters with different capitalization will create a separate ID.)
3. Modify the **Filter** by replacing the default `true` with a JavaScript expression that selects a subset of events to filter through this Subscription. (Your expression can specify an upstream sender, a data type, the presence of particular fields, etc. For examples, see [Cribl Expression Syntax](introduction-reference#filters).)
4. Modify (as needed) the Source's default `passthru` [Pre‑processing Pipeline](pipelines#input-conditioning-pipelines).
5. Optionally, add a **Description** of the Subscription's purpose.
6. Optionally, assign the Subscription to one or more Projects. (You can also import Subscriptions from individual [Projects' configurations](configuring-projects#configure-new), and can control Subscriptions' visibility in the [Project view](configuring-projects#configure-new).)
7. Click **Save** to save the new Subscription to the Subscriptions table.
8. Commit and deploy your changes.

    > A Subscription must specify a Pre‑processing Pipeline, even if this is the default `passthtru`, which performs no processing.
    {.box .warning}

## Adding and Previewing More Subscriptions {#configure-more}

From the **Subscriptions** tab, you can click **Add Subscription** repeatedly to define multiple Subscriptions. These stack on the left, like Routes in Cribl Stream's [Routing table](routes).

![Stacking Subscriptions](projects-subs-2nd-config.3bb44e25a2.png)
{border="true"}

Note these further similarities to the Routing table:
- Each Subscription's blue title bar has a slider that you can use to disable the Subscription from gathering data, while preserving its configuration.
- On the right, you can capture, import, and save sample data, and. You can use these samples to preview how your Subscriptions transform incoming data.


## Editing a Subscription {#editing}

To update an existing Subscription, just click its accordion in the Subscriptions table. This will reopen the same configuration options outlined [above](#configure-new).

![Subscriptions table](subscriptions-submenu-filled.7c9004a1ed.png)
{border="true"}
