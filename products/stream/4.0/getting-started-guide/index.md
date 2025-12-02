# Getting Started Guide


This guide walks you through planning, (optionally) installing, and configuring a basic deployment of Cribl Stream. You'll capture some realistic sample log data, and then use Cribl Stream's built-in Functions to redact, parse, refine, and shrink the data. 

By the end of this guide, you'll have assembled all of Cribl Stream's basic building blocks: a Source, Route, Pipeline, several Functions, and a Destination. You can complete this tutorial using Cribl Stream's included sample data, without connections to – or licenses on – any inbound or outbound services.

Assuming a cold start (from first-time setup of a Cribl.Cloud or self-hosted instance), this guide might take about an hour. But you can work through it in chunks, and Cribl Stream will persist your work between sessions.

> If you've already launched a Cribl Stream instance (either Cloud or self-hosted), skip ahead to [Get Data Flowing](#get-data-flowing).
> 
> Once you've mastered all the techniques in this tutorial, check out its [Distributed Quick Start](distributed-guide) successor.
>
{.box .success}

## Cloud or Self-Hosted? {#2-paths}

To quote Robert Frost and Robert Plant (of Led Zeppelin), there are two paths that you can go by:

- Do this tutorial with a free Cribl.Cloud instance, hosted by Cribl. Follow the registration instructions in the [section just below](#cloud). This skips you past all the requirements and installation sections below – you'll have nothing to install or host. Because Cribl.Cloud always runs in [distributed mode](deploy-distributed), this will require a few extra clicks (all clearly labeled) later in the tutorial's body. But it will give you immediate experience with Cribl Stream's typical production mode.
- Do this tutorial by downloading and installing Cribl Stream software on your own hardware or virtual machine. Follow the [Requirements](#requirements) and [Download and Install](#install) instructions below. You'll need to provide your own infrastructure. But if you're planning to use self-hosted/on-prem Cribl Stream in production, this will walk you through its realistic setup.

  > To fully experience a real-world, self-hosted production setup, you can switch your Cribl Stream installation to [distributed mode](deploy-distributed). And then chase this tutorial with our [Distributed Quick Start](distributed-guide), which fully exercises distributed mode's multiple Workers and Worker Groups.
  {.box .success}

## Quick Start with a Cloud Instance {#cloud}

As indicated just above, you can skip installing Cribl Stream software – and skip this tutorial's next several sections – by registering a free Cribl.Cloud instance. Cribl will quickly spin up a fully functioning copy of Cribl Stream for you, and manage it on your behalf. To use this fastest option:

1. In the Cribl.Cloud Launch Guide, follow the four steps listed in [Registering a Cribl.Cloud Portal](deploy-cloud#registering-a-criblcloud-portal).
2. In the same Cribl.Cloud Launch Guide, jump ahead to follow the four steps listed in [Managing Cribl.Cloud](deploy-cloud#managing).
3. That's it! Come right back to this tutorial, and skip ahead to [Get Data Flowing](#get-data-flowing).

> Really, skip all the other sections of the linked Cloud Guide, and all this tutorial's sections between here and [Get Data Flowing](#get-data-flowing). We told you this was a quick start!
>
{.box .success}

## Requirements for Self-Hosted Cribl Stream {#requirements}

The minimum requirements for running this tutorial are the same as for a Cribl Stream production [single-instance deployment](deploy-single-instance#requirements).

### OS (Intel Processors)
   - Linux 64-bit kernel >= 3.10 and glibc >= 2.17
   - Examples: Ubuntu 16.04, Debian 9, RHEL 7, CentOS 7, SUSE Linux Enterprise Server 12+, Amazon Linux 2014.03+

### OS (ARM64 Processors)
   - Linux 64-bit 
   - Tested so far on Ubuntu (14.04, 16.04, 18.04, and 20.04), CentOS 7.9, and Amazon Linux 2

### System
   * +4 physical cores, +8GB RAM – all beyond your basic OS/VM requirements
   * 5GB free disk space (more if [persistent queuing](persistent-queues) is enabled)

> We assume that 1 physical core is equivalent to 2 virtual/​hyperthreaded CPUs (vCPUs) on Intel/Xeon or AMD processors; and to 1 (higher-throughput) vCPU on Graviton2/ARM64 processors.
>
{.box .info}

### Browser Support

  * Firefox 65+, Chrome 70+, Safari 12+, Microsoft Edge

### Network Ports {#ports}

By default, Cribl Stream listens on the following ports:

Component | Default Port
--- | ---
UI default | `9000`
HTTP Inbound, default | `10080`
User options | + Other data ports as required.

You can [override](deploy-single-instance#ports-override) these defaults as needed.

## Plan for Production

For higher processing volumes, users typically enable Cribl Stream's [Distributed Deployment](https://docs.cribl.io/docs/deploy-distributed) option. While beyond the scope of this tutorial, that option has a few additional requirements, which we list here for planning purposes:

* Port `4200` must be available on the Leader Node for Workers' communications.
* Git  (1.8.3.1 or higher) must be installed on the Leader Node, to manage configuration changes.

See [Sizing and Scaling](https://docs.cribl.io/docs/scaling) for further details about configuring Cribl Stream to handle large data streams.

## Download and Install Cribl Stream {#install}

To avoid permissions errors, you should both install and run (next section) Cribl Stream as the same Linux user. For details on creating a new user (addressing both systemd and initd distro's), see [Enabling Start on Boot](deploy-single-instance#boot-start).

Download the latest version of Cribl Stream at https://cribl.io/download/. 

Un-tar the resulting `.tgz` file in a directory of your choice (e.g., `/opt/`). Here's general syntax, then a specific example:

```shell
tar xvzf cribl-<version>-<build>-<arch>.tgz
tar xvzf cribl-3.5.0-fa5eb040-linux-x64.tgz
```

You'll now have Cribl Stream installed in a `cribl` subdirectory, by default: `/opt/cribl/`. We'll refer to this `cribl` subdirectory throughout this documentation as `$CRIBL_HOME`.

## Run Cribl Stream {#running} 

In your terminal, switch to the `$CRIBL_HOME/bin` directory (e.g,: `/opt/cribl/bin`). Here, you can start, stop, and verify the Cribl Stream server using these basic `./cribl` CLI commands:

- **Start**: `./cribl start`
- **Stop**: `./cribl stop`
- **Get status**: `./cribl status`

> For other available commands, see [CLI Reference](cli-reference).
>
{.box .success}

Next, in your browser, open `http://<hostname>:9000` (e.g., `http://localhost:9000`) and log in with default credentials (`admin`, `admin`). 

Register your copy of Cribl Stream when prompted.

After registering, you'll be prompted to change the default password.

That's it!

## Get Data Flowing

With Cribl Stream now running – either in Cribl.Cloud or in your self-hosted copy – you're ready to configure a working Cribl Stream deployment. You'll set up a [Source](sources), [Destination](destinations), [Pipeline](pipelines), and [Route](routes), and will assemble several built-in [Functions](functions) to refine sample log data.

### Add a Source {#source}

Each Cribl Stream Source represents a data input. Options include Splunk, Elastic Beats, Kinesis, Kafka, syslog, HTTP, TCP JSON, and others. 

For this tutorial, we'll enable a Cribl Stream built-in datagen (i.e., data generator) that generates a stream of realistic sample log data.

![Adding a datagen Source](source-add-datagen.62cbe10536.png)
{border="true"}

> If you're on Cribl.Cloud or any other distributed mode, first click the top nav's **Manage** tab to select the `default` (or another) Worker Group.
>
{.box .success}

1. From Cribl Stream's **Manage** submenu, select **Data** > **Sources**. 

1. From the **Data Sources** page's tiles or left menu, select **Datagen**. 

    (You can use the search box to jump to the **Datagen** tile.)

1. Click **New Source** to open the **New Source** modal.

1. In the **Input ID** field, name this Source `businessevent`.

1. In the **Data Generator File** drop-down, select `businessevent.log`. 

    This generates...log events for a business scenario. We'll look at their structure shortly, in [Capture and Filter Sample Data](#capture).

1. Click **Save**. 

> If you're on Cribl.Cloud or any other distributed mode, click **Commit & Deploy** at Cribl Stream's upper right before proceeding. Then, in the resulting dialog box, click **Commit & Deploy** to confirm. You'll see a **Commit successful** message.
>
{.box .success}

The **Yes** slider in the **Enabled** column indicates that your Datagen Source has started generating sample data. {{< id `datagen-status` >}}

![Configuring a datagen Source](source-configure-datagen.f65e0e6051.png)

###  Add a Destination  {#destination}

Each Cribl Stream Destination represents a data output. Options include Splunk, Kafka, Kinesis, InfluxDB, Snowflake, Databricks, TCP JSON, and others. 

For this tutorial, we'll use Cribl Stream's built-in **DevNull** Destination. This simply discards events – not very exciting! But it simulates a real output, so it provides a configuration-free quick start for testing Cribl Stream setups. It's ideal for our purposes.

To verify that **DevNull** is enabled, let's walk through setting up a Destination, then setting it up as Cribl Stream's default output:
> On Cribl.Cloud or any other distributed mode, first click the top nav's **Manage** tab to select the `default` (or another) Worker Group.
>
{.box .success}

1. From Cribl Stream's top menu, select **Data** > **Destinations**. 

2. From the **Data Destinations** page's tiles or left menu, select **DevNull**.

    (You can use the search box to jump to the **DevNull** tile.)

3. On the resulting **devnull** row, look for the **Live** indicator under **Status**. This confirms that the **DevNull** Destination is ready to accept events.

4. From the **Data Destinations** page's left nav, select the **Default** Destination at the top.

5. On the resulting **Manage Default Destination** page, verify that the **Default Output ID** drop-down points to the **devnull** Destination we just examined.

> If you're on Cribl.Cloud or any other distributed mode, click **Commit & Deploy** at Cribl Stream's upper right (if available) before proceeding. Then, in the resulting dialog box, click **Commit & Deploy** to confirm. You'll see a **Commit successful** message. 
>
{.box .success}

We've now set up data flow on both sides. Is data flowing? Let's check.

### Monitor Data Throughput

Click the top nav's **Monitoring** tab. This opens a summary dashboard, where you should see a steady flow of data in and out of Cribl Stream. The left graph shows events in/out. The right graph shows bytes in/out.

![Monitoring dashboard](monitoring-page-3.1.20367fdbe1.png)
{border="true"}

[Monitoring](monitoring) displays data from the preceding 24 hours. You can use the **Monitoring** submenu to open detailed displays of Cribl Stream components, [collection jobs](collectors) and tasks, and Cribl Stream's own internal logs. Click **Sources** on the lower submenu to switch to this view:

![Monitoring Sources](monitoring-sources.bf87d2d020.png)
{border="true"}

This is a compact display of each Source's inbound events and bytes as a sparkline. You can click each Source's Expand button (highlighted at right) to zoom up detailed graphs. 

Click **Destinations** on the lower submenu. This displays a similar sparklines view, where you can confirm data flow out to the **devnull** Destination:

![Monitoring Destinations](monitoring-destinations.ea2322f1eb.png)
{border="true"}

With confidence that we've got data flowing, let's send it through a Cribl Stream Pipeline, where we can add Functions to refine the raw data.

##  Create a Pipeline  {#pipeline}

A [Pipeline](pipelines) is a stack of Cribl Stream Functions that process data. Pipelines are central to refining your data, and also provide a central Cribl Stream workspace – so let's get one going.

> On Cribl.Cloud or any other distributed mode, first click the top nav's **Manage** tab to select the `default` (or another) Worker Group.
>
{.box .success}

1. From the top menu, select **Processing** > **Pipelines**. 

    You now have a two-pane view, with a **Pipelines** list on the left and **Sample Data** controls on the right. (We'll capture some sample data momentarily.)

2. At the **Pipelines** pane's upper right, click **+ Pipeline**, then select **Create Pipeline**.

3. In the new Pipeline's **ID** field, enter a unique identifier. (For this tutorial, you might use `slicendice`.)

4. Optionally, enter a **Description ** of this Pipeline's purpose. 

5. Click **Save**.

Now scroll through the right **Preview** pane. Depending on your data sample, you should now see multiple events struck out and faded – indicating that Cribl Stream will drop them before forwarding the data.

> If you're on Cribl.Cloud or any other distributed mode, click **Commit & Deploy** at Cribl Stream's upper right before proceeding. Then, in the resulting dialog box, click **Commit & Deploy** to confirm. You'll see a **Commit successful** message. 
>
{.box .success}

Your empty Pipeline now prompts you to preview data, add Functions, and attach a Route. So let's capture some data to preview.

![Pipeline prompt to add Functions](pipeline-prompt.d206559d1f.png)

## Capture and Filter Sample Data {#capture}

The right **Sample Data** pane provides multiple tools for grabbing data from multiple places (inbound streams, copy/paste, and uploaded files); for previewing and testing data transformations as you build them; and for saving and reloading sample files.

Since we've already got live (simulated) data flowing in from the datagen Source we built, let's grab some of that data.

#### Capture New Data

1. In the right pane, click **Capture New**.

1. Click **Capture**, then accept the drop-down's defaults – click **Start**.

1. When the modal finishes populating with events, click **Save as Sample File**.

1. In the **SAMPLE FILE SETTINGS** fly-out, change the generated **File Name** to a name you'll recognize, like `be_raw.log`.

1. Click **Save**. This saves to the **File Name** you entered above, and closes the modal. You're now previewing the captured events in the right pane. (Note that this pane's **Preview Simple** tab now has focus.)

1. Click the **Show more** link to expand one or more events.

By skimming the key-value pairs within the data's `_raw` fields, you'll notice the scenario underlying this preview data (provided by the `businessevents.log` datagen): these are business logs from a mobile-phone provider. 

To set up our next step, find at least one `marketState` K=V pair. Having captured and examined this raw data, let's use this K=V pair to crack open Cribl Stream's most basic data-transformation tool, Filtering.

#### Filter Data and Manage Sample Files

1. Click the right pane's **Sample Data** tab.

2. Again click **Capture New**.

3. In the **Capture Sample Data** modal, replace the **Filter Expression** field's default `true` value with this simple regex: `_raw.match(/marketState=TX/)` 

    We're going to Texas! If you type this in, rather than pasting it, notice how Cribl Stream provides typeahead assist to complete a well-formed JavaScript expression.
    
    You can also click the Expand button at the **Filter Expression** field's right edge to open a modal to validate your expression. The adjacent drop-down enables you to restore previously used expressions.

    ![Expand button and history drop-down](filter-widgets.a4e42bb4f4.png)

4. Click **Capture**, then **Start**. 

   Using the **Capture** drop-down's default limits of 10 seconds and 10 events, you'll notice that with this filter applied, it takes much longer for Cribl Stream to capture 10 matching events.

5. Click **Cancel** to discard this filtered data and close the modal.

6. On the right pane's **Sample Data** tab, click **Simple** beside `be_raw.log`. 

This restores our preview of our original, unfiltered capture. We're ready to transform this sample data in more interesting ways, by building out our Pipeline's Functions.

## Refine Data with Functions

[Functions](functions) are pieces of JavaScript code that Cribl Stream invokes on each event that passes through them. By default, this means all events – each Function has a **Filter** field whose value defaults to `true`. As we just saw with data capture, you can replace this value with an expression that scopes the Function down to particular matching events.

In this Pipeline, we'll use some of Cribl Stream's core Functions to:

- Redact (mask) sensitive data
- Extract (parse) the `_raw` field's key-value pairs as separate fields.
- Add a new field.
- Delete the original `_raw` field, now that we've extracted its contents.
- Rename a field for better legibility.

#### Mask: Redact Sensitive Data

In the right **Preview** pane, notice each that event includes a **social** key, whose value is a (fictitious) raw Social Security number. Before this data goes any further through our Pipeline, let's use Cribl Stream's `Mask` Function to swap in an md5 hash of each SSN.

1. In the left **Pipelines** pane, click **+ Function**.  

2. Search for `Mask`, then click it. 

3. In the new Function's **Masking Rules**, click the into **Match Regex** field. 

   Now we want to build the **Match Regex**/**Replace Expression** row shown just below.

4.  Enter or paste this regex, which simply looks for the string `social=`, followed by any digits: `(social=)(\d+)` 

5. In **Replace Expression**, paste the following hash function. The backticks are literal: ``` `${g1}${C.Mask.md5(g2)}` ```

6. Note that **Apply to Fields** defaults to `_raw`. This is what we want to target, so we'll accept this default.

7. Click **Save**.

![Entering a Masking Rule to hash Social Security numbers](mask-fn-regex-replace.b6aed8518d.png)
{border="true"}

You'll immediately notice some obvious changes: 

- The **Preview** pane has switched from its **IN** to its **OUT** tab, to show you the outbound effect of the Pipeline you just saved.

- Each event's `_raw` field has changed color, to indicate that it's undergone some redactions.

Now locate at least one event's **Show more** link, and click to expand it. You can verify that the `social` values have now been hashed.

![Mask Function and hashed result](mask-fn-result.1596d2ba5a.png)
{border="true"}

#### Parser: Extract Events

Having redacted sensitive data, we'll next use a Parser function to lift up all the `_raw` field's key-value pairs as fields:

1. In the left **Pipelines** pane, click **+ Function**.

2. Search for `Parser`, then click it. 

3. Leave the **Operation Mode** set to its `Extract` default.

4. Set the **Type** to `Key=Value Pairs`. 

5. Leave the **Source Field** set to its `_raw` default.

6. Click **Save**.

![Parser configured to extract K=V pairs from `_raw`](parser-fn-config.a5a8559fc8.png)
{border="true"}

You should see the **Preview** pane instantly light up with a lot more fields, parsed from `_raw`. You now have rich structured data, but not all of this data is particularly interesting: Note how many fields have `NA` ("Not Applicable") values. We can enhance the **Parser** Function to ignore fields with `NA` values. 

1. In the Function's **Fields Filter Expression** field (near the bottom), enter this negation expression: `value!='NA'`.

    Note the single-quoted value. If you type  (rather than paste) this expression, watch how typeahead matches the first quote you type.

2. Click **Save**, and watch the **Preview** pane.

![Filtering the Parser Function to ignore fields with 'NA' values](parse-exclude-fields.01abb91490.png)

Several fields should disappear – such as `credits`, `EventConversationID`, and `ReplyTo`. The remaining fields should display meaningful values. Congratulations! Your log data is already starting to look better-organized and less bloated.

> ##### Missed It?
>
> If you didn't see the fields change, slide the Parser Function **Off**, click **Save** below, and watch the **Preview** pane change. Using these toggles, you can preserve structure as you test and troubleshoot each Function's effect. 
> 
> Note that each Function also has a **Final** toggle, defaulting to **Off**. Enabling **Final** anywhere in the Functions stack will prevent data from flowing to any Functions lower in the UI.
> 
> Be sure to toggle the Function back **On**, and click **Save** again, before you proceed!
>
{.box .success}

![Toggling a Function off and on](function-state-toggle.996abc37ad.png)
{border="true"}

Next, let's add an extra field, and conditionally infer its value from existing values. We'll also remove the `_raw` field, now that it's redundant. To add and remove fields, the **Eval** Function is our pal.

#### Eval: Add and Remove Fields {#eval}

Let's assume we want to enrich our data by identifying the manufacturer of a certain popular phone handset. We can infer this from the existing `phoneType` field that we've lifted up for each event.

##### Add Field (Enrich)

1. In the left **Pipelines** pane, click **+ Function**.

2. Search for `Eval`, then click it. 

3. Click **+ Add Fields** to open the **Evaluate Fields** table.

    Here you add new fields to events, defining each field as a key-value pair. If we needed more key-value pairs, we could click `+ Add Field` for more rows.

4. In the table's first row, click into the **Name** field and enter: `phoneCompany`.

5. In the adjacent **Value Expression** field, enter this JS ternary expression that tests `phoneType`'s value:
`phoneType.startsWith('iPhone') ? 'Apple' : 'Other'` (Note the `?` and `:` operators, and the single-quoted values.)

6. Click **Save**. Examine some events in the **Preview** pane, and each should now contain a  `phoneCompany` field that matches its `phoneType`.

![Adding a field to enrich data](eval-fn-enrich.c2b12e20db.png)
{border="true"}

##### Remove Field (Shrink Data)

Now that we've parsed out all of the `_raw` field's data – it can go. Deleting a (large) redundant field will give us cleaner events, and reduced load on downstream resources.

1. Still in the **Eval** Function, click into **Remove Fields**.

2. Type: `_raw` and press **Tab** or **Enter**.

3. Click **Save**.

The **Preview** pane's diff view should now show each event's `_raw` field stripped out.

![Removing a field to streamline data](eval-remove-field.ce1e8c0bc3.png)
{border="true"}

Our log data has now been cleansed, structured, enriched, and slimmed-down. Let's next look at how to make it more legible, by giving fields simpler names.

#### Rename: Refine Field Names

1. In the left **Pipelines** pane, click `+ Function`. 
 
    This rhythm should now be familiar to you.

2. Search for `Rename`, then click it. 

3. Click **+ Add fields** to open the **Rename fields** table.

4. Click into the new Function's **Rename fields** table. 

    This has the same structure you saw above in [Eval](#eval): Each row defines a key-value pair.

5. In **Current name**, enter the longhaired existing field name: `conversationId`.

6. In **New name**, enter the simplified field name: `ID`.

7. Watch any event's `conversationId` field in the **Preview** pane as you click **Save** at left. This field should change to `ID` in all events.

#### Drop: Remove Unneeded Events

We've already refined our data substantially. To further slim it down, a Pipeline can entirely remove events that aren't of interest for a particular downstream service.

> As the "Pipeline" name implies, your Cribl Stream installation can have multiple Pipelines, each configured to send out a data stream tailored to a particular Destination. This helps you get the right data in the right places most efficiently.
>
{.box .success}

Here, let's drop all events for customers who use prepaid monthly phone service (i.e., **not** postpaid):

1. In the left **Pipelines** pane, click **+ Function**.

2. Search for `Drop`, then click it. 

3. Click into the new Function's **Filter** field.

4. Replace the default `true` value with this JS negation expression: `accountType!='PostPaid'`

5. Click **Save**.

Now scroll through the right **Preview** pane. Depending on your data sample, you should now see multiple events struck out and faded – indicating that Cribl Stream will drop them before forwarding the data.

#### A Second Look at Our Data

Torture the data enough, and it will confess. By what factor have our transformations refined our data's volume? Let's check.

In the right **Preview** pane, click the **Basic Statistics** button:

![Displaying Basic Statistics](basic-statistics-button.320ade35ce.png)
{border="true"}

Even without the removal of the `_raw` field (back in [Eval](#eval)) and the dropped events, you should see a substantial % reduction in the **Full Event Length**.

![Data reduction quantified](basic-statistics-table.0cfde72c4b.png)
{border="true"}

Woo hoo! Before we wrap up our configuration: If you're curious about individual Functions' independent contribution to the data reduction shown here, you can test it now. Use the toggle **Off** > **Save** > **Basic Statistics** sequence to check various changes.

##  Add and Attach a Route  {#route}

We've now built a complete, functional Pipeline. But so far, we've tested its effects only on the static data sample we captured earlier. To get dynamic data flowing through a Pipeline, we need to filter that data in, by defining a Cribl Stream [Route](routes).

1. At the **Pipelines** page's top left, click **Attach to Route**. 

    This displays the **Routes** page. It's structured very similarly to the **Pipelines** page, so the rhythm here should feel familiar.

2. Click `+ Route`.

3. Enter a unique, meaningful **Route Name**, like `demo`.

4. Leave the **Filter** field set to its `true` default, allowing it to deliver all events. 

    Because a Route delivers events to a Pipeline, it offers a first stage of filtering. In production, you'd typically configure each Route to filter events by appropriate `source`, `sourcetype`, `index`, `host`,  `_time`, or other characteristics. The **Filter** field accepts JavaScript expressions, including AND (`&&`) and OR (`||`) operators.

5. Set the **Pipeline** drop-down to our configured `slicendice` Pipeline.

6. Leave the **Enable Expression** field set to its **No** default.
   Toggling this field to **Yes** changes the **Output** field to an **Output Expression** field where you can enter a JavaScript expression for your **Destination** name.

7. Set the **Output** drop-down to either `devnull` or `default`. 

    This doesn't matter, because we've set `default` as a pointer to `devnull`. In production, you'd set this carefully.

8. You can leave the **Description** empty, and leave **Final** set to **Yes**.

9. Grab the new Route by its left handle, and drag it above the `default` Route, so that our new Route will process events first. You should see something like the screenshot below.

10. Click **Save** to save the new Route to the Routing table.

> If you're on Cribl.Cloud or any other distributed mode, click **Commit & Deploy** at Cribl Stream's upper right before proceeding. Then, in the resulting dialog box, click **Commit & Deploy** to confirm. You'll see a **Commit successful** message. 
>
{.box .success}

![Configuring and adding a Route](route-move.0a66617c31.png)

The sparklines should immediately confirm that data is flowing through your new Route:

![Live Routes](route-sparklines-fx.79a6bb3a43.png)
{border="true"}

To confirm data flow through the whole system we've built, select **Monitoring > Data > Routes** and examine `demo`.

![Monitoring data flow through Routes](monitor-routes.d3e7b34731.png)
{border="true"}

Also select **Monitoring > Data > Pipelines** and examine `slicendice`.

![Monitoring data flow through Pipelines](monitor-pipelines.d4668f5573.png)
{border="true"}

## What Have We Done?

Look at you! Give yourself a pat on the back! In this short, scenic tour – with no hit to your cloud-services charges – you've built a simple but complete Cribl Stream system, exercising all of its basic components:

- Downloaded, installed, and run Cribl Stream.
- Configured a Source to hook up an input.
- Configured a Destination to feed an output.
- Monitored data throughput, and checked it twice.
- Built a Pipeline.
- Configured Cribl Stream Functions to redact, parse, enrich, trim, rename, and drop event data.
- Added and attached a Route to get data flowing through our Pipeline.

## Next Steps

Interested in guided walk-throughs of more-advanced Cribl Stream features? We suggest that you next check out these further resources.

- [Cribl Stream Sandbox](https://cribl.io/try-cribl): Work through general and specific scenarios in a free, hosted environment, with terminal access and real data inputs and outputs.

- [Distributed Quick Start](distributed-guide): Building on this tutorial that you've just completed, launch and configure a Cribl Stream distributed deployment. You'll work with a small but realistic model of a fully scaleable production deployment.

- [Use Cases](usecase-ingest-time-fields) documentation: Bring your own services to build solutions to specific challenges.

- [Cribl Concept: Pipelines](https://vimeo.com/442794849) – Video showing how to build and use Pipelines at multiple Cribl Stream stages.

- [Cribl Concept: Routing](https://vimeo.com/441168083) – Video about using Routes to send different data through different paths.

## Cleaning Up

Oh yeah, you've still got the Cribl Stream server running, with its `businessevent.log` datagen still firing events. If you'd like to shut these down for now, in reverse order:

1. Go to **Data > Sources > Datagen**.

2. Slide `businessevent` to **Off**, and click **Save**. (Refer back to the screenshot [above](#datagen-status).)

3. In your terminal's `$CRIBL_HOME/bin` directory, shut down the server with: `./cribl stop`

That's it! Enjoy using Cribl Stream.
