# CrowdStream


CrowdStream is a special [Cloud-hosted](deploy-cloud) version of Cribl Stream, available through [CrowdStrike Falcon LogScale](https://www.crowdstrike.com/products/observability/falcon-logscale/). This collaboration is tailored to forward data from a wide range of sources into LogScale for [XDR](https://www.crowdstrike.com/cybersecurity-101/what-is-xdr/) analysis and log management.

En route, you can process events using all of Cribl Stream's options for aggregating, sampling, redacting, cloning, and otherwise shaping and directing your data. CrowdStream provides parallel options to route data to cost-effective Amazon S3 storage, and to S3 data lakes, for later replay and analysis.

## Getting Started

These steps assume that you've set up your CrowdStrike Falcon LogScale account, obtained your corresponding [API token](https://library.humio.com/humio-server/parsers-assigning-to-ingest-tokens.html).



You can get access to CrowdStream in two ways:

- Through Falcon Next-Gen SIEM interface
- Through Falcon LogScale interface

To log into CrowdStream from Next-Gen SIEM interface:

1. In the left menu, select **Next-Gen SIEM**
2. Then, under **Log management** select **CrowdStream**.

![Navigating from Next-Gen SIEM to CrowdStream](crowdstream-loginvia-ngsiem.png)
{scale="40%" border="true"}

To log into CrowdStream from LogScale interface:

1. Log into LogScale.
2. On the **CrowdStream** tile at right, select **Try now**.

![Navigating from LogScale to CrowdStream](crowdstream-tile-4.9.png)
{border="true" scale="90%"}

3. On the resulting page, select **Log in**.

Regardless of your login method, you should now see the CrowdStream home page.

4. Select **Manage** on the **CrowdStream** tile.

![Selecting the CrowdStream app](crowdstream-main-page-4.9.png)
{border="true" scale="90%"}

5. On CrowdStream's home page, under **Worker Groups**, select the **default** Group.

   (A Worker Group is a collection of processing instances that share the same configuration. For details, see Cribl Stream's [Distributed Deployment](deploy-distributed) topic.)

   ![Selecting the **default** Group](crowdstream-select-group-4.9.png)
   {border="true" scale="90%"}

   > If you see a **Worker Group is not provisioned** warning, select the **Provision Now** button to create Workers before proceeding.
   {.box .warning}

6. To configure this Group, you can choose between the **QuickConnect** or **Routes** tiles. How to choose? See the next section.

![Group **Overview** page](crowdstream-group-overview-4.9.png)
{border="true"}

> ##### For More Information
>
> - [Cribl.Cloud Launch Guide](deploy-cloud)
>
{.box .success}

## Simple or Complex Routing? {#routing}

Your first choice is how simply or intricately you want to configure data flow from the first data Source you set up. CrowdStream offers two user interfaces:

- [QuickConnect](quickconnect) (the left tile shown above) provides a simple visual UI. You drag and drop connections from Sources on the left to Destinations on the right. Each connection can include as much or as little intermediate processing as you like. To remove a connection, just drag it off the Destination (you'll get a confirmation modal). The only practical limitation here is that data flow through every Source ‑> Destination connection must be parallel and independent.

- [Data Routing](routes) (the right tile shown above) provides fine-grained control, via a more traditional UI that you configure across separate modals and pages. You build a Routing table with conditional processing. This can cascade and clone data across multiple processing Routs, according to the arbitrary filtering rules and sequence that you define.

So the trade-off between these two options is ease of use versus flexibility. These two options are separate, but not mutually exclusive: You can set up simultaneous data flows through QuickConnect and Data Routes.

You can also move Sources, and their connections to Destinations, between the two UIs. So one obvious option is to configure your first data flow in QuickConnect, and then move to the Routing UI as your familiarity and needs expand.

Here is a lightning-fast (one-minute) preview of **all** the configuration steps below, in QuickConnect:

{{< video 819798662 "daaf31dea3" >}}

## Navigate Between User Interfaces {#switch}

To work in QuickConnect, select the left tile shown above, or open the **Worker Groups** and select **Routing** > **QuickConnect**.

To work in the Routing UI, select the right tile shown above, or open the **Worker Groups** submenu and select **Routing** > **Data Routes** .

## Configure a Source {#source}

A CrowdStream [Source](sources) is a model of an upstream service sending data into the system. In this model, you configure authentication and/or other needed parameters.

You can configure multiple instances of a given Source type, each with different parameters. You can also attach [processing](#connect) that's specific to each instance. CrowdStream provides the [Datagen](sources-datagens) Source type to generate sample inbound events for development and testing.

### Using QuickConnect {#src-qc}

In QuickConnect, select **Add Source** on the left (Sources) side. In the resulting left drawer, search for and select your desired Source type to begin configuring a new instance of that Source.

![Selecting a Source through the QuickConnect UI](crowdstream-source-drawer-4.9.png)
{scale="60%" border="true"}

To configure (or reconfigure) a Source that's already present in QuickConnect, hover over its tile and select **⚙️ Configure**. Note that for multiple Sources of the same type, tiles will display stacked until you hover over the stack.

### Using Data Routes {#src-route}

To use the Routing UI, on the **Worker Groups** submenu select **Data**, then **Sources**. On the resulting **Sources** page, search for your desired Source type, then select its tile to begin configuring a new instance of that Source.

(After you click through to a Source, the same set of peer Sources will be available in the left nav.)

![Sources page](crowdstream-sources-4.9.png)
{border="true"}

Select an existing Source's row here to access its configuration, or select **Add Source** to configure a new instance of that Source.

![Manage Syslog Sources UI](crowdstream-sources-list-4101.png)
{border="true" scale="90%"}

> To get data flowing, jump ahead to [commit and deploy](#commit) your new configuration.
>
{.box .success}

> ##### For More Information
>
> - [Sources](sources)
> - [Using Datagens](datagens)
> - [QuickConnect](quickconnect)
> - [Routes](routes)
>
{.box .success}

## Configure a Destination {#destination}

A CrowdStream [Destination](destinations) is a model of a downstream receiver of events, enabling you to configure authentication and/or other needed parameters.

As with [Sources](#source), you can configure multiple instances of a given Destination type, each with different parameters. You can attach [processing](#connect) that's specific to each instance. CrowdStream provides the following Destinations:
- [CrowdStrike Falcon LogScale](destinations-humio-hec) – Set up your LogScale endpoint, token, and other parameters here.
- [Amazon S3 Compatible Stores](destinations-s3) – Route full-fidelity data to cost-effective storage for later replay and analysis.
- [Data Lakes > Amazon S3](destinations-data-lake-s3) – Store full-fidelity data for later retrieval or analysis. Compatible with [Cribl Search](/search/) (a separate product).
- [DevNull](destinations-devnull) – This output simply consumes and discards events, to facilitate development and testing.

### Using QuickConnect {#dest-qc}

To connect a configured Source to a Destination tile already present on the right, just drag from the Source and drop the connector onto the Destination. A pop-up will prompt you to configure the connection via a [Passthru, Pipeline or Pack](#connect) (explained below).

![Multiple Destinations added to QuickConnect](crowdstream-qc-4.9.png)
{border="true"}

To configure (or reconfigure) the Destination, hover over its tile and select **⚙️ Configure**. For multiple Destinations of the same type, tiles will display stacked until you hover over the stack.

To add a new Destination, select **Add Destination** on the right (Destinations) side. In the resulting right drawer, search for and select your desired Destination type to begin configuring a new instance of that Destination.

![Selecting additional Destinations in QuickConnect](crowdstream-new-dest-4.9.png)
{scale="60%" border="true"}

### Using Data Routes {#dest-route}

To use the Routing UI, on the **Worker Groups** submenu select **Data**, then **Destinations**. On the resulting **Destinations** page, search for your desired Destination type, then select its tile to begin configuring it.

(After you click through to a Destination, the same set of peer Destinations will be available in the left nav.)

![Destinations page](crowdstream-destinations-4.9.png)
{border="true" scale="80%"}

Select an existing Destination's row here to access its configuration, or slect **Add Destination** to configure a new instance of that Destination.

![Manage LogScale Destinations UI](crowdstream-dests-list-4.9.png)
{border="true" scale="90%"}

> To get data flowing, jump ahead to [commit and deploy](#commit) your new configuration.
>
{.box .success}

> ##### For More Information
>
> - See the individual Destination topics linked at the [head](#destination) of this section.
>
{.box .success}

## Connect: Passthru, Pipeline, or Pack {#connect}

In QuickConnect, as soon as you drag and drop a Source ‑> Destination connection, a pop-up dialog will prompt you to select a connection type: **Passthru**, **Pipeline**, or **Pack**. (In Data Routes, you'll make the same choices elsewhere.)

![Connection-type selector in QuickConnect](crowdstream-qc-connection-4.9.png)
{border="true" scale="60%"}

What are these options?

- A **Passthru** establishes a simple direct connection between a Source ‑> Destination pair.

- A [Pipeline](pipelines) hosts a stack of [Functions](functions) to process your data, in the order you arrange them.

- A [Pack](packs) is a microcosm of a whole CrowdStream configuration, designed to share configs between Worker Groups or deployments. A Pack can encapsulate its own Pipelines, Routes, Knowledge libraries, and sample data (but not data Sources or Destinations). A Pack can snap in wherever you can use a Pipeline.

So let's unpack Pipelines, where you'll define most of your event processing.

### Pipeline Options {#pipes}

When you first create a new Pipeline, it's empty, making it equivalent to **Passthru**. CrowdStream provides a set of out-of-the-box Pipelines for specific purposes. As you can see below, one of these is a **Passthru** Pipeline. This similarly contains no Functions – making it the Data Routes UI's equivalent of QuickConnect's **Passthru** button.

For these reasons, it's easy to start with an empty Pipeline, and then add Functions as you need them.

A Pipeline, as you populate it, is a stack of processing Functions and optional comments. Use the accordions on the left to expand or collapse each Function. Use the toggles on the left to enable/disable each Function – preserving structure as you develop and debug.

Use the right **Sample Data** pane to [preview](data-preview) how the Pipeline's current state transforms a set of events. (Controls here enable you to capture live data, save it to sample or [datagen](datagens) files, and reload those files.)

![A Pipeline's Functions stack](crowdstream-pipeline-functions-4.9.png)
{border="true" scale="80%"}

Although you'll configure most data shaping in processing Pipelines between Sources and Destinations, you can also attach a [pre-processing](pipelines#input-conditioning-pipelines) Pipeline to any Source instance, and a [post-processing](pipelines#output-conditioning-pipelines) Pipeline to any Destination instance. Use these options when you need to "condition" the specific data type handled by a particular inbound or outbound integration.

### Connect via QuickConnect {#cx-qc}

To select a connection type in QuickConnect, select the appropriate button in the pop-up dialog shown above. To accept the default **Passthru** option, select **Save**. Your Source and Destination will now be connected.

If you select **Pipeline**, you'll see an **Add Pipeline to Connection** modal. Here, you can choose an existing Pipeline, or select **Add Pipeline** to define a new one. Then select **Save** here to establish the connection.

Selecting **Pack** opens an **Add Pack to Connection** modal that works exactly the same way.

Whichever option you choose here, your connection between this Source and Destination is now complete.

> To get data flowing, jump ahead to [commit and deploy](#commit) your new configuration.
>
{.box .success}

### Connect via Data Routes {#cx-route}

The Data Routes UI enables you to finely configure multiple, interdependent connections between Sources and Destinations. Events clone and cascade across a [Routing table](routes#how-do-routes-work), based on the filtering rules and sequence you define.

In exchange for this flexibility, you sacrifice QuickConnect's one-stop, snap-together UI metaphor. After configuring your Sources and Destinations – which you've already done above – you need to connect them by separately setting up at least one Pipeline and one Route.

### Pipelines Access {#routes-pipes}

On the **Worker Groups** submenu, select **Processing**, then **Pipelines**. In addition to the minimal **Passthru** Pipeline identified [above](#pipes), other simple building blocks here include **main** (which initially contains only a simple [Eval Function](eval-function)) and **devnull** (which contains a single Function that just [drops events](drop-function)).

Out-of-the-box Pipelines are already connected to Routes. On the **Pipelines** page, the **Route/Input** columns displays the connected Route, as shown here. But as our final connection step, let's next look at how to connect up everything (Source, Pipeline, Destination) in a Route.

![Checking Pipelines Route connections](crowdstream-connected-routes-4.9.png)
{border="true" scale="40%"}

> ##### For More Information
>
> - [Refine Data with Functions](getting-started-guide#refine-data-with-functions) walks you through configuring CrowdStream's built-in Pipeline Functions to perform common processing tasks like parsing, masking, adding, deleting, and renaming event fields.
>
{.box .success}

### Configure a Route {#routes-config}

In the Data Routes UI, a Route is where you complete a data-flow connection. Each Route connects exactly one Pipeline with exactly one Destination.

This combination can consume data from one, multiple, or all the Sources you've configured on the Data Routes side. (QuickConnect Sources are separate, but you can open those Sources' configs to move them to the Data Routes UI.) This is governed by the [filtering](#routes-filter) we'll see below.

To open the Routing table, on the **Worker Groups** submenu, select **Routing**, then **Data Routes**.

CrowdStream ships with one starter Route named **default**. It's configured as follows:

- The **Filter** expression is set to **true**. This simple, default expression makes data from **all** active Sources available to this Route. (See finer filtering options [below](#routes-filter).)

- The **Pipeline** drop-down is where you set the 1:1 Pipeline:Route connection. This starter Route's Pipeline defaults to **main**, but you can select any one other configured Pipeline.

- The **Destination** drop-down defines the single Destination for this Pipeline:Route connection. If this isn't already set to your LogScale Destination, you'll most likely want to select that here. (Note that each option displayed on this list corresponds to the unique, arbitrary **Output ID** set on one **instance** of a Destination type. So some names might initially be unfamiliar.)

![Changing a Route's Destination](crowdstream-default-route-4120.png)
{border="true"}

> To get data flowing, jump ahead to [commit and deploy](#commit) your new configuration.
>
{.box .success}

### Filter a Route's Input Data {#routes-filter}

When you want a Route to ingest data from only one Source, or a subset of Sources, you expand its **Filter** expression beyond the default **true** catch-all.

**Filter** is itself a drop-down. Open it to select among typeahead suggestions for several common inbound data types.

![Tuning a Route's Filter expression](crowdstream-route-filter-4120.png)
{border="true"}

You can select one of these expressions and then modify it, using the same `__inputId` syntax. In the example expression below, note that the identifier after the `:` is – as with the **Destination** options we saw above – the arbitrary name of one Syslog Source instance:

`__inputId.startsWith('syslog:in_syslog:')`

> ##### For More Information
>
> - [Build Custom Logic to Route and Process Your Data: Filter Expressions](filter-and-transform-data#filters)
{.box .success}

## Commit/Deploy Config Changes {#commit}

To get data flowing as you expect, you'll always need to use CrowdStream's built-in Git version-control integration to commit your configuration changes and deploy them to your event-processing Worker instances. After any significant configuration change, you'll be prompted with Group-level **Commit & Deploy** button at the upper right

Depending on your [version-control settings](version-control#collapse-actions), **Commit** and **Deploy** might appear as separate buttons.

Selecting either flavor of the **Commit** button will display a confirmation modal like this. You can either accept the default commit message, or enter a specific message. Then be sure to select the appropriate buttons to both commit **and** deploy your changes.

![Confirming a commit](crowdstream-commit-changes-4.9.png)
{border="true" scale="60%"}

Data should now start flowing through the Workers. (You can check this by selecting the top nav's **Monitoring** link. This takes you to read-only dashboards, so remember to select **Manage** when you want to return to configuration options.)

In return for clearing all the red-dot prompts, your reward will be one more red dot, on the top nav's CrowdStream-wide Version Control button. This is because CrowdStream sees the preceding deploy to Workers as a change to record (commit) on the **Leader's** configuration.

![Leader commit prompt](crowdstream-leader-commit-button-4.9.png)
{border="true" scale="50%"}

This top-level commit isn't necessary to enable data flow, but it backs up and safeguards your overall configuration. Select the button to display one final confirmation prompt:

![Confirming a commit on the Leader](crowdstream-confirm-leader-commit-4.9.png)
{border="true" scale="50%"}

Select **Commit** here, and your configuration is complete!

> ##### For More Information
>
> - [Version Control > Committing Changes](version-control#committing)
> - [Version Control > Collapse Actions](version-control#collapse-actions)
> - [Monitoring](monitoring)
>
{.box .success}

## Moving Ahead with CrowdStream {#next}

To quote a fundamental treatise on SecOps: you know all the rules by now, and the fire from the ice. To delve deeper into CrowdStream's data shaping and routing options, follow these links to conceptual overview and reference topics.

> ##### For More Information
>
> - [Cribl Stream Basic Concepts](basic-concepts)
> - [Event Model](event-model)
> - [Pipeline Data Preview](data-preview)
> - [Functions](functions)
> - [Knowledge Objects](knowledge-library)
> - [Distributed Quick Start](distributed-guide#add-a-pipeline) – This tutorial's middle sections walk you through configuring Pipelines, Functions, and Routes, and scaling Workers, via the Routing UI.
> - [Techniques & Tips](tips)
> - [Setup Guides](usecase-edge-stream)
> - [Expressions and other references](reference)
>
{.box .success}
